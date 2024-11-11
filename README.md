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

#### Steps

- Clone the repository - git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git <br>
![gitClone](/images/Day1/sky130GitClone.png)

#### Key Points

- my_lib: Directory contains all the standard cell files used for synthesis
- lib: Directory with Sky130 Standard Cell library used for synthesis
- verilog_files: Directory contains all the verilog codes, along with their testbench, for the standard cells used in lib.<br>
![files](/images/Day1/filesSky130.png)

#### Lab Steps

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

#### Lab steps

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
 `write_verilog good_mux_netlist.v or write_verilog -noattr good_mux_netlist.v`<br>
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

#### Cell `sky130_fd_sc_hd__a2111o_1`

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

- Main difference is hierarchical synthesis allows for modular design whereas flat synthesis allows a single-level design.

#### Lab - Using multiple_module.v

![example1](/images/Day1/example1.png)

#### After Synthesis we expect -

![example2](/images/Day1/synBlock.jpeg)

#### Lab Steps - Hierarchical Synthesis

- Invoke Yosys <br>
  `./yosys`
- Read Liberty Files: Loads the library file for synthesis <br>
  `read_liberty -lib ../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.v`
- Read Verilog Files: Load design verilog file <br>
  `read_verilog ../sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v`
- Run Synthesis on Top Level Module: Specify the top level module for yosys synthesis <br>
  `synth -top multiple_modules` <br>
![example3](/images/Day1/example3.png)
![example4](/images/Day1/example4.png)
- ABC Algorithum for Logic Optimization: Logic optimization of synthesized design by utilizing same liberty file. <br>
  `abc -liberty ../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.v`
![abcHS](/images/Day1/abcHS.png)
- Show command: to visualize the synthesized design <br>
  `show`
![showHS](/images/Day1/example5.png)
- Write Verilog Netlist: Generate synthesized netlist verilog file <br>
  `write_verilog -noattr multiple_modules_netlist.v`
![wvHS](/images/Day1/wvHS.png)
![mmnetlist](/images/Day1/mmnetlist.png)
- We can observe that hierarchy is preserved. The sub_module1 and sub_module2 are instantiated separately in the synthesized Verilog netlist rather than seeing AND or OR gate.

**Learning Note**:

- The selection of cells (implementation of design) depends on the synthesis tools/PDKs. The synthesis tool selects appropriate cells based on the design’s needs, balancing speed, area, and power to meet timing requirements effectively.
- In device fabrication, PMOS stacked structures are generally less preferred due to Larger Area Requirement. PMOS devices are generally larger than NMOS devices due to their need for lower doping concentrations to achieve the desired threshold voltage. Stacked PMOS structures therefore require more silicon area, which increases the chip’s footprint and cost. Other limitations include Higher Threshold Voltage (Vth) Requirement, increased delay and slower performance, increased power consumption, poorer substrate control.

#### Lab Steps - Flat Synthesis

- <strong>Flatten:</strong> Eliminates hierarchy and and converts the design to a flat structure. <br>
 `flatten` <br>
![flatten](/images/Day1/flatten.png)
- <strong>Write Flatten Synthesized Design to Netlist Verilog Code:</strong> Eliminates hierarchy and and converts the design to a flat structure. <br>
 `write_verilog -noattr multiple_modules_flat_netlist.v` <br>
 `!gvim multiple_modules_flat_netlist.v` <br>
![flattenBD](/images/Day1/flattenBD.png)
![wvFlatten](/images/Day1/wvFlatten.png)

#### Sub-module Level Synthesis and its necessity

- RTL (Register Transfer Level) designs consists of various functional blocks or sub-modules. Sub-module level synthesis allows each of these sub-modules to be synthesized independently.
- The synthesis tool may efficiently optimize each sub-module by synthesizing them separately. Each sub-module undergoes area minimization, technology mapping, and logic optimization. As a result, the total chip area is decreased and resources are used more effectively.
- Each submodule can be designed, verified, and optimized independently. They can be reused in a large design multiple times saving time and enhancing efficiency.
- Parallel processing increases efficiency by allowing the simultaneous synthesis of several sub-modules. Parallel synthesis greatly shortens turnaround times for large designs.

#### Lab Steps - Sub module synthesis

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![submodule1](/images/Day1/submodule1.png)
![submodule2](/images/Day1/submodule2.png)
![submodule3](/images/Day1/submodule3.png)

### 3. SKY130RTL D2SK3 - Various Flop Coding Styles and Optimization

#### What are Flip-Flops?

- A flip-flop is a type of digital memory circuit that can store one bit of binary data, either a 0 or a 1. Unlike latches, which are level-sensitive, flip-flops are edge-triggered devices, meaning they change states only at specific moments, usually on the rising or falling edge of a clock signal. They form the building blocks for registers, counters, and other timing-dependent component where data needs to be stored, synchronized, or sequenced based on clock cycles.

#### Importance of flops and how do they prevent glitches in the circuit?

