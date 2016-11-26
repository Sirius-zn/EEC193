
## Prereqs: Compute running Ubuntu 14.04 and up (Do not use VMs as the simulator is too slow, just dualboot if required)

## Step 1: Install the px4 toolchain prereqs
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
Now run to install 32-bit libraries (only do this if you are running a 64-bit system)
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
## Step 2 Install ROS Indigo
Follow the instructions on the wiki. You need to do a full desktop install. You do not have to mess around with any dependencies before the installation. 
[ROS Indigo Installation Instructions]http://wiki.ros.org/indigo/Installation/Ubuntu
