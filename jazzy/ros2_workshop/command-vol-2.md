# Seed-Lifter-Mover環境構築

---

## 動作環境

| 項目 | バージョン |
|------|-----------|
| OS | Ubuntu 24.04 |
| ROS2 | Jazzy Jalisco LTS |

---

## ワークスペース作成
```bash
mkdir -p ~/seed_ws/src
cd ~/seed_ws/src
git clone -b ros2-jazzy https://github.com/rsdlab/seed_robot_ros2_pkg.git
```

## サブモジュールアップデート
```bash
cd seed_robot_ros2_pkg/
git submodule update --init --recursive
```
## パッチ適用
```bash
patch -p0 < patch/urg_node2.patch
```

## ロボットプロジェクトのクローン
```bash
cd ~/seed_ws/src/seed_robot_ros2_pkg/robots
git clone -b ros2-jazzy https://github.com/rsdlab/noid_lifter_mover.git
```

### Udev設定(実機を動かす場合)
```bash
cd ~/seed_ws/src/seed_robot_ros2_pkg/scripts
./make_udev_install.sh
```
以下のメッセージが表示されます。
```
udevファイルをコピーします
完了しました
```

## Gazebo用パッケージをクローン
```bash
cd ~/seed_ws/src
git clone -b jazzy https://github.com/ros-controls/gz_ros2_control.git
touch gz_ros2_control/gz_ros2_control_demos/COLCON_IGNORE
```

## 移動機能パッケージをクローン
```bash
cd ~/seed_ws/src
git clone -b ros2-jazzy https://github.com/rsdlab/movement_function.git
```

## 人協働マニピュレーションパッケージをクローン
```bash
cd ~/
git clone -b ros2-jazzy https://github.com/rsdlab/Human_Collaboraiton_System.git
cd Human_Collaboraiton_System/
cp -r Seed-noid/* ~/seed_ws/src
```

## ビルド
```bash
cd ~/seed_ws
colcon build --symlink-install
source install/setup.bash
```

## シミュレーション(Gazebo)
それぞれ別のターミナルで起動してください
```bash 
ros2 launch noid_lifter_mover bringup_gazebo.launch.py
ros2 run map_management map_management_node
ros2 run node_transformation get_environment_coordinates_server
ros2 run workpieces_detection_subsystem WorkpiecesDetectionNode
ros2 run place_position_detection_subsystem PlacePositionDetectionNode
ros2 run collaboration_manipulation_module CollaborationManipulationModule --ros-args -p use_sim_time:=true
ros2 run mobile_robot_navigation_module mobile_robot_navigation_node --ros-args -p use_sim_time:=true
ros2 run external_app pose_hint_client
ros2 run management_system ManagementSystemNode --ros-args -p use_sim_time:=true
```

## 実機
それぞれ別のターミナルで起動してください
```bash
ros2 launch noid_lifter_mover bringup_robot.launch.py
ros2 run map_management map_management_node
ros2 run node_transformation get_environment_coordinates_server
ros2 run workpieces_detection_subsystem WorkpiecesDetectionNode
ros2 run place_position_detection_subsystem PlacePositionDetectionNode
ros2 run collaboration_manipulation_module CollaborationManipulationModule
ros2 run mobile_robot_navigation_module mobile_robot_navigation_node
ros2 run external_app pose_hint_client
ros2 run management_system ManagementSystemNode
```



## 実装のコマンド
### 課題1：周辺環境・人検出
```bash
cd ~/seed_ROS2_ws
colcon build --packages-select intrusion_detection_exercise
source install/setup.bash
ros2 run intrusion_detection_exercise PerEnvDetectNode
```

#### 型が std_msgs/Int16 であることを確認
```bash
ros2 topic info /intrusion_result 
```

Gazeboを起動

人協働システムを実際に止めたい場合は、手動でトピックに「人あり」を流します：

```bash
ros2 topic pub /intrusion_result std_msgs/Int16 "{data: 1}"
```

### 課題2：ワーク認識サービスサーバ
```bash
cd ~/seed_ROS2_ws
colcon build --packages-select workpieces_detection_exercise
source install/setup.bash
```

Gazeboを起動

`ros2 run place_position_detection_subsystem PlacePositionDetectionNode`の代わりに以下を実行

`ros2 run workpieces_detection_exercise WorkDetectNode`

```bash
ros2 service call /detect_workpieces_service \
  collaboration_manipulation_message/srv/DetectWorkpieces \
  "{task_command_id: 1, work_type_id: 5}"
```

### 課題3：上位アプリから作業を指令
```bash
cd ~/seed_ROS2_ws
colcon build --packages-select management_system_exercise
source install/setup.bash
```

`CollaborationManipulationModule`まで起動


```bash
ros2 run management_system_exercise SendTaskCommandNode
```
