# Autowareのインストール方法

## 用意する環境
```
GPUが搭載されたPC
Ubuntu 20.04環境
```


## Autowareをインストール

```
$ cd # ホームディレクトリへ
$ git clone git@github.com:AutomotiveAIChallenge/aichallenge2022final-test
$ cd ~/aichallenge2022final-test
$ mkdir src
$ vcs import src < autoware.proj.repos # ※注意1
$ ./setup_ubuntu20.04.sh # ※注意2
$ source /opt/ros/galactic/setup.bash
$ colcon build --symlink-install --cmake-args -DCMAKE_BUILDTYPE=Release # ※注意3
```
注意1
> 長時間かかるときもある。
> ..E..となるときはエラーが出てるため、再度vcs import。
> すべて"......."となったなら次のコマンドへ進む（それ以上やるとエラーが再発するときがある）。

注意2
> y, passward, y の順番で受け答えする。
> エラーが出た場合は[こちら](./error_resolution_jp.md)を参照。

注意3
> もしフリーズするときは下記コマンドでやると負荷を下げられる。
```
$ MAKEFLAGS=-j1 colcon build --symlink-install --cmake-args -DCMAKE_BUILDTYPE=Release --parallel-worker 1
```

## 動作確認

~~以下のコマンドでRvizが立ち上がれば動作確認完了~~
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final-test
$ source install/setup.bash
$ cd scripts
$ # ./can_config.sh # 実機のときのみ必要
$ ./run.sh
```
~~Rvizが立ち上がれば動作確認完了~~

[こちら](./launch_simulator_jp.md)のRvizが立ち上がれば動作確認完了
