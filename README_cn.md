# 中文 | [English](README.md)

# TRON1 机器人描述

TRON1 双足和轮足机器人的 URDF/Xacro 模型。支持 ROS 1 (Noetic)、ROS 2 (Iron)、MuJoCo 和 Gazebo 仿真。

## 可用机器人模型

### 双足 — pointfoot（10 种变体）

| 型号 | 内容 |
|-------|----------|
| `PF_P441A`、`PF_P441B`、`PF_P441C`、`PF_P441C2` | urdf、xacro、xml、meshes |
| `PF_TRON1A`、`PF_TRON1B` | urdf、xacro、xml、meshes |
| `SF_TRON1A`、`SF_TRON1B` | urdf、xacro、xml、meshes |
| `WF_TRON1A`、`WF_TRON1B` | urdf、xacro、xml、meshes |

> `pointfoot/meshes_camera/` — 可选的 Intel RealSense D435 相机网格，被 SF/WF/PF_TRON1B 型号引用（当前处于注释状态）。

### 轮足（2 种变体）

| 型号 | 内容 |
|-------|----------|
| `WL_P311D`、`WL_P311E` | urdf、srdf、meshes |

## 快速开始

### ROS 1 (Noetic)

```bash
cd ~/limx_ws/src
git clone https://github.com/limxdynamics/tron1-robot-description.git
cd ~/limx_ws
source /opt/ros/noetic/setup.bash
catkin_make install
source devel/setup.bash

# 设置机器人型号（例如 SF_TRON1A）
echo 'export ROBOT_TYPE=SF_TRON1A' >> ~/.bashrc && source ~/.bashrc

# 加载 URDF
rosrun xacro xacro $(rospack find robot_description)/pointfoot/$ROBOT_TYPE/xacro/robot.xacro
```

### ROS 2 (Iron)

```bash
cd ~/limx_ws/src
git clone https://github.com/limxdynamics/tron1-robot-description.git
cd ~/limx_ws
source /opt/ros/iron/setup.bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
source install/setup.bash
```

### MuJoCo

所有 pointfoot 型号均包含 MuJoCo XML：

```python
import mujoco
model = mujoco.MjModel.from_xml_path("tron1-robot-description/pointfoot/SF_TRON1A/xml/robot.xml")
data = mujoco.MjData(model)
```

## 相关仓库

- [tron1-gazebo-ros](https://github.com/limxdynamics/tron1-gazebo-ros) — Gazebo 仿真（ROS Noetic）
- [tron1-gazebo-ros2](https://github.com/limxdynamics/tron1-gazebo-ros2) — Gazebo 仿真（ROS 2 Iron）
- [tron1-mujoco-sim](https://github.com/limxdynamics/tron1-mujoco-sim) — MuJoCo 仿真
- [tron1-rl-deploy-ros](https://github.com/limxdynamics/tron1-rl-deploy-ros) — RL 部署（ROS Noetic）
- [tron1-rl-deploy-ros2](https://github.com/limxdynamics/tron1-rl-deploy-ros2) — RL 部署（ROS 2 Iron）
