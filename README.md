# Vivado Hermes NoC Demo

This repo contains scripts to recreate [Hermes network-on-chip](https://www.sciencedirect.com/science/article/abs/pii/S0167926004000185) Demo project. It consists of a 2x2 NoC connected to Zynq and 3 toy hardware accelerators.
The project is setup for Zedboard, although it would be easy to change to other boards assuming you have some basic TCL skills.

# Module/IP design

This module has a 2x2 mesh NoC with the following IPs connected to it:
 - address 0x0000) Zynq
 - address 0x0001) Counter + 1
 - address 0x0100) Counter + 2
 - address 0x0101) Counter + 3

![2x2 NoC with counters](noc2x2.png)

The test software called **noc_test** sends packets from the Zynq to each of the counters
and each counter reply with the packet payload incremented by 1, 2, or 3, depending on the 
target counter. The software compares the received packet with the expected one to determine
if it has a match or not.

# How to download it

This repository might have custom IPs included as git submodules. Thus, the following command is required to download all its dependencies.

```
git clone --recursive https://github.com/amamory/<repo-name>.git
```

If you already cloned the repository without `--recursive`, then run the following command to download all the submodules.

```
git submodule update --init --recursive
```

Refer to this [tutorial](https://www.vogella.com/tutorials/GitSubmodules/article.html) to learn how to manage submodules.


# How to run it

These scripts are assuming Linux operation system (Ubuntu 18.04) and Vivado 2018.2.

Follow these instructions to recreate the Vivado and SDK projects:
 - Open the **build.sh** script and edit the first two lines to setup the environment variables:
    - **VIVADO**: path to the Vivado install dir;
    - **VIVADO_DESIGN_NAME**: mandatory name of the design
    - **XIL_APP_NAME**: used only in projects with software. Not used in this design; 
    - **VIVADO_TOP_NAME**: set the top name (optional).  
 - run *build.sh*

These scripts will recreate the entire Vivado project, compile the design, generate the bitstream, export the hardware to SDK, create the SDK projects, import the source files, build all projects, and finally download both the bitstream and the elf application. Hopefully, all these steps will be executed automatically.

# How to update the scripts

These scripts come from a template repository and they get updated and improved over time. If you wish to get the latest script version, then follow these steps:

```
git remote add template https://github.com/amamory/vivado-base-project.git
git fetch --all
git merge --no-commit --no-ff template/master --allow-unrelated-histories
```

Solve any conflict manually and then commit.

# Future work

 - update the scripts to Vitis
 - support or test with Windows (help required !!! :D )

# Credits

The scripts are based on the excellent scripts from [fpgadesigner](https://github.com/fpgadeveloper/zedboard-axi-dma) plus few increments from my own such as project generalization, support to SDK project creation and compilation and other minor improvements. 
