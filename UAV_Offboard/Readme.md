# GETTING STARTED WITH OFFBOARD CONTROL
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
### Terminal 4:
```
rosrun rqt_graph rqt_graph
```
One can see that the node **offb_node** subscribes to the topic **/mavros/state** and publishes to the topic **/mavros/setpoint_position/local**
![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/rqt_graph.PNG)
We can use **rqt_publisher** to manually publish our own waypoints to the **/mavros/setpoint_position/local** topic. Open the rqt_publisher by executing:
```
rosrun rqt_publisher rqt_publisher
```
Configure the window to have the same field values as this:
![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/settings_rqt_publisher.png)
Go to the **Terminal 3** and kill **offb_node** (press control + c).  Now you should be able to control the drone's position by altering the **X Y Z** fields of the position message
![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/pos_control.gif)
One can see the messages being published to the **/mavros/setpoint_position/local** topic by executing:
```
rostopic echo /mavros/setpoint_position/local
```
![alt tag](https://github.com/jkrs/EEC193/raw/master/UAV_Offboard/readme_resources/ros_topic_echo.PNG)

# Troubleshooting
If you run into an issue where the drone drifts from the held position, this is the solution: http://discuss.px4.io/t/solved-px4-sitl-position-hold-not-working/1842

# ROS Resources
This sample code is provided just as a "boiler plate" example that can be extended.  You aren't going to be able to do much if you don't have a basic understanding of how ROS works.  At a minimum, you should go through the beginner tutorials here: http://wiki.ros.org/ROS/Tutorials .  This book is also a really good introduction: https://cse.sc.edu/~jokane/agitr/







