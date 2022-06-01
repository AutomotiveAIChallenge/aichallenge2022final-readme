# Error resolution

## Errors at "setup_ubuntu20.04.sh"

- **TASK [cuda : CUDA (add CUDA repository into sources.list)]でfail**
  
  ![image](https://user-images.githubusercontent.com/45618513/171146891-d3b7c601-cf47-405e-b80a-ac8af2b103a7.png)

  The following command resolved this issue.
  ```
  sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
  ```
  
 

- **Fail at TASK [cuda : install software-properties-common]**

  ![Screenshot from 2022-05-16 10-49-50](https://user-images.githubusercontent.com/67366636/169556134-d19a0103-1f23-44c6-b31c-2181e83f56eb.png)

  The cause is a key update in the NVIDIA repository. The following command resolved the issue.
  ```
  sudo apt-key del 7fa2af80
  wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
  sudo dpkg -i cuda-keyring_1.0-1_all.deb 
  sudo apt update
  ```
  - [NVIDIA website](https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/)
  - [Japanese Site](https://www.nemotos.net/?p=5178)

  ※If the site's procedures and error solutions do not work, 、Add XXXXXXX public key
  ```  
  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys XXXXXX 
  ```
  
- **Fail at TASK [tensorrt : TensorRT (install libraries for TensorRT 7)]**

  ![Screenshot from 2022-05-23 23-12-47](https://user-images.githubusercontent.com/67366636/169840548-078d9650-a2e5-48ae-b474-89008d1e48d8.png)
  
  The cause is that the version of CUDA is different from the required version. In my case, the following procedure solved the problem.
  
  ※cuda: 11.1, cudnn: 8.0.5, TensorRT: 7.2.1
  
  ```  
  sudo apt remove nvidia-*
  sudo dpkg --remove --force-remove-reinstreq XXXXXXX　※Forcibly remove the not upgraded package in the lower part of the error message in the attached image.
  ```
  
- **Fail at TASK [libtorch : Libtorch (unarchive)]**

  ![Screenshot from 2022-05-23 23-25-46](https://user-images.githubusercontent.com/67366636/169854877-9bc3b1fa-46de-42c5-80d1-0ff57a01ab56.png)
  
  The following procedure resolves the problem.
  
  1. Download [this package](https://drive.google.com/u/0/uc?id=1eNh3F3xCQ4AMJEHtwb1dhshSyzWMjoc8) 
  2. Rename the downloaded zip file to **libtorch.zip**.
  3. Move libtorch.zip to /tmp
  4. Comment out 2~5 lines in　/pilot-auto.test.x1.aichallenge/ansible/roles/libtorch/tasks/main.yaml
  
      <img src="https://user-images.githubusercontent.com/67366636/169856701-2b06197a-9d92-4987-aeb1-b906f903bb33.png" width="75%">

## Errors at "colcon build"
- **Failed  <<<  tier4_planning_rviz_plugin**

  ![Screenshot from 2022-05-24 08-53-40](https://user-images.githubusercontent.com/67366636/169956833-74c7c729-7284-4806-a43c-47b2fbb67adf.png) 
  
  Change the following in　/pilot-auto.test.x1.aichallenge/src/autoware/universe/common/tier4_planning_rviz_plugin/src/tools/jsk_overlay_utils.hpp
  
  ```  
  #include <QImage>
          ↓Change to the following
  #include <QCursor>
  #include <QImage>
  #include <QVariant>
  ```  

## Errors at "PSim / LSim"
- **Error at [rmw_cyclonedds_cpp]**

  ![Screenshot from 2022-05-26 22-40-13](https://user-images.githubusercontent.com/67366636/170617250-f743cbc1-0e9a-4915-84aa-688fdf49a7b1.png)

  1. Change the DDS to be used
  
  ```  
  export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
  sudo apt install ros-foxy-rmw-cyclonedds-cpp
  ```  

  2. Setting up cyclonedds to do multicast on localhost

    The file can be placed anywhere, but let's place it in /opt/autoware/cyclonedds_config.xml and copy & paste the following contents

  ```  
  <?xml version="1.0" encoding="UTF-8" ?>
  <CycloneDDS xmlns="https://cdds.io/config" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://cdds.io/config https://raw.githubusercontent.com/eclipse-cyclonedds/cyclonedds/master/etc/cyclonedds.xsd">
    <Domain id="any">
      <General>
        <NetworkInterfaceAddress>lo</NetworkInterfaceAddress>
      </General>
      <Internal>
        <MinimumSocketReceiveBufferSize>10MB</MinimumSocketReceiveBufferSize>
      </Internal>
    </Domain>
  </CycloneDDS>
  ```


  3. After creating the file, add the following to ~/.bashrc

  ```
  export CYCLONEDDS_URI=file:///opt/autoware/cyclonedds_config.xml
  ```


  4. Change maximum receive buffer size (required on every restart)
 
  ```
  sudo sysctl -w net.core.rmem_max=2147483647
  ```
  
  5. Multicast support for localhost (required on every reboot)
  
  ```
  sudo ifconfig lo multicast
  ```
  
