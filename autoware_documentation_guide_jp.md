# Autoware Documentation Guide
参加者は次の手順で作業を進めること．
- [Autowareをインストール](./installation_jp.md)．
- [AutowareのDesign Documentation](#design-documentation)を見ながら，自分の追加したい機能を追加．
- [シミュレータ(Psim/Sim)](./launch_simulator_jp.md)を使って動作確認．
- 当日のセットアップは[車両セットアップ方法](./vehicle_setup_jp.md)を見ながら実施．
    > 資料は事前に見ておくこと．ビルドも事前に済ませておくと練習時間が増える．

以下は参考．
- [公式ドキュメント](https://autowarefoundation.github.io/autoware-documentation/main/).

## インストール方法
自動運転AIチャレンジ用のAutowareをインストールする方法は [installation_jp.md](./installation_jp.md)を参照．

Autowareセットアップ時に，PC環境によってはNvidia driverやCUDAの競合，その他エラーが起きる可能性があるため，エラーが発生した場合は[error_resolution_jp.md](./error_resolution_jp.md)を参照．
それでもわからない場合はSlackにて運営に連絡．


## Design Documentation
Autowareは主にSensing, Localization, Perception, Planning, Controlモジュールで構成されている．
各モジュールの動作は各パッケージのREADMEや，[公式ドキュメント](https://autowarefoundation.github.io/autoware-documentation/main/)を参照．


競技用レポジトリのlaunchファイル，システム構成は[ベースレポジトリのノードダイアグラム](https://drive.google.com/file/d/1o8onRPBdhQ5zbgDRxfL7Vq7brMTrpu6E/view?usp=sharing) を参照．
各モジュールのソースコードは下記表にて記載．

| Module name  | Source code path | Explanation | 
| ------------ | ---------------- | ----------- | 
| Sensing      | $WORKSPACE/src/autoware/universe/sensing | LiDARのデータを前処理するモジュール．| 
| Localization | $WORKSPACE/src/autoware/universe/localization | Ego車両の自己位置を推定するモジュール． | 
| Perception   | $WORKSPACE/src/autoware/universe/perception | センサーデータを使い，周囲の物体を検知するモジュール． | 
| Planning     | $WORKSPACE/src/autoware/universe/planning | 現在位置から目的地までの経路を計算するモジュール． | 
| Control      | $WORKSPACE/src/autoware/universe/control | 経路に沿って動くように，Ego車両の速度と角速度を計算するモジュール． | 

## ソースコード編集時の注意事項

- 地図ファイルlanelet2_map.osm 及び，地図ファイルに書かれた制限速度を読み込む部分のソースコードは変更しない事．
