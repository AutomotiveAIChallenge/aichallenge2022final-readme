# Error resolution

## Errors at "setup_ubuntu20.04.sh"

- **TASK [cuda : CUDA (add CUDA repository into sources.list)]でfail**
  
  ![image](https://user-images.githubusercontent.com/45618513/171146891-d3b7c601-cf47-405e-b80a-ac8af2b103a7.png)

  下記コマンドで解消。
  ```
  sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
  ```
  
  

- **TASK [cuda : install software-properties-common]でfail**

  ![Screenshot from 2022-05-16 10-49-50](https://user-images.githubusercontent.com/67366636/169556134-d19a0103-1f23-44c6-b31c-2181e83f56eb.png)

  NVIDIAのリポジトリのキーが更新が原因のようです。下記コマンドで解消。
  ```
  sudo apt-key del 7fa2af80
  wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
  sudo dpkg -i cuda-keyring_1.0-1_all.deb 
  sudo apt update
  ```
  - [NVIDIAのサイト](https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/)
  - [日本語サイト](https://www.nemotos.net/?p=5178)

  ※サイトの手順、エラーの解決策でもダメだった場合、XXXXXXXの公開鍵を追加
  ```  
  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys XXXXXX 
  ```
  
- **TASK [tensorrt : TensorRT (install libraries for TensorRT 7)] でfail**

  ![Screenshot from 2022-05-23 23-12-47](https://user-images.githubusercontent.com/67366636/169840548-078d9650-a2e5-48ae-b474-89008d1e48d8.png)
  
  要求されるCUDAのバージョンと違うものが入っていたことが原因のようです。自分の場合は下記手順で解消。
  
  ※cuda: 11.1, cudnn: 8.0.5, TensorRT: 7.2.1
  
  ```  
  sudo apt remove nvidia*
  sudo dpkg --remove --force-remove-reinstreq XXXXXXX　※添付画像のエラーメッセージ下部分にあるnot upgradedなパッケージを強制削除
  ```
  
- **TASK [libtorch : Libtorch (unarchive)] でfail**

  ![Screenshot from 2022-05-23 23-25-46](https://user-images.githubusercontent.com/67366636/169854877-9bc3b1fa-46de-42c5-80d1-0ff57a01ab56.png)
  
  下記手順で解消。
  
  1. [ここから](https://drive.google.com/u/0/uc?id=1eNh3F3xCQ4AMJEHtwb1dhshSyzWMjoc8) パッケージをダウンロード
  2. ダウンロードしたzipファイル名を**libtorch.zip**に変更
  3. libtorch.zipを/tmpに移動
  4. /pilot-auto.test.x1.aichallenge/ansible/roles/libtorch/tasks/main.yaml 内の2~5行をコメントアウト
  
      <img src="https://user-images.githubusercontent.com/67366636/169856701-2b06197a-9d92-4987-aeb1-b906f903bb33.png" width="75%">

## Errors at "colcon build"
- **Failed  <<<  tier4_planning_rviz_plugin**

  ![Screenshot from 2022-05-24 08-53-40](https://user-images.githubusercontent.com/67366636/169956833-74c7c729-7284-4806-a43c-47b2fbb67adf.png) 
  
  /pilot-auto.test.x1.aichallenge/src/autoware/universe/common/tier4_planning_rviz_plugin/src/tools/jsk_overlay_utils.hpp 内を下記のように変更
  
  ```  
  #include <QImage>
          ↓下に変更する
  #include <QCursor>
  #include <QImage>
  #include <QVariant>
  ```  
  
## Errors at "PSim / LSim"
- **Error at [rmw_cyclonedds_cpp]**

  ![Screenshot from 2022-05-26 22-40-13](https://user-images.githubusercontent.com/67366636/170617250-f743cbc1-0e9a-4915-84aa-688fdf49a7b1.png)

  1. 使用するDDSの変更
  
  ```  
  export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
  sudo apt install ros-foxy-rmw-cyclonedds-cpp
  ```  

  2. multicastをlocalhostで行うためのcycloneddsの設定

    ファイルを置く場所はどこでもいいが、今は仮に /opt/autoware/cyclonedds_config.xml に置き、下記内容をコピペする

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


  3. ファイルを作ったら、 ~/.bashrc に以下を追加

  ```
  export CYCLONEDDS_URI=file:///opt/autoware/cyclonedds_config.xml
  ```


  4. maximum receive buffer sizeの変更（再起動時毎回必要）
 
  ```
  sudo sysctl -w net.core.rmem_max=2147483647
  ```
  
  5. localhostをmulticast対応（再起動時毎回必要）
  
  ```
  sudo ifconfig lo multicast
  ```
  
