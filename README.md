# APAI-LAB: PULP Virtual Platform Setup and C programming


### **How the lab session works**:
- Every student must setup their own simulation environment on his/her laptop (next slides)
- We will go through some code examples to learn in practice some basic concepts discussed during the theoretical classes. Plus lab assignment(s) to try on your own

Requirements
- C programming (this is NOT a C programming course!)
- Familiarity with UNIX/Linux (Ubuntu) environment

### Device 
The target device for the lab sessions is the multi-core [PULP](https://github.com/pulp-platform/pulp) platform. 
The PULP Virtual Platform simulator GVSOC, which is included within the [PULP SDK](https://github.com/pulp-platform/pulp-sdk), will be used during the class. 
To install the GVSOC Simulator and setup your environment, you can follow one of the options described below. 

# 1. Setup
pre-requisites to be installed before the lab session

## GVSOC Simulator Install Guide

### *Option 1 (RECOMMENDED)*: Download the PULP Virtual Machine
The Virtual Machine includes a pre-installed PULP SDK and Virtual Platform simulator.

The machine can be used in VirtualBox, VMware (instructions below) or other hosts (not tested - you are on your own...).

**FIRST OF ALL:** Download **pulp_box_2021_dory.ova** from this OneDrive [link](https://liveunibo-my.sharepoint.com/:u:/g/personal/alessio_burrello_unibo_it/EYDij6QsMKFBp7pOJx5eQAwBG2FHH59c9fs9a4eorDd9ew?e=f8bJ0O) (8.1 GB)
    * The link works with *unibo* accounts. 

**If you want to use VMware Workstation Player 16**:
1. Install and open [**VMware Workstation Player 16**](https://www.vmware.com/it/products/workstation-player/workstation-player-evaluation.html). Notice that "Player" is the free version of WMware, while "Pro" must be paid.
2. Go to Player> File > open ...
3. Select pulp_box_2021_dory.ova as the Appliance to import
4. If needed, change the destination folder, then click on Import
5. If WMware gets you a warning, force the import of the VM image provided
6. In the main WMware window, click on the new virtual machine, then on "Play virtual machine"


**Alternatively, If you want to use VirtualBox**:
1. Install and open [**VirtualBox**](https://www.virtualbox.org/)
2. Go to File > Import Appliance...
3. Select pulp_box_2021_dory.ova as the Appliance to import
4. If needed, change the destination folder, then click on Import
5. In the main VirtualBox window, click on the new virtual machine, then on Start

**VM info:**
The username in the virtual machine is _pulp-user_, while the password is _pulp_. Check the README file on the Desktop.
To make the SDK and GVSOC available in a newly opened shell, use the following command:
~~~~~shell
source /pulp/sourceme.sh
~~~~~

### *Option 2 (alternative)*: Install the PULP-SDK on your Linux machine
If you prefer to use your own Linux machine, you can follow the instructions [here](https://github.com/pulp-platform/pulp-sdk#getting-started), which have been tested on a fresh Ubuntu 18.04 Bionic Beaver 64-Bit machine.

**_Beware_** that if you prefer to use your own machine, you may encounter issues for which we cannot easily support you -- especially if your distro is different from Ubuntu 18.04 64-bit.
The main steps concern:
1. Install the compiler. A precompiled version can be taken from [here](https://github.com/pulp-platform/pulp-riscv-gnu-toolchain/releases/tag/v1.0.16).

2. Export the RISC-V compiler path.
~~~~~shell
export PULP_RISCV_GCC_TOOLCHAIN=<INSTALL_DIR>
~~~~~

3. Clone and build the PULP SDK
~~~~~shell
git clone https://github.com/pulp-platform/pulp-sdk.git
cd pulp-sdk
source configs/pulp-open.sh
make build
~~~~~

### *Option 3 (alternative)*: Install the PULP-SDK in a Docker container
To install the PULP-SDK inside a Docker container running Ubuntu 18.04, follow these steps:

1. Install `docker` and `docker-compose` on your host machine.
If you use on a Debian-based Linux distro, you can do it as follows:
~~~~~shell
sudo apt install docker docker-compose
~~~~~

2. Add your user to the `docker` group to avoid having to use `sudo`.
~~~~~shell
sudo usermod -aG docker $USER
~~~~~
Then restart your system.

3. Run the container (NB the first run may take several minutes, as it needs to build the Docker image).
~~~~~shell
docker-compose run --rm pulp
~~~~~
A new shell will open inside the container, in the `/home/pulp/workspace` folder (you're the `pulp` user); such folder is a volume, meaning that you will see it on your host too (you can share files between the host and the container using such folder).

4. After terminating the container (with Ctrl+D), close the resources.
~~~~~shell
docker-compose down
~~~~~

# 2. Test your GVSOC Installation
After completing the GVSOC setup, you can download some example code in your preferred working directory and run the _Helloworld_ on the PULP platform.

1. Download this repo in your home
~~~~~shell
cd <your_work_directory>
git clone https://github.com/EEESlab/APAI22-LAB-VM-Setup
~~~~~

2. test environment
~~~~~shell
cd <your_work_directory>
cd APAI22-LAB-VM-Setup/
source /pulp/sourceme.sh
make clean all run
~~~~~
If you see an *Hello* from PULP, your setup is fine! ;)

# 3. Additional Information
### Source the PULP SDK every time you open a new shell
To run example code on the PULP Virtual Plaform, you must export the RISC-V Compiler path and source the platorm configurafion file every time a shell is opened.

If you are using the PULP Virtual Machine, we prepared a bash script that does this for you. For each new shell, all you need to do is running:
~~~~~shell
source /pulp/sourceme.sh
~~~~~
### if you are not using the VM we provided:
If you installed the VM on your own, you will not find the sourceme.sh file. You must run the following commands manually each time you open a new shell: 

~~~~~shell
export PULP_RISCV_GCC_TOOLCHAIN=<INSTALL_DIR>
source configs/pulp-open.sh
~~~~~

You can consider to prepare a bash script (equivalent to the sourceme.sh) that you will use to automatically run these commands.

# 4. Read about Linux basics
[UNIX_Linux,C_basics_GIT.pdf](https://github.com/EEESlab/APAI22-LAB-VM-Setup/blob/main/UNIX_Linux%2CC_basics_GIT.pdf)


# Help
You may need to change screen resolution size and keyboard language
- Use `xrandr` to adjust the screen resolution: [link](https://www.linuxfordevices.com/tutorials/linux/change-screen-resolution)
- You may need to change language of the keyboard: [link](https://support.hp.com/ee-en/document/c04948046)

# Troubleshooting

#### 1. Error: `Illegal instruction (core dumped)`
Try building again the `pulp-sdk`
```
cd /pulp-pulp-sdk
rm -rf build/
rm -rf install
make build
```

