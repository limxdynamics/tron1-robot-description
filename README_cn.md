<div align="center">

[English](README.md) | **中文**

# TRON1 Robot Description

**逐际动力 TRON1 双足机器人的开源仿真模型 —— 点足、双足（带踝关节）、轮足三种形态，另附轮足四足模型。**

URDF · Xacro · MuJoCo MJCF · ROS 1 · ROS 2 · Gazebo

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![ROS 1](https://img.shields.io/badge/ROS%201-Noetic-22314E?logo=ros)](http://wiki.ros.org/noetic)
[![ROS 2](https://img.shields.io/badge/ROS%202-Iron-22314E?logo=ros)](https://docs.ros.org/en/iron/)
[![MuJoCo](https://img.shields.io/badge/MuJoCo-ready-0A7BBB)](https://mujoco.org/)
[![Gazebo](https://img.shields.io/badge/Gazebo-ready-F58113)](https://gazebosim.org/)
[![GitHub stars](https://img.shields.io/github/stars/limxdynamics/tron1-robot-description?style=social)](https://github.com/limxdynamics/tron1-robot-description/stargazers)

<img src="docs/images/hero.png" alt="TRON1 点足、双足、轮足及轮足四足模型" width="100%">

<sub>以上图片直接由本仓库的 MJCF / URDF 文件渲染生成。</sub>

</div>

> **TRON1 开源生态的一部分** —— 覆盖从机器人模型、强化学习训练、sim-to-sim 验证到真机部署的完整链路。详见 [TRON1 开源生态地图](https://github.com/limxdynamics/tron1-rl-isaacgym/blob/master/README_cn.md#tron1-开源生态)。

## 亮点

- **一台 TRON1，三种足端。** 点足、双足（带踝关节）、轮足之间切换，只需改一个型号名。
- **30 秒加载模型。** 所有双足型号都附带 MuJoCo MJCF，只需 `pip install mujoco`，无需安装 ROS。
- **一套模型，全工具链通用。** 同源提供 URDF、Xacro、MJCF，支持 ROS 1 Noetic、ROS 2 Iron、Gazebo 和 MuJoCo。
- **为 sim-to-real 而生。** 与官方 TRON1 训练（[Isaac Lab](https://github.com/limxdynamics/tron1-rl-isaaclab) / [Isaac Gym](https://github.com/limxdynamics/tron1-rl-isaacgym)）、仿真和部署仓库使用同一套模型。
- **内置 Gazebo 与 ros2_control 配置。** Xacro 文件已包含 Gazebo 插件、传动配置和 `ros2_control` 接口，开箱即用。

## 30 秒上手（无需 ROS）

```bash
git clone https://github.com/limxdynamics/tron1-robot-description.git
pip install mujoco
python -m mujoco.viewer --mjcf=tron1-robot-description/pointfoot/WF_TRON1A/xml/robot.xml
```

把 `WF_TRON1A` 换成下表中任意型号即可。也可以在 Python 中加载：

```python
import mujoco

model = mujoco.MjModel.from_xml_path("tron1-robot-description/pointfoot/SF_TRON1A/xml/robot.xml")
data = mujoco.MjData(model)
mujoco.mj_step(model, data)
```

## 机器人模型

| | 类型 | 型号 | 驱动自由度 | 格式 |
|:-:|---|---|:-:|---|
| <img src="docs/images/pf.png" width="110"> | **点足**双足 | `PF_TRON1A` `PF_TRON1B`<br>`PF_P441A` `PF_P441B` `PF_P441C` `PF_P441C2` | 6 | URDF · Xacro · MJCF |
| <img src="docs/images/sf.png" width="110"> | **双足**（带踝关节） | `SF_TRON1A` `SF_TRON1B` | 8 | URDF · Xacro · MJCF |
| <img src="docs/images/wf.png" width="110"> | **轮足**双足 | `WF_TRON1A` `WF_TRON1B` | 8 | URDF · Xacro · MJCF |
| <img src="docs/images/wl.png" width="110"> | **轮足四足** | `WL_P311D` `WL_P311E` | 16 | URDF · SRDF |

<details>
<summary><b>目录结构</b></summary>

```text
tron1-robot-description/
├── pointfoot/                 # TRON1 双足系列
│   ├── <型号>/
│   │   ├── urdf/robot.urdf    # 展开后的 URDF
│   │   ├── xacro/             # robot.xacro + gazebo / transmission / ros2_control
│   │   ├── xml/robot.xml      # MuJoCo MJCF
│   │   └── meshes/            # STL 网格
│   └── meshes_camera/         # 可选 Intel RealSense D435 网格（*_TRON1B 中已引用，当前为注释状态）
├── wheellegged/
│   └── <型号>/{urdf,srdf,meshes}
├── CMakeLists.txt             # 在 ROS 1 / ROS 2 下均编译为 `robot_description` 包
└── package.xml
```

</details>

## 在 ROS 中使用

本仓库在 ROS 1 和 ROS 2 下都编译为同一个名为 **`robot_description`** 的功能包。

<details open>
<summary><b>ROS 1 (Noetic)</b></summary>

```bash
mkdir -p ~/limx_ws/src && cd ~/limx_ws/src
git clone https://github.com/limxdynamics/tron1-robot-description.git
cd ~/limx_ws
source /opt/ros/noetic/setup.bash
catkin_make install
source devel/setup.bash

# 选择机器人型号，例如 SF_TRON1A
export ROBOT_TYPE=SF_TRON1A

# 由 Xacro 生成 URDF
rosrun xacro xacro $(rospack find robot_description)/pointfoot/$ROBOT_TYPE/xacro/robot.xacro
```

</details>

<details open>
<summary><b>ROS 2 (Iron)</b></summary>

```bash
mkdir -p ~/limx_ws/src && cd ~/limx_ws/src
git clone https://github.com/limxdynamics/tron1-robot-description.git
cd ~/limx_ws
source /opt/ros/iron/setup.bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
source install/setup.bash

export ROBOT_TYPE=SF_TRON1A
xacro $(ros2 pkg prefix robot_description)/share/robot_description/pointfoot/$ROBOT_TYPE/xacro/robot.xacro
```

</details>

**Xacro 参数**（双足型号）：

| 参数 | 默认值 | 作用 |
|---|---|---|
| `use_gazebo` | `false` | 加入 Gazebo 的 `ros2_control` 硬件配置（`PointFootHWSim`）及关节、IMU 接口 |
| `use_support` | `false` | 将机身固定在离地 0.3 m 处，便于悬空调试关节 |

```bash
xacro .../pointfoot/$ROBOT_TYPE/xacro/robot.xacro use_gazebo:=true
```

> **提示：** 展开后的 URDF 以 `package://robot_description/...` 引用网格。在 ROS 之外（如 Isaac Sim、PyBullet、Pinocchio）使用时，请将加载器的 package 路径指向本仓库的上级目录，或直接使用 MJCF 文件。

## TRON1 开源生态

```text
tron1-robot-description ──► tron1-rl-isaaclab / tron1-rl-isaacgym ──► tron1-mujoco-sim ──► tron1-rl-deploy-* ──► TRON1 真机
      （当前仓库）                         （训练）                      （sim-to-sim）          （部署）
```

| 阶段 | 仓库 |
|---|---|
| 训练 | [tron1-rl-isaaclab](https://github.com/limxdynamics/tron1-rl-isaaclab)（推荐） · [tron1-rl-isaacgym](https://github.com/limxdynamics/tron1-rl-isaacgym) |
| 仿真 | [tron1-mujoco-sim](https://github.com/limxdynamics/tron1-mujoco-sim) · [tron1-gazebo-ros](https://github.com/limxdynamics/tron1-gazebo-ros) · [tron1-gazebo-ros2](https://github.com/limxdynamics/tron1-gazebo-ros2) |
| 部署 | [tron1-rl-deploy-python](https://github.com/limxdynamics/tron1-rl-deploy-python) · [tron1-rl-deploy-ros](https://github.com/limxdynamics/tron1-rl-deploy-ros) · [tron1-rl-deploy-ros2](https://github.com/limxdynamics/tron1-rl-deploy-ros2) |

**还没有真机？** 直接在 `tron1-mujoco-sim` 中运行 `tron1-rl-deploy-python` 自带的预训练策略，无需训练。

## 参与贡献

发现网格、惯量或关节限位问题，或希望支持其他仿真器？欢迎 [提交 Issue](https://github.com/limxdynamics/tron1-robot-description/issues) 或 Pull Request —— 每一处模型修正都能帮助所有基于 TRON1 训练的开发者。

如果这些模型帮你节省了时间，**欢迎点个 Star**，让更多机器人开发者发现 TRON1。

## 许可证

[Apache License 2.0](LICENSE) © LimX Dynamics
