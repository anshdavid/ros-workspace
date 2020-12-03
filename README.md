ROS-WORKSPACE
=============

My Ros Neotic workspace. [Ubuntu install instructions][__NOETIC__]

# ROS

Repository contains ubuntu ros packages to work with :

1. [Jackal][__JACKAL__]
2. [Spot][__SPOT__]
3. [Turtlebot3][__TURTLEBOT3__]

Addition packages and dependencies are required to work with

1. [Husky][__HUSKY__]
2. [Turtlebot][__TURTLEBOT__]

# UNITY3D ROS SHARP

Repository contains `file_server` and `unity_simulation_scene` packages required to establish connection with unity3D for robotics simulation. 
> Refer to [repo][__GIT_UNITYSIM__] for setup

## Jackal communiation launch

`roslaunch unity_simulation_scene unity_jackal_scene.launch`

## Turtlebot3 communiation launch

`roslaunch unity_simulation_scene unity_turtlebot3_scene.launch`

# Development Setup

1. [Containerized development setup instruction for LXC/LXD][__DOC_LXD__]
2. [Containerized development setup instruction for Docker][__DOC_DOCKER__]

---

### Directory structure

    .
    ├── docs
    └── robot_ws
        └── src
            ├── file_server
            ├── gazebo_simulation_scene
            ├── unity_simulation_scene
            ├── robot
            │   ├── husky
            │   │   ├── husky_base
            │   │   ├── husky_bringup
            │   │   ├── husky_control
            │   │   ├── husky_description
            │   │   ├── husky_desktop
            │   │   ├── husky_gazebo
            │   │   ├── husky_msgs
            │   │   ├── husky_navigation
            │   │   ├── husky_robot
            │   │   ├── husky_simulator
            │   │   └── husky_viz
            │   ├── jackal
            │   │   ├── jackal_control
            │   │   ├── jackal_description
            │   │   ├── jackal_msgs
            │   │   ├── jackal_navigation
            │   │   └── jackal_tutorials
            │   ├── spot_ros
            │   │   ├── spot_description
            │   │   ├── spot_driver
            │   │   ├── spot_msgs
            │   │   └── spot_viz
            │   ├── turtlebot
            │   │   ├── create_description
            │   │   ├── kobuki_description
            │   │   └── turtlebot_description
            │   └── turtlebot3
            │       ├── turtlebot3
            │       ├── turtlebot3_bringup
            │       ├── turtlebot3_description
            │       ├── turtlebot3_example
            │       ├── turtlebot3_msgs
            │       ├── turtlebot3_navigation
            │       ├── turtlebot3_simulations
            │       ├── turtlebot3_slam
            │       └── turtlebot3_teleop
            ├── ros_pkgs
            │   └── teleop_twist_joy
            │       ├── config
            │       ├── include
            │       ├── launch
            │       ├── src
            │       └── test
            └── vendor
                ├── LMS1xx
                └── pointgrey_camera_driver
    <!-- tree . -d -L 5 -->

---

[__NOETIC__]: https://wiki.ros.org/noetic/Installation/Ubuntu
[__JACKAL__]: https://github.com/jackal/jackal
[__SPOT__]: https://github.com/clearpathrobotics/spot_ros
[__TURTLEBOT3__]: https://github.com/ROBOTIS-GIT/turtlebot3
[__HUSKY__]: https://github.com/husky/
[__TURTLEBOT__]: https://github.com/turtlebot/turtlebot
[__GIT_UNITYSIM__]: https://github.com/ironWolf1990/unity-ros-sharp
[__DOC_LXD__]: ./docs/lxd.md
[__DOC_DOCKER__]: ./docs/docker.md
