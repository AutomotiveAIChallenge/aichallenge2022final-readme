# Autoware Documentation Guide
公式ドキュメントは [こちら](https://autowarefoundation.github.io/autoware-documentation/main/).

## インストール方法
Autowareをインストールする方法は [installation_jp.md](./installation_jp.md)と [installation_en.md](./installation_en.md) に書かれている。

公式サイトではなく、上記のサイトを見るように。


## Degine Documentation
Autowareは主にSensing, Localization, Perception, Planning, Controlモジュールで構成されいている。

[Autoware Universeのノードグラフ](https://tier4.github.io/autoware-documentation/latest/design/node-diagram/) は各モジュールの関係図を可視化したものである。
各モジュールのソースコードは下記表にて記載。

| Module name  | Source code path | Explanation | 
| ------------ | ---------------- | ----------- | 
| Sensing      | $WORKSPACE/src/autoware/universe/sensing | LiDARのデータを前処理するモジュール。| 
| Localization | $WORKSPACE/src/autoware/universe/localization | Ego車両の自己位置を推定するモジュール。 | 
| Perception   | $WORKSPACE/src/autoware/universe/perception | センサーデータを使い、周囲の物体を検知するモジュール。 | 
| Planning     | $WORKSPACE/src/autoware/universe/planning | 現在位置から目的地までの経路を計算するモジュール。 | 
| Control      | $WORKSPACE/src/autoware/universe/control | 経路に沿って動くように、Ego車両の速度と角速度を計算するモジュール。 | 

## ソースコード編集時の注意事項

- lanelet2_map.osmファイルに書かれた制限速度を読み込んでいる部分のソースコードは変更しない事。
