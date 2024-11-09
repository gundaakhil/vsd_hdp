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
$ brew install graphviz xdot neovim macvim
$ cd yosys
$ git submodule update --init
$ make
$ ./yosys
```

![yosys](/images/Day0/yosys.png)

## Iverilog

```
$ brew install icarus-verilog
$ iverilog
```

![iverilog](/images/Day0/iverilog.png)

## GTKWave

```
$ brew install gtkwave
$ gtkwave
```

![gtkwave](/images/Day0/gtkwave.png)

## Introduction - Digital VLSI SoC Design and Planning

The following stages are involved in Digital VLSI SoC design and Planning:

- **Chip Modelling** - The planning starts with defining the chip. The design specifications are described in testbench (C/C++ based). This step is concluded by verifying the testbench using appropriate compilers.
- **Register-transfer level (RTL) Designing** - This stage involves creating a soft copy of the hardware, by converting c model to register-transfer level model using verilog and validate that design specifications are met.
- **Synthesis** - This step involves converting RTL verilog code (synthesizable code) to Gate Level Netlist which describes the design using logic gates. Macros (Reusable blocks in the design, this also require synthesizable code) and Analog IP (Block connecting with real analog world, this required functional RTL code).
- **SoC Integration** - Integrates all the netlist and blocks synthesized from RTL code into a single model (processor, memory, clock, analog IP, etc).
- **Physical Design** - Physical design converted logical connectivity of cells into physical connectivity. This step involves, Partitioning, Floorplanning, Power Planning, Placement, Clock Tree Synthesis, Routing and More.
- The final GDSII (Layout) generated from Netlist goes through verifications (DRC and LVS) steps before fabrication process (tapeout).
- The cycle is completed by testing the final chip from fab using the same testbench designed in planning phase to ensure the working the hardware chip.

</details>

<details>

 <summary>Day 1 - Introduction to Verilog RTL Design and Synthesis</summary>

## Overview

Day 1: RTL design and synthesis using Iverilog, GTKWave and Yosys with SKY130 Technology

### 1. Introduction to Open-Source Simulator Iverilog

- Simulator: A tool which is used to verify the RTL (Register Transfer Level) design and the tool used for this course is Iverilog.
- Design: Verilog code or a group of Verilog codes that, in order to satisfy the necessary requirements, implement the intended functionality.
- Testbench: A setup in which stimulus (test vectors) are applied to the design in order to verify its functionality.

#### How Simulator Works

- The simulator evaluates the ouput only when there is a change in the inputs.
- If no input changes, there is no change in output.

#### Test Design Structure

- The design may have one or more primary inputs and one or more primary outputs.
- The testbench does not have primary inputs or outputs.<br>
![iverilogDesignStructure](/images/Day1/iverilogDesignStructure.png)

#### Iverilog-Based Simulation Flow

- Iverilog is given both the design and the testbench.
- A VCD (Value Change Dump) file created by Iverilog is sent to GTKWave for waveform display.<br>
![iverilogSimulationFlow](/images/Day1/iverilogSimulationFlow.png)

### 2. Labs using Iverilog and GTKWave

#### Steps:
 - Clone the repository - git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git <br>
![gitClone](/images/Day1/sky130GitClone.png)

#### Key Points:
 - my_lib: Directory contains all the standard cell files used for synthesis
 - lib: Directory with Sky130 Standard Cell library used for synthesis
 - verilog_files: Directory contains all the verilog codes, along with their testbench, for the standard cells used in lib.<br>
![files](/images/Day1/filesSky130.png)

#### Lab Steps:
 - Load the Multiplexer and its testnech into iverilog, then execute the a.out file which is generated through iverilog. This will generate vcd file.<br>
![iverilogLoad](/images/Day1/iverilogLoad.png)
 - Load the .vcd file into the GTKWave for visulaization and functional verification.<br>
![gtkwaveLoad](/images/Day1/gtkwaveLoad.png)

### 3. Introduction to Yosys and Logic Synthesis

#### Introduction to Yosys
 - Synthesizer tool converts verilog RTL design code into gate-level netlist.
 - Yosys is a open source synthesizer tool used for coverting the RTL code to netlist.
 - Netlist represents the design in terms of standard cells from the library (.lib) file.<br>
![yosysSetup](/images/Day1/yosysSetup.png)
 - read_verilog: Command will read the design verilog file.
 - read_liberty: Command will read the standard cell library file.
 - write_verilog: Command will generate the netlist file.

#### Verification of Synthesis
 - The generated netlist can be verified using iverilog. 
 - The primary inputs and outputs of the synthesized netlist remain the same as in the RTL design, meaning the same testbench can be used.
 - The synthesis simulation should match with the RTL simulation.<br>
![synthesisVerify](/images/Day1/synthesisVerify.png)

#### Introduction to Logic Synthesis
 - <strong>RTL Design:</strong> Behavioral representation of the required design specification written in verilog HDL<br>
![rtl](/images/Day1/RTLdesign.png)
 - <strong>Synthesis:</strong> Conversion of RTL verilog code to gate level represenatation with logic gates and interconnects.
 - <strong>Netlist:</strong> Consisting of logic gates and interconnects generated through synthesis process.<br>
![syn](/images/Day1/Synthesis.png)
 - <strong>.lib:</strong> A collection of logical modules, including basic logic gates (e.g., AND, OR, NOT). Different flavors of the same gate exist (e.g., slow, medium, fast versions, 2input, 3input, 4input).
 - <strong>Need of different falours of gate:</strong> Faster and slower cells have different useage in the design to meet different design specifications.<br>
![faster](/images/Day1/Need1.png)
![slower](/images/Day1/Need2.png)
![diff](/images/Day1/fastslow.png)
 - <strong>Selection of cells:</strong> It is important to guide the synthesizer to select the flavour of cells that is optimum for the implementation of logic circuit. More use of faster cells may lead to bad circuit interms of power and area and hold time violations, more use of slower cells will lead to performace degardation and circuit becomes sluggish. All this guidance provided to the synthesizer through "Constraints".
 - <strong>Synthesis illustration:</strong> <br>
![syn2](/images/Day1/synillustration.png) 

### 4. Labs using Yosys and SKY130 PDKs

#### Lab steps:
 - Use of Iverilog to simulate RTL and Yosys for generating netlist.
 - Visualize waveform from both RTL and synthesis simulation and analyze sythezied designs
 - Invoke yosys to the sythesis process in the terminal <br>
![yosys](/images/Day0/yosys.png)
 - Read liberty files consisting of library files <br>
![readLib](/images/Day1/readLib.png)
 - Reading verilog design for verification <br>
![verifyDesign](/images/Day1/verifyread.png)
 - Now Run synthesis process on the top-level module <br>
 `synth -top good_mux` <br>
![synTopLevel](/images/Day1/synTopLevel.png)
 - Perform logic optimization using ABC command. This command will convert the RTL design to gate level with the cells, I/O singals provided in standard cell library <br>
 `abc -liberty ../sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v` <br>
![abc](/images/Day1/abc.png)
 - Use show command to view graphical representation of design <br>
 `show` <br>
![show1](/images/Day1/show1.png) <br>
![show2](/images/Day1/show2.png)
 - Write Verilog Netlist file <br>
 `write_verilog good_mux_netlist.v or write_verilog -noattr good_mux_netlist.v`
 `!gvim good_mux_netlist.v` <br>
![gvim](/images/Day1/gvim.png) 

</details>

<details>
<summary>Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles</summary>

## Overview

Day 2: Understanding timing libraries (.libs), the difference between hieratchical and flat synthesis, and optimizing flip-flop coding techniques. These ideas are essential for enhancing RTL design and synthesis procedures.

### 1. SKY130RTL D2SK1 - Introduction to timing .libs

#### Introduction to timing .libs
 - The breakdown of SkyWater 130nm process design kit's (PDK) library named `sky130_fd_sc_hd__tt_025C_1v80.lib` is - 
 - <strong>Sky130</strong>: 130nm technology developed by SkyWater.
 - <strong>fd_sc_hd</strong>: Fully depleted, high density standard cell library.
 - <strong>tt_025C</strong>: Indicates typical-typical process corner at temperature of 25°C.
 - <strong>1v80</strong>: Nominal operating supply voltage of 1.8 Volts.
 - <strong>PVT</strong>: Process-voltage-temperature variation is a term used to describe the three main sources of variation in circuit design: process, voltage, and temperature. PVT variations can affect a circuit's timing, power, and noise characteristics. PVT variations are caused by the unpredictability of process parameters during the fabrication of an integrated circuit (IC). Some of the parameters include: Oxide thickness, Channel length, Channel width, Impurity concentration densities, and Diffusion depths.

 #### Basic Info from Library
 ![libInfo](/images/Day1/libInfo.png)

 #### Cell `sky130_fd_sc_hd__a2111o_1`:
  - Indicates a 2-input AND into first input of 4-input OR. `X = ((A1 & A2) | B1 | C1 | D1)`
  - <strong>a</strong>: AND gate
  - <strong>o</strong>: OR gate
  - <strong>2111</strong>: Two inputs (A1, A2) go into an AND gate. The result feeds into the first input of a 4-input OR gate (A1&A2, B1, C1, D1). <br>
![logicCell](/images/Day1/logicCell.png)

#### Other observations
 - It also tell about the cell's 32 (5 input) combinations of leakage power values, area, power port info, description of capacitance, power, max_transistion of each pin, timing information.
 - As the number of gates increases,a larger cell is employing wider transistors, the area also increases.
 - Wider cells are faster but require larger areas and consume more power.
 - Smaller cells have more delay but require less area and consume less power. <br>
![compareCells](/images/Day1/compareCells.png)

### 2. SKY130RTL D2SK2 - Hierarchical vs Flat Synthesis

#### Hierarchical vs Flat Synthesis

</detail>
