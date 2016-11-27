## Prereqs: 
Computer running Ubuntu 16.04

## Step 1: Install the px4 toolchain prereqs
http://dev.px4.io/starting-installing-linux.html
http://dev.px4.io/starting-installing-linux-boutique.html

Run
```
sudo usermod -a -G dialout $USER
```
This adds your user to the dialout group

Then run 
```
sudo add-apt-repository ppa:george-edison55/cmake-3.x -y
sudo apt-get update
sudo apt-get install python-argparse git-core wget zip \
    python-empy qtcreator cmake build-essential genromfs -y
# simulation tools
sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev 
```
This modem manager apparently interferes with robotics related applications so just remove it
```
sudo apt-get remove modemmanager
```
Then run this
```
sudo apt-get remove gcc-arm-none-eabi gdb-arm-none-eabi binutils-arm-none-eabi
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
sudo apt-get update
sudo apt-get install python-serial openocd \
    flex bison libncurses5-dev autoconf texinfo build-essential \
    libftdi-dev libtool zlib1g-dev \
    python-empy gcc-arm-embedded -y
 ```
Check if your installation was successful
```
arm-none-eabi-gcc --version
```
If the output doesn't look like this
```
arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 4.7.4 20140401 (release) [ARM/embedded-4_7-branch revision 209195]
Copyright (C) 2012 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
Then you need to do a bare metal installation
(If the output is correct, jump to step 2 directly)
```
pushd .
cd ~
wget https://launchpad.net/gcc-arm-embedded/5.0/5-2016-q2-update/+download/gcc-arm-none-eabi-5_4-2016q2-20160622-linux.tar.bz2
tar -jxf gcc-arm-none-eabi-5_4-2016q2-20160622-linux.tar.bz2
exportline="export PATH=$HOME/gcc-arm-none-eabi-5_4-2016q2/bin:\$PATH"
if grep -Fxq "$exportline" ~/.profile; then echo nothing to do ; else echo $exportline >> ~/.profile; fi
. ~/.profile
popd
```
Now run the following to install 32-bit libraries (only do this if you are running a 64-bit system)
```
sudo apt-get install libc6:i386 libgcc1:i386 gcc-4.6-base:i386 libstdc++5:i386 libstdc++6:i386
```
Run the version check again 
```
arm-none-eabi-gcc --version
```
and check that you have the correct output

This installation step is optional but recommended, all it does is update cmake so it builds shit faster
```
mkdir -p $HOME/ninja
cd $HOME/ninja
wget https://github.com/martine/ninja/releases/download/v1.6.0/ninja-linux.zip
unzip ninja-linux.zip
rm ninja-linux.zip
exportline="export PATH=$HOME/ninja:\$PATH"
if grep -Fxq "$exportline" ~/.profile; then echo nothing to do ; else echo $exportline >> ~/.profile; fi
. ~/.profile
```
## Step 2 Install ROS Kinetic
Follow the instructions on the wiki. You need to do a full desktop install. You do not have to mess around with any dependencies before the installation. 
[ROS Kinetic Installation Instructions](http://wiki.ros.org/kinetic/Installation/Ubuntu)

## Step 3 Install MAVROS
After setting up ROS, you can install MAVROS with a binary installation
```
$ sudo apt-get install ros-kinetic-mavros ros-kinetic-mavros-extras
```
## Step 4 Install Gazebo7
http://gazebosim.org/tutorials?tut=install_ubuntu&ver=7.0&cat=install

1 .Install
```
    curl -ssL http://get.gazebosim.org | sh
```
2. Run
```
    gazebo
```

## Step 5 Fork the PX4 Firmware
Reference section: http://dev.px4.io/starting-building.html
http://dev.px4.io/simulation-gazebo.html
Create a directory where you want to develop then run 
```
mkdir -p ~/src
cd ~/src
git clone https://github.com/PX4/Firmware.git
cd Firmware
git submodule update --init --recursive
cd ..
```
to fork the PX4 development code into your directory

Run this command to build the code
```
cd Firmware
make px4fmu-v2_default
```
If this errors you fucked up somewhere, try re-forking the PX4 repo. Your output should look like this.

```
[100%] Linking CXX executable firmware_nuttx
[100%] Built target firmware_nuttx
Scanning dependencies of target build_firmware_px4fmu-v2
[100%] Generating nuttx-px4fmu-v2-default.px4
[100%] Built target build_firmware_px4fmu-v2
```
Gazebo 7 needs libignition-math2-dev.
```
sudo apt-get update
sudo apt-get install libignition-math2-dev
```

Now you should be good to go to simulate. Run this command to start a basic simulation.
```
make posix_sitl_default gazebo
```
If the build is successful then Gazebo should automatically open up, and the terminal will enter into the PX4 shell.
```
[init] shell id: 140735313310464
[init] task name: px4

______  __   __    ___
| ___ \ \ \ / /   /   |
| |_/ /  \ V /   / /| |
|  __/   /   \  / /_| |
| |     / /^\ \ \___  |
\_|     \/   \/     |_/

px4 starting.


pxh>
```
Note that it will take a while for Gazebo to first setup as it has to generate all the models.
After Gazebo is done loading, you can check that you've interfaced to the simulator properly by having the drone takeoff. Run this command in the terminal window with the PX4 shell
```
commander takeoff
```
After running this command in the terminal, the drone should takeoff in Gazebo.

You can download QGroundControl as well. If the simulator is on, it will automatically connect to the simulated drone. You can fly missions and do all the normal QGC things from there.

## Step 6:  Install Qgroundcontrol
Use appimage found here: https://donlakeflyer.gitbooks.io/qgroundcontrol-user-guide/content/download_and_install.html
Give the dowloaded file execution permissions
```
chmod +x <filename>
```
## Run an offboard SITL: https://github.com/jkrs/EEC193/tree/master/UAV_Offboard
