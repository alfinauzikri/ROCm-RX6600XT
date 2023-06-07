# AMD ROCm Installation Guide on RX 6600 XT + TensorFlow and PyTorch

## What is ROCm?
ROCm is an open-source stack for GPU computation. ROCm is primarily Open-Source Software (OSS) that allows developers the freedom to customize and tailor their GPU software for their own needs while collaborating with a community of other developers, and helping each other find solutions in an agile, flexible, rapid and secure manner. [Read More..](https://rocm.docs.amd.com/en/latest/rocm.html)

## Installation Prerequisites
In this guide, I will install **AMD ROCm** + **TensorFlow** and **PyTorch** on **Ubuntu 22.04 LTS**

### Make Sure It's Up to Date
1. Run the following commands:
```
sudo apt update && sudo apt upgrade
```
2. Reboot if necessary or continue to the next step.

### Setting Permissions for Groups
This section provides steps to add any current user to a video group to access GPU resources.
1. To check the groups in your system, issue the following command:
2. Add yourself to the `render` or `video` group using the following instruction:
```
sudo usermod -a -G render $LOGNAME
sudo usermod -a -G video $LOGNAME
```
3. Use of the video group is recommended for all ROCm-supported operating systems.

To add all future users to the video and render groups by default, run the following commands:
```
echo 'ADD_EXTRA_GROUPS=1' | sudo tee -a /etc/adduser.conf
echo 'EXTRA_GROUPS=video' | sudo tee -a /etc/adduser.conf
echo 'EXTRA_GROUPS=render' | sudo tee -a /etc/adduser.conf
```

## ROCm Installation

To download and install the `amdgpu-install` script on the system, run the following commands:
```
sudo apt update
wget https://repo.radeon.com/amdgpu-install/5.5.1/ubuntu/jammy/amdgpu-install_5.5.50501-1_all.deb
sudo apt install ./amdgpu-install_5.5.50501-1_all.deb
```

We will install ROCm with `Single-version Installation` mode, run the following commands:

```
sudo amdgpu-install --usecase=hiplibsdk,rocm
```

After everything has been done, `reboot` your system.

## Docker Engine Installation
1. Install Docker Engine using bash script, run the following commands:
```
curl -sSL https://get.docker.com/ | sudo sh
```
2. The Docker daemon binds to a Unix socket, not a TCP port. By default it’s the root user that owns the Unix socket, and other users can only access it using `sudo`. The Docker daemon always runs as the `root` user. Run the following commands:
```
sudo groupadd docker
```
```
sudo usermod -aG docker $USER
```
```
newgrp docker
```

## TensorFlow Installation
1. Pull the latest public TensorFlow Docker image.
```
docker pull rocm/tensorflow:latest
```
3. Once you have pulled the image, run it by using the command below:
```
docker run -it --network=host --device=/dev/kfd --device=/dev/dri --ipc=host --shm-size 16G --group-add video --cap-add=SYS_PTRACE --security-opt seccomp=unconfined rocm/tensorflow:latest
```

## PyTorch Installation
1. Pull the latest public PyTorch Docker image.
```
docker pull rocm/pytorch:latest
```
3. Once you have pulled the image, run it by using the command below:
```
docker run -it --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --device=/dev/kfd --device=/dev/dri --group-add video --ipc=host --shm-size 8G rocm/pytorch:latest
```

## Tips and Tricks
Make sure before running any program in either TensorFlow or PyTorch, run the following command:
```
HSA_OVERRIDE_GFX_VERSION=10.3.0
```
## Results
![Screenshot from 2023-06-07 12-18-21](https://github.com/alfinauzikri/ROCm-RX6600XT/assets/14070303/603534a7-6571-4c86-bda6-1ae0575ec71a)
![Screenshot from 2023-06-07 12-28-51](https://github.com/alfinauzikri/ROCm-RX6600XT/assets/14070303/c831e0f5-81ac-4901-bcb2-388767cdc778)

