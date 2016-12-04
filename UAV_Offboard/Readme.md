# OFFBOARD CONTROL
Boilerplate code for SITL Offboard control in ROS/Gazebo
## Installation
I'm assuming you have gone through the install guide: https://github.com/jkrs/EEC193/blob/master/InstallGuide.md

## Pre-setup
Create and initialize a catkin workspace: http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment

# Steps to run: (Ctrl+Shift+T opens new terminal tab in Ubuntu)

## Terminal 1:
```
cd ~/catkin_ws/
source devel/setup.bash
catkin_make
roslaunch UAV_Offboard px4Local.launch 
```

## Terminal 2:
```
cd ~/src/Firmware/ 
make posix_sitl_default gazebo
```

## Terminal 3:
```
cd ~/catkin_ws/
source devel/setup.bash
rosrun UAV_Offboard offb_node
```
The drone should enter offboard control and then takeoff.
![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/takeoff.gif)

## Manually Publish Waypoints
A graph plot shows the structure of the ROS nodes, topics, and messages.
Open a new terminal and execute:
```
rosrun rqt_graph rqt_graph
```

![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/rqt_graph.PNG)
![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/rqt_pub_settings.png)
![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/pos_control.gif)

# Troubleshooting
If you run into an issue where the drone drifts from the held position, this is the solution: http://discuss.px4.io/t/solved-px4-sitl-position-hold-not-working/1842

# ROS Resources
This sample code is provided just as a "boiler plate" example that can be extended.  You aren't going to be able to do much if you don't have a basic understanding of how ROS works.  At a minimum, you should go through the beginner tutorials here: http://wiki.ros.org/ROS/Tutorials .  This book is also a really good introduction: https://cse.sc.edu/~jokane/agitr/







