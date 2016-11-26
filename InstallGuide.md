
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
