# ROS2 環境構築手順

> **この資料について**：`人協働マニピュレーション実装教材(ROS2).pptx` の内容と同等の手順書です。`人協働マニピュレーション実装教材(ROS2).pptx`をもとにROS2環境をセットアップする開発者向け

---

## 動作環境

| 項目 | バージョン |
|------|-----------|
| OS | Ubuntu 24.04 |
| ROS2 | Jazzy Jalisco LTS |

---
### 動作確認
(ターミナル①)
```shell
ros2 run demo_nodes_cpp listener
```
(ターミナル②)
```shell
ros2 run demo_nodes_cpp talker
```

---

# ROS2通信実装
## 事前準備
```shell
mkdir -p ~/colcon_ws/src
cd ~/colcon_ws/src
mkdir ros2_topic_sample ros2_service_sample ros2_action_sample
```

## Topic通信実装
### ビルド
```shell
cd ~/colcon_ws
colcon build -packages-select exercise_topic
source install/setup.bash
```

### 実行
(ターミナル①)
```shell
ros2 run ros2_topic_sample listener
```
(ターミナル②)
```shell
ros2 run ros2_topic_sample talker
```

期待される動作例
>(listener)
```shell
$ ros2 run ros2_topic_sample listener
[INFO] [1781489269.650403754] [minimal_subscriber]: Subscriberノードを起動しました。メッセージを待機中...
[INFO] [1781489274.258273758] [minimal_subscriber]: 受信 [1件目]: "こんにちは、ROS2! カウント: 0"
[INFO] [1781489274.757738197] [minimal_subscriber]: 受信 [2件目]: "こんにちは、ROS2! カウント: 1"
[INFO] [1781489275.257928139] [minimal_subscriber]: 受信 [3件目]: "こんにちは、ROS2! カウント: 2"
[INFO] [1781489275.757403737] [minimal_subscriber]: 受信 [4件目]: "こんにちは、ROS2! カウント: 3"
[INFO] [1781489276.257924483] [minimal_subscriber]: 受信 [5件目]: "こんにちは、ROS2! カウント: 4"
[INFO] [1781489276.758017357] [minimal_subscriber]: 受信 [6件目]: "こんにちは、ROS2! カウント: 5"
```
>(talker)
```shell
$ ros2 run ros2_topic_sample talker
[INFO] [1781489273.764787381] [minimal_publisher]: Publisherノードを起動しました
[INFO] [1781489274.257640960] [minimal_publisher]: 配信: "こんにちは、ROS2! カウント: 0"
[INFO] [1781489274.757225301] [minimal_publisher]: 配信: "こんにちは、ROS2! カウント: 1"
[INFO] [1781489275.257422232] [minimal_publisher]: 配信: "こんにちは、ROS2! カウント: 2"
[INFO] [1781489275.756855030] [minimal_publisher]: 配信: "こんにちは、ROS2! カウント: 3"
[INFO] [1781489276.257358760] [minimal_publisher]: 配信: "こんにちは、ROS2! カウント: 4"
[INFO] [1781489276.757378403] [minimal_publisher]: 配信: "こんにちは、ROS2! カウント: 5"
```

## Service通信実装
### ビルド
```shell
cd ~/colcon_ws
colcon build --packages-select ros2_service_sample
source install/setup.bash
```
### 実行
(ターミナル①)
```shell
ros2 run ros2_service_sample service_server
```
(ターミナル②)
```shell
ros2 run ros2_service_sample service_client
```
期待される動作
> service_server
```shell
[INFO] [1781490805.615295848] [minimal_server]: Serverノードを起動しました。リクエストを待機中...
[INFO] [1781490817.124400293] [minimal_server]: リクエスト受信: a=3, b=5 → sum=8
```
>service_client
```shell
[INFO] [1781490817.123502492] [minimal_client]: Clientノードを起動しました
[INFO] [1781490817.125051364] [minimal_client]: 結果受信: 3 + 5 = 8
```

## Action通信実装
### ビルド
```shell
cd ~/colcon_ws
colcon build --packages-select ros2_action_sample
source install/setup.bash
```
### 実行
(ターミナル①)
```shell
ros2 run ros2_action_sample action_server
```
(ターミナル②)
```shell
ros2 run ros2_action_sample action_client
```
期待される動作
> action_server
```shell
[INFO] [1781491817.074692526] [minimal_action_server]: Action Serverノードを起動しました。Goalを待機中...
[INFO] [1781491819.765021092] [minimal_action_server]: Goal受信: order=10
[INFO] [1781491819.766072898] [minimal_action_server]: フィードバック送信: [0, 1, 1]
[INFO] [1781491820.267654054] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2]
[INFO] [1781491820.769781234] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2, 3]
[INFO] [1781491821.271282008] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2, 3, 5]
[INFO] [1781491821.772557088] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2, 3, 5, 8]
[INFO] [1781491822.274064113] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2, 3, 5, 8, 13]
[INFO] [1781491822.775685885] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2, 3, 5, 8, 13, 21]
[INFO] [1781491823.277248144] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
[INFO] [1781491823.778599764] [minimal_action_server]: フィードバック送信: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
[INFO] [1781491824.280241944] [minimal_action_server]: Result送信: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
> action_client
```shell
ros2 run ros2_action_sample action_client
[INFO] [1781491819.510307722] [minimal_action_client]: Action Clientノードを起動しました
[INFO] [1781491819.510589244] [minimal_action_client]: Action Serverを待機中...
[INFO] [1781491819.761712894] [minimal_action_client]: Goal送信: order=10
[INFO] [1781491819.764738367] [minimal_action_client]: Goalが受理されました。結果を待機中...
[INFO] [1781491819.766751066] [minimal_action_client]: フィードバック受信: [0, 1, 1]
[INFO] [1781491820.268208522] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2]
[INFO] [1781491820.770082601] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2, 3]
[INFO] [1781491821.271743700] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2, 3, 5]
[INFO] [1781491821.773033179] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2, 3, 5, 8]
[INFO] [1781491822.274606396] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2, 3, 5, 8, 13]
[INFO] [1781491822.776112131] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2, 3, 5, 8, 13, 21]
[INFO] [1781491823.277750211] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
[INFO] [1781491823.779046386] [minimal_action_client]: フィードバック受信: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
[INFO] [1781491824.283245562] [minimal_action_client]: Result受信: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```