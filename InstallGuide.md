
## Prereqs: Compute running Ubuntu 14.04 and up (Do not use VMs as the simulator is too slow, just dualboot if required)

## Step 1: Install the px4 toolchain prereqs
Run
```
sudo usermod -a -G dialout $USER
```
This adds your use to the dialout group

Then run 
```
sudo add-apt-repository ppa:george-edison55/cmake-3.x -y
sudo apt-get update
sudo apt-get install python-argparse git-core wget zip \
    python-empy qtcreator cmake build-essential genromfs -y
# simulation tools
sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev 
```

