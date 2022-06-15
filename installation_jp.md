# Autowareのインストール方法

## 用意する環境
```
GPUが搭載されたPC．
Ubuntu 20.04環境．
※WSLは運営がサポートしていない．
```


## Autowareをインストール

```
$ cd # ホームディレクトリへ
$ git clone git@github.com:AutomotiveAIChallenge/aichallenge2022final-base.git ./aichallenge2021final # ※注意1
$ cd ~/aichallenge2022final
$ ./setup_ubuntu20.04.sh # ※注意2
$ source /opt/ros/galactic/setup.bash
$ colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release # ※注意3
```

注意1
cloneするrepository(git@github.com:AutomotiveAIChallengeaichallenge2022final-base.git)は一例であり，指定されたrepositoryを設定すること．

注意2
> y, passward, y の順番で受け答えする．
> Autowareセットアップ時に，PC環境によってはNvidia driverやCUDAの競合，その他エラーが起きる可能性があるため，エラーが発生した場合は[error_resolution_jp.md](./error_resolution_jp.md)を参照．

注意3
> ビルドの負荷が高く，ビルドに失敗するときは下記コマンドを使用する．(ビルドの負荷を制限している．)
```
$ MAKEFLAGS=-j1 colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --parallel-worker 1
```

## 動作確認


**Simulatorを動かすとき**
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final
$ source install/setup.bash
$ cd scripts
$ ./psim.sh
```
Rvizが立ち上がれば動作確認完了．

**自動運転用ゴルフカートを動かすとき**
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2022final
$ source install/setup.bash
$ cd scripts
$ # ./can_config.sh # 実機のときのみ必要
$ ./run.sh
```

