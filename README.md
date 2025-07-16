# Servo Sim - ROS2 Gimbal Simulation

A ROS2 package for simulating a servo gimbal (pan-tilt camera mount) using Gazebo.

## Overview

This package provides a complete simulation environment for a servo gimbal consisting of:
- Base platform
- Horizontal rotation cylinder (pan)
- Vertical rotation cylinder (tilt) 
- Camera module

## Requirements

- ROS2 (tested with jazzy)
- Gazebo (gz-sim), not working with classic
- RViz2
- Standard ROS2 packages:
  - `robot_state_publisher`
  - `joint_state_publisher_gui`
  - `ros_gz_sim`
  - `ros_gz_bridge`

## Installation

1. Clone this repository into your ROS2 workspace:
```bash
cd ~/ros2_ws/src
git clone <repository-url> servo_sim
```

2. Build the package:
```bash
# make sure you have sourced your ROS2 workspace
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --packages-select servo_sim
source install/setup.bash
```

## Usage

### Display Only (RViz + Joint State Publisher)
```bash
ros2 launch servo_sim display.launch.py
```

### Full Simulation (Gazebo + Controls + Rviz)
```bash
ros2 launch servo_sim servo_complete.launch.py
```
### Basic Simulation (Gazebo Only)
```bash
ros2 launch servo_sim servo_sim.launch.py
```

## Manual Control

Send twist commands to control the gimbal:
```bash
ros2 topic pub /servo/cmd_vel geometry_msgs/msg/Twist "{angular: {y: 0.5, z: 0.3}}"
```

## Topics

- `/camera/image_raw` - Camera image stream
- `/servo/cmd_vel` - Twist commands for manual control
- `/servo/horizontal_joint/cmd_pos` - Direct horizontal joint position to gazebo
- `/servo/vertical_joint/cmd_pos` - Direct vertical joint position to gazebo
- `/servo/command` - Command for the servo joints
- `/joint_states` - Current joint positions

## Package Structure

```
servo_sim/
├── CMakeLists.txt          # Build configuration
├── package.xml             # Package dependencies
├── launch/                 # Launch files
│   ├── display.launch.py   # RViz only
│   ├── servo_complete.launch.py  # Full simulation
│   └── servo_sim.launch.py # Basic simulation
├── model/                  # Robot models
│   └── servo.xacro         # Main robot description
├── rviz/                   # RViz configurations
│   └── servo.rviz          # Default RViz config
├── src/                    # Source code
│   └── nodes/              # ROS2 nodes
│       └── servo_controller.cpp  # C++ controller
└── world/                  # Gazebo world files
    └── world.sdf           # Simulation world
```

## License

Apache 2.0

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## TODO
- Add direct position control for joints, like real servo motors. 