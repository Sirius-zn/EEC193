# EEC193
Clone this repo in your catkin_ws

cd ~/catkin_ws/src
https://github.com/jkrs/EEC193.git

I'm assuming you've cloned the PX4 toolchain in: ~/drone/src/Firmware/
if not, then do so...


Steps to run: (Ctrl+Shift+T opens new terminal tab)

Terminal 1:
cd ~/catkin_ws/
source devel/setup.bash
catkin_make
roslaunch UAV_Offboard px4Local.launch 

Terminal 2:
cd ~/drone/src/Firmware/ #go to cloned px4 firmware directory
make posix_sitl_default gazebo

Terminal 3:
cd ~/catkin_ws/
source devel/setup.bash
rosrun UAV_Offboard offb_node