- Flops are essential components in digital circuits, providing stable and synchronized data storage and transfer, which is crucial for reliable circuit performance. As edge-triggered devices, flops only update their output in response to specific clock edges (e.g., rising or falling), ensuring data transitions occur at precise, controlled intervals. This timing control synchronizes all elements of the circuit, preventing timing misalignments, noise, and transient variations that could lead to unintended signal fluctuations, or glitches.
- Proper placement of flops during synthesis is equally important, as it minimizes data travel distances and timing delays, reducing overall power consumption and enhancing timing accuracy. Well-placed flops ensure consistent data flow and eliminate spurious outputs, allowing circuits to meet stringent timing requirements, enabling performance optimizations like pipelining, and ensuring energy efficiency through streamlined data handling. <br>
![flopglitch](/images/Day1/flopglitch.jpeg)

#### Different types of Flip-Flops

##### Synchronous Flip-Flops

- Synchronous flip-flops change their state based on a clock signal, meaning they respond only on a specific clock edge (e.g., rising or falling). This synchronization to the clock makes them predictable and easier to control, as all flip-flops in a synchronous circuit change state simultaneously based on the clock cycle. Synchronous flip-flops are widely used in digital systems because they allow for precise timing, which helps maintain consistent data flow and prevents glitches due to uncoordinated state changes.
- Example: D Flip-Flop, JK Flip-Flop
- Application: Used in synchronous circuits like counters, shift registers, and memory storage elements in a processor.

##### Asynchronous Flip-Flops

- Asynchronous flip-flops are not controlled by a clock signal; instead, they change state immediately when the input changes, which is independent of any clock. This behavior makes them faster but also more prone to timing issues, as they respond as soon as an input signal is detected. Asynchronous flip-flops are less common in large digital systems but useful in simple applications where immediate response is essential and timing control is less critical.
- Example: Set-Reset (SR) Latch (which can be converted into an asynchronous SR Flip-Flop)
- Application: Simple state-holding circuits, debounce circuits, or for immediate response requirements in small systems.

##### Set-Reset (SR) Flip-Flops

- Set-Reset (SR) flip-flops, also known as SR latches, are flip-flops with two inputs: Set (S) and Reset (R). These inputs allow the flip-flop to either "set" (output high) or "reset" (output low) its state. Unlike typical flip-flops that respond only to clock edges, SR flip-flops can be either asynchronous or synchronous, depending on whether they’re clocked. However, SR flip-flops have a unique state called the forbidden state (when both S and R are high) where the output becomes unpredictable.
- Example: Basic SR Latch (asynchronous), Clocked SR Flip-Flop (synchronous)
- Application: Used where simple on/off memory states are required, such as temporary storage, control systems, and toggling circuits.

#### Flop Simulations

##### dff_asyncres.v

![asyncrespaper](/images/Day1/asyncrespaper.jpeg)
![asyncrescode](/images/Day1/asyncrescode.png)
![asyncressim](/images/Day1/asyncressim.png)

##### dff_async_set.v

![asyncsetcode](/images/Day1/asyncsetcode.png)
![asyncsetsim](/images/Day1/asyncsetsim.png)

##### dff_syncres.v

![syncrespaper](/images/Day1/syncrespaper.jpeg)
![syncrescode](/images/Day1/syncrescode.png)
![syncressim](/images/Day1/syncressim.png)

#### Flop Synthesis

##### dff_asyncres.v

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![asyncresstat](/images/Day1/asyncresstat.png)
![asyncresshow](/images/Day1/asyncresshow.png)

##### dff_async_set.v

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![asyncsetstat](/images/Day1/asyncsetstat.png)
![asyncsetshow](/images/Day1/asyncsetshow.png)

##### dff_syncres.v

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
stat
show
```

![syncresstat](/images/Day1/syncresstat.png)
![syncresshow](/images/Day1/syncresshow.png)

##### DFF SyncRes Block Diagram Analysis

![syncresanalysis](/images/Day1/syncresanalysis.png)

**NOTE** - The command dfflibmap -liberty <lib_path> is used because every standard cell libary stores flipflops standard cells in a seperate lib space and we need to explicitly mention sythesizer to consider these lib files for flipflop synthesis.

#### Interesting Optimizations

##### Multiply by 2 (mult_2 synthesis)

- To implement y[3:0] = 2*a[2:0], we append a 1'b0 to the a[2:0] i.e, y[3:0] = {a[2:0],0}. This is also equal to left shift the input bits by 1. This can be realized by just wiring and expect no hardware is required in this case. The command 'abc' is not required for mapping when there are no cells.

```
module mul2 (input [2:0] a, output [3:0] y);
	assign y = a * 2;
endmodule
```

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mul2_netlist.v
```

![mul2stat](/images/Day1/mul2stat.png)
![mul2show](/images/Day1/mul2show.png)
![mul2netlist](/images/Day1/mul2netlist.png)

##### Multiply by 9 (special case synthesis)

- To implement y[5:0] = 9*a[2:0], we append 000 to a[2:0] and then add a i.e, y[5:0] = {a[2:0],000} + a[2:0] (y=9*a can be considered 8*a+1*a ). This can be realized just by wiring and expect no hardware.

```
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_8.v
synth -top mult8
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mul8_netlist.v
```

![mul8stat](/images/Day1/mul8stat.png)
![mul8show](/images/Day1/mul8show.png)
![mul8netlist](/images/Day1/mul8netlist.png)

</detail>
