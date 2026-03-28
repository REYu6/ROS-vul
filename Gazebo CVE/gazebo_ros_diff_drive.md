
# Improper Input Validation in GazeboRosDiffDrive Allows Malformed `/cmd_vel` Input to Propagate into Motion Control and Odometry

## Overview

The Gazebo ROS Diff Drive plugin in `gazebo_ros_pkgs` contains an improper input validation vulnerability in `gazebo_plugins/src/gazebo_ros_diff_drive.cpp`. Crafted malformed `geometry_msgs::Twist` messages received on `/cmd_vel` are written directly into internal motion control state without finite-value or range validation, allowing untrusted values to propagate through wheel velocity computation, joint control, odometry integration, and `/odom` publication. This may cause unintended, unstable, or uncontrollable robot motion and corrupted odometry output.

## Affected Component

- Repository: `https://github.com/ros-simulation/gazebo_ros_pkgs/blob/noetic-devel/gazebo_plugins/src/gazebo_ros_diff_drive.cpp`


## Vulnerability Type

- CWE-20: Improper Input Validation

## Attack Vector

An attacker can exploit this issue by publishing a crafted malformed `geometry_msgs::Twist` message to the ROS topic `/cmd_vel`.

## Example:

```bash
ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "{linear: {x: nan, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0}}"
```

Test video: https://github.com/REYu6/ROS-vul/blob/main/Gazebo%20CVE/260328211736.mp4
