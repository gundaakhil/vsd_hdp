# VLSI System Design - Hardware Design Program

<details>

 <summary>Day 0 - Tools Installation</summary>

## System Details

- **RAM**: 8GB
- **Storage**: 256GB SSD
- **OS**: macOS

## Yosys

```
$ git clone https://github.com/YosysHQ/yosys.git
$ brew install cmake gcc gawk tcl-tk libtool bison flex make
$ brew install graphviz
$ cd yosys
$ git submodule update --init
$ make
$ yosys --version
```
```
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make 
$ sudo make install
```

Getting Some Issue - Need to show

## Iverilog

```
brew install icarus-verilog
iverilog
```
![iverilog](/images/Day0/iverilog.png)

## GTKWave

```
brew install gtkwave
gtkwave
```
![gtkwave](/images/Day0/gtkwave.png)

## Introduction - Digital VLSI SoC Design and Planning

The following stages are involved in Digital VLSI SoC design and Planning:

* **Chip Modelling** - The planning starts with defining the chip. The design specifications are described in testbench (C/C++ based). This step is concluded by verifying the testbench using appropriate compilers.
* **Register-transfer level (RTL) Designing** - This stage involves creating a soft copy of the hardware, by converting c model to register-transfer level model using verilog and validate that design specifications are met.
* **Synthesis** - This step involves converting RTL verilog code (synthesizable code) to Gate Level Netlist which describes the design using logic gates. Macros (Reusable blocks in the design, this also require synthesizable code) and Analog IP (Block connecting with real analog world, this required functional RTL code).
* **SoC Integration** - Integrates all the netlist and blocks synthesized from RTL code into a single model (processor, memory, clock, analog IP, etc).
* **Physical Design** - Physical design converted logical connectivity of cells into physical connectivity. This step involves, Partitioning, Floorplanning, Power Planning, Placement, Clock Tree Synthesis, Routing and More.
* The final GDSII (Layout) generated from Netlist goes through verifications (DRC and LVS) steps before fabrication process (tapeout).
* The cycle is completed by testing the final chip from fab using the same testbench designed in planning phase to ensure the working the hardware chip.

</details>
