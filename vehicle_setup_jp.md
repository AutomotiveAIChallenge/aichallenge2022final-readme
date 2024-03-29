# Autowareで車両を動かす方法

## Vehicle interfaceの有効化
別途ビルドされた `~/vi_ws` の中にvehicle interfaceが入っております.
vehicle interfaceの起動のため、`~/vi_ws/install` 内で参照されているディレクトリを変更する必要があります.
初回のみ、以下の作業を行ってください.

1. aichallenge2022final-testの移動 `mv aichallenge2022final-test/ aichallenge2022final-test_bk`
2. シンボリックリンクの適用 
以下のコマンドを実行し, シンボリックリンクを適用する.

```
$ ln -snf /home/autoware/<開発に使用しているレポジトリ> /home/autoware/aichallenge2022final-test
```

3. vehicle interface の起動確認
`scripts/run.sh` 
を実行し、
`ros2 node list |grep g30` で以下のノードが起動していることを確認する. 

```
/g30esli/socket_can_receiver
/g30esli/socket_can_sender
/g30esli_interface
/g30esli_interface_awiv_adapt_receiver
/g30esli_interface_awiv_adapt_sender
```


## センサとの接続方法
LiDARとCameraデータを取得するための方法を説明する．


1. PCと車両から出ているType-AのUSBを接続（Type-Cとの変換アダプタが接続されていることもある）．
2. 設定→ネットワーク→有線の歯車（有線設定）→IPv4を開き下記の設定．
    ![image](https://user-images.githubusercontent.com/45618513/173177397-444d40da-1146-4d02-be77-e96ac2054268.png)

3. IPv6を無効に変更．
4. 再度接続（そのままだと設定変更が反映されない）．

5. CANの設定．
    ```
    $ ./can_config.sh # 成功ならば出力はCAN configuration done
    $ ifconfig # txqueuelenが500000になっていれば成功
    ```

6. 車両IDの設定．
    ```
    $ export VEHICLE_ID=3 # 3号車の場合. VEHICLE_IDは競技当日、メンターから提示.
    ```
7. （カメラを使う場合は）別ターミナルでカメラノードを立ち上げる．
    ```
    $ ./launch_camera.sh
    ```


## 接続確認

1. 下記コマンドでLiDARから応答があるか確認．
    ```
    $ ping 192.168.1.201
    ```
2. 応答がない場合，tcpdumpコマンドでどのIPからパケットが来ているか調べる．
    ```
    $ sudo tcpdump -i <network IF name>
    ```
    ※出力は `時刻 IPアドレス > 255.255.255.255.xxxx: UDP, length 1206` ．

## Autoware起動〜自動運転開始の操作
※車両を動かす前に，必ず [車両を動かす際の注意点](#車両を動かす際の注意点)を一読ください．

1. 下記コマンドでAutowareを起動する．
    ```
    $ source /opt/ros/galactic/setup.bash #.bashrc記載の場合は不要
    $ cd ~/aichallenge2022final
    $ source install/setup.bash #.bashrc記載の場合は不要
    $ cd scripts
    $ ./can_config.sh # 実機のときのみ必要
    $ ./run.sh
    ```

2. 自己位置推定を開始する.
    - Aコースの場合: Psim同様, 2D pose estimate をrVizから入力する.
    - Bコースの場合: `scripts/B_set_start.sh` を実行すると自己位置推定

3. ゴール地点を指定する．
    - Aコースの場合：Psimと同じ要領でゴール地点を設定．
    - Bコースの場合：scriptsディレクトリで`./B_set_goal.sh`を実行するとゴールが指定される．

4. Web controllerを開き，Vehicle Engage, Autoware Engage が falseになっていることを確認後, Vehicle Engageを押す（Trueにする）．
    - web controllerを起動（下記URL）
        - localhost:8085/web_controller/index.html
    - Vehicle Engage項目のengageボタンをクリック

5. rVizをアクティブにし、の左下にあるEngageボタンを押す．



## 車両を動かす際の注意点

1. 自動運転発進の際のコミュニケーション．
    - SD: Golf Cartを自動モードに設定する．
    - メンター: 競技参加者に "Webコントローラーを起動．Vehicle engageとAutoware engageのdisengageを押下する" 指示を出す．
    - 参加者: Webコントローラーを起動．Vehicle engageとAutoware engageのdisengageを押下．
    - メンター: 競技参加者に "Vehicle engageを押下する" 指示を出す．
    - 競技参加者: Vehicle engageのengageを押下．
    - SD: ステアの挙動から，正常にVehicle engageされたことを確認．
    - メンター: "RVizに切り替え，Autoware engageを押下する" 指示を出す．
    - 競技参加者: RVizに切り替え，Autoware engageボタンを押下．

2. オーバーライドが発生した際（ドライバーが車両を停止させたとき）は以下の手順を必ず行う．
    - [web controller](localhost:8085/web_controller/index.html)上でAutoware EngageとVehicle Engage項目のDisengageボタンを押す．
    
        ※既にFalseになっていたとしても，必ずDisengageを押す．

3. Vehicle Engage時に，ハンドルが急回転することがある．本事象が発生した場合は、Autoware Engage と Vehicle EngageをFalseにしてから手順をやり直す．
4. 自動走行時，PCを操作しない参加者は, 歩道上で待機する．(経路上や車両周囲は立ち入り禁止)
5. 自動走行時は必ず上部の手すりを掴む．
6. 自動運転に使用するノートPCをバンドで固定する. キーボードが打ちづらい場合は貸出用のキーボードを使用する．

