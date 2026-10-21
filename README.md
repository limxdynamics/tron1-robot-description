# English | [中文](README_cn.md)

# TRON1 Robot Description

> 🧭 **Part of the TRON1 open-source ecosystem.** All TRON1 repositories and where to start: [TRON1 open-source ecosystem](https://github.com/limxdynamics/tron1-rl-isaacgym#tron1-open-source-ecosystem).

URDF/Xacro robot models for TRON1 biped and wheel-legged robots. Ready for ROS 1 (Noetic), ROS 2 (Iron), MuJoCo, and Gazebo simulation.

## Available Robot Models

### Biped — pointfoot (10 variants)

| Model | Contents |
|-------|----------|
| `PF_P441A`, `PF_P441B`, `PF_P441C`, `PF_P441C2` | urdf, xacro, xml, meshes |
| `PF_TRON1A`, `PF_TRON1B` | urdf, xacro, xml, meshes |
| `SF_TRON1A`, `SF_TRON1B` | urdf, xacro, xml, meshes |
| `WF_TRON1A`, `WF_TRON1B` | urdf, xacro, xml, meshes |

> `pointfoot/meshes_camera/` — optional Intel RealSense D435 camera meshes, referenced by SF/WF/PF_TRON1B models (currently commented out).

### Wheel-legged (2 variants)

| Model | Contents |
|-------|----------|
| `WL_P311D`, `WL_P311E` | urdf, srdf, meshes |

## Quick Start

### ROS 1 (Noetic)

```bash
cd ~/limx_ws/src
git clone https://github.com/limxdynamics/tron1-robot-description.git
cd ~/limx_ws
source /opt/ros/noetic/setup.bash
catkin_make install
source devel/setup.bash

# Set your robot type (e.g. SF_TRON1A)
echo 'export ROBOT_TYPE=SF_TRON1A' >> ~/.bashrc && source ~/.bashrc

# Load the URDF
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

All pointfoot models include MuJoCo XML:

```python
import mujoco
model = mujoco.MjModel.from_xml_path("tron1-robot-description/pointfoot/SF_TRON1A/xml/robot.xml")
data = mujoco.MjData(model)
```

## Related Repositories

- [tron1-gazebo-ros](https://github.com/limxdynamics/tron1-gazebo-ros) — Gazebo simulation with ROS Noetic
- [tron1-gazebo-ros2](https://github.com/limxdynamics/tron1-gazebo-ros2) — Gazebo simulation with ROS 2 Iron
- [tron1-mujoco-sim](https://github.com/limxdynamics/tron1-mujoco-sim) — MuJoCo simulation
- [tron1-rl-deploy-ros](https://github.com/limxdynamics/tron1-rl-deploy-ros) — RL deployment with ROS Noetic
- [tron1-rl-deploy-ros2](https://github.com/limxdynamics/tron1-rl-deploy-ros2) — RL deployment with ROS 2 Iron
