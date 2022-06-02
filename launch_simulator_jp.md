# シミュレーションの起動方法
## PSim
Planning Simulator。
src下のソースコードを使ったPlanning（経路計画）、Control（経路追従）の挙動を確認できる。

手順1
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final-test
$ source install/setup.bash
$ cd scripts
$ ./psim.sh
```

手順2
- Rviz上の "2D Pose Estimate" をクリックし、マップ上でクリック＆ドラッグ操作すると初期位置が設定できる。
- Rviz上の "2D Goal Pose" をクリックし、上記と同様の操作をすることでゴール地点を設定する。
- Rviz左下のEngageを押すと車両は経路通りに動き始める。



## LSim
Logging Simulator (ROSBAG Simulator)。
ROSBAGを再生し、Sensing、Localization（自己位置推定）、Detection（物体検出）、Planning（経路計画）の挙動を確認できる。
（ROSBAGには事前にセンサー情報、車両のフィードバック情報などを保存しておく。）
`src/autoware/launcher/autoware_launch/launch/logging_simulator.launch.xml` の Optional parametersを編集することで検証したいモジュールだけを動作させることが可能。
rosbagは [こちら](https://drive.google.com/drive/folders/1rmRtTkxzzIgh1Na3ocdcvFkpiOxl8VOx?usp=sharing) からDL可能。

手順1
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final-test
$ source install/setup.bash
$ cd scripts
$ ./lsim.sh
```

手順2 : ROSBAGを再生
```
$ ros2 bag play /path/to/rosbag.db3
```

手順3
- Rviz上の "2D Pose Estimate" をクリックし、自己位置らしき地点をクリック＆ドラッグ。
- ずれていた場合は "2D Pose Estimate" を繰り返す。
- Rviz上の "2D Goal Pose" をクリックし、上記と同様の操作をすることでゴール地点を設定する。
