# MARIO-COM: Mobile Autonomous Robot for Medical Waste Collection

![Robot Simulation](https://img.shields.io/badge/ROS2-Foxy-blue) ![Language](https://img.shields.io/badge/C%2B%2B-17-green) ![Platform](https://img.shields.io/badge/TurtleBot3-Waffle_Pi-orange)

## 🚀 Overview
MARIO-COM is an autonomous robotics solution designed to automate the hazardous process of medical waste collection in hospital environments. By combining a mobile base with a robotic manipulator, the system searches, identifies, picks up, and disposes of bio-hazardous waste bins without human intervention.

## 🛠️ Technical Stack
* **Framework:** ROS2 Foxy (C++ / Python)
* **Simulation:** Gazebo, RViz
* **Navigation:** Nav2 (Navigation 2 Stack), SLAM (Cartographer)
* **Perception:** OpenCV (Image Processing), LiDAR (Laser Scan)
* **Manipulation:** OpenManipulator-X integration
* **Development Tools:** Colcon, Git, Doxygen

## 🏗️ System Architecture

### 1. Perception Logic
Detects medical waste bins using a visual sensor. 
* **C++ Implementation:** Utilizes `cv_bridge` to convert ROS image messages to OpenCV matrices.
* **Algorithm:** Implements color-space filtering and centroid tracking to calculate the bin's relative position.

### 2. Navigation & Path Planning
* **Autonomous Search:** A grid-based search algorithm implemented in the `Navigation` class.
* **Disposal Logic:** Once a bin is "picked," the robot calculates the shortest path to the designated disposal zone using the Nav2 stack.

### 3. Manipulation Module
* **Pick & Place:** Coordinates the OpenManipulator-X arm. 
* **Simulation Pipeline:** Uses Gazebo service calls (`DeleteEntity`/`SpawnEntity`) to handle high-fidelity physics interactions between the gripper and waste bins.

## 📂 Project Structure
* `Navigation.hpp/cpp`: Manages the robot's movement states and search patterns.
* `Perception.hpp/cpp`: Handles CV logic and LiDAR-based obstacle avoidance.
* `Manipulation.hpp/cpp`: Logic for gripper control and entity management.
* `hospital.launch.py`: Primary launch file for the 3D hospital environment, Nav2, and TurtleBot3.

## 🚀 Installation & Usage

### Build
```bash
source /opt/ros/foxy/setup.bash
colcon build --packages-select mario_com
source install/setup.bash


# Set environment variables
export TURTLEBOT3_MODEL=waffle_pi
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:`ros2 pkg prefix mario_com`/share/mario_com/models/

# Launch Hospital World
ros2 launch mario_com hospital.launch.py

# Run Main Controller
ros2 run mario_com my_main