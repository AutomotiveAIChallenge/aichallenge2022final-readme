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
$ ./setup_ubuntu20.04.sh # ※注意1
$ source /opt/ros/galactic/setup.bash
$ colcon build --symlink-install --cmake-args -DCMAKE_BUILDTYPE=Release # ※注意2
```

注意1
> y, passward, y の順番で受け答えする。
> エラーが出た場合は[こちら](./error_resolution_jp.md)を参照。

注意2
> ビルドの負荷が高く、ビルドに失敗するときは下記コマンドを使用する。(ビルドの負荷を制限しています。)
```
$ MAKEFLAGS=-j1 colcon build --symlink-install --cmake-args -DCMAKE_BUILDTYPE=Release --parallel-worker 1
```

## 動作確認


**Simulatorを動かすとき**
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final-test
$ source install/setup.bash
$ cd scripts
$ ./psim.sh
```
Rvizが立ち上がれば動作確認完了

**自動運転用ゴルフカートを動かすとき**
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final-test
$ source install/setup.bash
$ cd scripts
$ # ./can_config.sh # 実機のときのみ必要
$ ./run.sh
```

