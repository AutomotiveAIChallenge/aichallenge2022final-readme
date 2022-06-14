# シミュレーションの起動方法
## PSim
Planning Simulator。
src下のソースコードを使ったPlanning（経路計画）、Control（経路追従）の挙動を確認できる。

手順1
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final
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

.zst で圧縮されているので、以下の手順で解凍する.
```
$ sudo apt install zstd
$ unzst <filename>
```


手順1
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final
$ source install/setup.bash
$ cd scripts
$ ./lsim.sh
```

手順2 : ROSBAGを再生
```
$ ros2 bag play /path/to/rosbag.db3 --clock
```

各コースのROSBAGが再生され、点群データ、カメラデータが出力される。
それらのデータをAutowareが入力として受け取り、シミュレーションがスタートする。


### 補足

`logging_simulator.launch.xml` の Optional parameters から、シミュレーションで動かすモジュール, 動かさないモジュールを選択可能.
ここから動かさないモジュールを選択する。シミュレーション時、不要な(重複している)トピックはrosbag再生時のtopic選択やremapで除外する。

例: perception モジュールのシミュレーションをする場合

1. Optional parameters を以下のように設定する

```
  <!-- Optional parameters -->
  <arg name="vehicle" default="false" description="launch vehicle" />
  <arg name="system" default="false" description="launch system" />
  <arg name="map" default="true" description="launch map" />
  <arg name="sensing" default="false" description="launch sensing" />
  <arg name="localization" default="false" description="launch localization" />
  <arg name="perception" default="true" description="launch perception" />
  <arg name="planning" default="false" description="launch planning" />
  <arg name="control" default="false" description="launch control" />
  <arg name="rviz" default="true" description="launch rviz" />
```

2. rosbagの再生コマンド例 (record時のtopicをremap)

```
ros2 bag play . --clock -r 0.5 --remap /perception/object_recognition/objects:=/recorded/perception/object_recognition/objects
```

3. perception関連のlaunchファイルを編集し、動かしたいモジュールがlaunchされるようにする

4. logging simulator を起動する

