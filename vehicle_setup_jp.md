# Autowareで車両を動かす方法
## センサとの接続方法
LiDARとCameraデータを取得するための方法を説明する。


1. PCと車両から出ているType-AのUSBを接続（Type-Cとの変換アダプタが接続されているかも）
2. 設定→ネットワーク→有線の歯車（有線設定）→IPv4を開き下記の設定
写真
3. IPv6を無効に変更
4. 再度接続（そのままだと設定変更が反映されない）

5. CANの設定
    ```
    $ ./can_config.sh #出力はCAN configuration doneと出るはず
    $ ifconfig #txqueuelenが500000になっていれば成功
    ```

6. 車両IDの設定
    ```
    $ export VEHICLE_ID=3 #3号車の場合は
    ```
7. （カメラを使う場合は）カメラノードを立ち上げる
    ```
    $ ./launch_camera.sh
    ```


## 接続確認

1. 下記コマンドで応答があるか確認
    ```
    $ ping 192.168.1.201
    ```
2. 応答がない場合、tcpdumpコマンドでどのIPからパケットが来ているか調べる
    ```
    $ sudo tcpdump
    ```
    ※出力が `時刻 IPアドレス > 255.255.255.255.xxxx: UDP, length 1206` となっているはず

## Autowareで制御する手順
※車両を動かすときは、必ず [車両を動かす際の注意点](#車両を動かす際の注意点)を読み、必ずそこに書いてあることを守ること。

1. 下記コマンドでAutowareを起動する
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final
$ source install/setup.bash
$ cd scripts
$ ./can_config.sh # 実機のときのみ必要
$ ./run.sh
```

2. ゴール地点を指定
- Aコースの場合：Psimと同じ容量でゴール地点を設定
- Bコースの場合：scriptディレクトリで`./B_set_goal.sh`を実行するとゴールが指定される

3. Web controllerを開き、Vehicle Engageを押す（Trueにする）
    - [web controller](localhost:8085/web_controller/index.html)
    - Vehicle Engage項目のengageボタンをクリック

4. Autoware Engageは、Rvizの左したにあるEngageボタンを押す



## 車両を動かす際の注意点

1. 自動運転発進の際のコミュニケーション
    - SD：自動運転準備OKです。
    - 参加者：Web controllerを立ち上げる
    - 参加者：Web controllerでVehicle engageをEngageする
    - SD：Engageお願いします。
    - 参加者：RvizでAutoware engageをする

2. オーバーライドが発生した際（ドライバーが車両を停止させたとき）は以下の手順を必ずやる
    - [web controller](localhost:8085/web_controller/index.html)上でAutoware EngageとVehicle Engage項目のDisengageボタンを押す
    
        ※例えすでにFalseになっていたとしても、必ずDisengageを押す


