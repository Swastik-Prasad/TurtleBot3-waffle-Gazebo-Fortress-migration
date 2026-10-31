# TurtleBot3 Waffle - Gazebo Fortress Migration (ROS 2 Humble)

This repository contains a modified TurtleBot3 simulation package migrated from Gazebo Classic to Ignition Gazebo Fortress for ROS 2 Humble.

Key modifications applied to the Waffle model (`model.sdf`):

* **Mesh Rendering Fix:** Replaced legacy Gazebo Classic `<script>` materials with Fortress-compatible `<ambient>`, `<diffuse>`, and `<specular>` properties to correctly render the 3D Waffle chassis instead of the fallback collision box.


## Prerequisites

* Ubuntu 22.04 LTS
* ROS 2 Humble
* Ignition Gazebo Fortress (v6.18.0+)
* `turtlebot3` and `turtlebot3_navigation2` ROS 2 packages installed.

## Build Instructions

Navigate to your ROS 2 workspace, clone the repository, and build the package:

```bash
cd ~/turtlebot3_ws/src
git clone https://github.com/Swastik-Prasad/TurtleBot3-waffle-Gazebo-Fortress-migration.git turtlebot3_simulations
cd ~/turtlebot3_ws
colcon build --symlink-install
source install/setup.bash

```

## Running the Simulation

### 1. Launch the Gazebo Environment

Always export the Waffle model variable before launching to prevent the simulation from defaulting to the Burger model.

```bash
export TURTLEBOT3_MODEL=waffle
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py

```

### 2. Manual Teleoperation

Open a new terminal, source the workspace, and run the teleop node to verify drive control and kinematics:

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard

```

* Press `i` to drive forward.
* Press `k` to stop.
* Press `j` / `l` to rotate.

### 3. Autonomous Navigation (Nav2)

Open a new terminal, export the model variable, and launch the Nav2 stack with simulation time enabled to synchronize the ROS network with Gazebo:

```bash
export TURTLEBOT3_MODEL=waffle
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=true map:=$(ros2 pkg prefix turtlebot3_navigation2)/share/turtlebot3_navigation2/map/map.yaml

```

**In the RViz2 GUI:**

1. Click **2D Pose Estimate** in the top toolbar. Click and drag on the map at the exact location where the robot spawned in Gazebo to align the Lidar scans with the map walls.
2. Click **Nav2 Goal** and click/drag on an empty space on the map to send the robot on an autonomous, obstacle-avoiding trajectory.
