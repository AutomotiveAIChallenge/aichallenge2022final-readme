# Installation of Autoware

## Prerequisites
```
PC with GPU
Ubuntu 20.04
```


## Installation

```
$ cd # go to home directory
$ git clone git@github.com:AutomotiveAIChallenge/aichallenge2022final-base ./aichallenge2022final # ※Note 1
$ cd ~/aichallenge2022final
$ ./setup_ubuntu20.04.sh # ※Note 2
$ source /opt/ros/galactic/setup.bash
$ colcon build --symlink-install --cmake-args -DCMAKE_BUILDTYPE=Release # ※Note 3
```

Note 1
The repository(git@github.com:AutomotiveAIChallenge/aichallenge2022final-base) listed is an example.
Please set the specified repository

Note 2
> Reply y, passward, y in order.
> If occuring errors, look [here](./error_resolution_en.md)).

Note 3
> If PC was freezed, the below command may decreases the CPU load.
```
$ MAKEFLAGS=-j1 colcon build --symlink-install --cmake-args -DCMAKE_BUILDTYPE=Release --parallel-worker 1
```

## Launch test

~~If Rviz is launched by the following commands, the launch test is completed.~~ 
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final
$ source install/setup.bash
$ cd scripts
$ # ./can_config.sh # only real experiments
$ ./run.sh
```
If Rviz is started up according to [this procedure](./launch_simulator_en.md), the operation check is complete.
