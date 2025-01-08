# VLSI System Design - Hardware Design Program

<details>

 <summary>Day 0 - Tools Installation</summary>

## System Details

- **RAM**: 8GB
- **Storage**: 256GB SSD
- **OS**: macOS

## Yosys

```
git clone https://github.com/YosysHQ/yosys.git
brew install cmake gcc gawk tcl-tk libtool bison flex make
brew install graphviz xdot neovim macvim
cd yosys
git submodule update --init
make
./yosys
```

![yosys](/images/Day0/yosys.png)

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

- Clone the repository - git clone <https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git> <br>
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

### 1. Introduction to timing .libs

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

### 2. Hierarchical vs Flat Synthesis

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

### 3. Various Flop Coding Styles and Optimization

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

![mul2paper](/images/Day1/mul2paper.png)

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

![mul8paper](/images/Day1/mul8paper.png)

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

</details>

<details>
<summary>Day 3 -  Combinational and Sequential Optmizations </summary>

## Overview

Day 3: Understanding about combinational and sequential optmizations
techniques and importance in chip design.

### 1. Introduction to Optimizations

#### Introduction to Optimization

- Sequencing the logic to achieve the most optimized design (Area and Power Saving).
- Techniques used in optimization:
  - **Constant Propagation**: Directly optimized by identifying constant signals within the circuit, known as constant propagation, to simplify the design.
  - **Boolean Logic Optimization**: Techniques such as K-map and Quine McKluskey for simplifying Boolean expressions, reducing the complexity of logic expressions and enhancing efficiency.

#### Combinational Logic Optimization

- **Constant Propagation**: Direct optimization technique that substitutes the values of known constant variable. In addition to removing pointless computations, this can simplify expressions. Consider the following example, if `A` signal is always `0` in a certain context, any expression dependent on that signal can be simplified accordingly.

![ccp](/images/Day1/ccp.png)

- **Boolean Logic Optimization**: Using methods such as Karnaugh maps (K-maps) or the Quine-McCluskey algorithm, boolean expressions can be simiplied further. This helps reduce the number of logic gates implemented in the design, improving area and power consumption.

![cbl](/images/Day1/cbl.png)

#### Sequential Logic Optimization

- Sequential logic optimization concentrates on unnecessary flip-flops and state reduction.
- **Basic Technique**: Sequential constant propagation. If a flip-flop’s output (Q) remains unchanged (when D is tied to 0 in an async reset), the flip-flop can be removed to streamline the circuit.
- **Advance Techniques**:
  - **State Optimization**: Removes unused states to simplify the state machine.
  - **Sequential Logic Cloning**: Reduces interconnect delays by duplicating a flip-flop whose output feeds multiple distant flops, placing each clone close to its destination. (Floor Plan Aware Synthesis)
  - **Retiming**: Adjusts the placement of combinational logic between flip-flops, redistributing it to optimize the slack time and increase the maximum operating frequency. Improve the performance of the circuit.

#### Sequential constant propagation

- Consider an example of DFF with asynchronous reset where D input is grounded, the logic ie reduced to Y=1. Whereas for a DFF with the asynchronous set because while Q=1 when Set=1, but Q=0 at Set=0 at the next CLK pulse. Q is dependent not only on Set but also on the clock edge, so this technique canot be applied.

![scp](/images/Day1/scp.png)

#### Other Techniques not covered in labs

![oso](/images/Day1/oso.png)

### 2. Combinational Logic Optimizations

#### Example 1 - opt_check.v

```
module opt_check (input a , input b , output y);
 assign y = a?b:0;
endmodule
```

`y=a?b:0` can be written as `y=(~a*0) + (ab)` which can futher be reduced to `y=ab`, that is an AND gate.

- **Command to optimize**: `opt_clean -purge`

##### Lab steps for opt_check.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_ckeck
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check_netlist.v
```

![opt_clean_command](/images/Day1/opt_clean_command.png)
![optcheckshow](/images/Day1/optcheckshow.png)

#### Example 2 - opt_check2.v

```
module opt_check2 (input a , input b , output y);
 assign y = a?1:b;
endmodule
```

`y=a?1:b` can be written as `y=(~a*b) + (a*1)` which can futher be reduced to `y=a+b` using **Absorption Law**, that is an OR gate.

- **Command to optimize**: `opt_clean -purge`

##### Lab steps for opt_check2.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_ckeck2
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check2_netlist.v
```

![optcheck2show](/images/Day1/optcheck2show.png)

#### Example 3 - opt_check3.v

```
module opt_check3 (input a , input b, input c , output y);
 assign y = a?(c?b:0):0;
endmodule
```

`y=a?(c?b:0):0` can be written as `y=(~a*0) + (a*(~c*0+cb))` which can futher be reduced to `y=abc`, that is an 3 input AND gate.

- **Command to optimize**: `opt_clean -purge`

##### Lab steps for opt_check3.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v
synth -top opt_ckeck3
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check3_netlist.v
```

![optcheck3show](/images/Day1/optcheck3show.png)

#### Example 4 - opt_check4.v

```
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
endmodule
```

`y=a?(b?(a & c ):c):(!c)` can be written as `y=(~a*~c) + a*((~b*c)+b(ac))` which can futher be reduced to `y=(~a*~c)+abc+(a*~b*c)` => `y=(~a*~c)+c(a*~b+ab)` which comes into a `y=(~a*~c)+(a*c)`, that is an XNOR gate.

- **Command to optimize**: `opt_clean -purge`

##### Lab steps for opt_check4.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check4.v
synth -top opt_ckeck4
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check4_netlist.v
```

![optcheck4show](/images/Day1/optcheck4show.png)

#### Task 1 - multiple_module_opt.v

```
module sub_module1(input a , input b , output y);
  assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
  assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
  wire n1,n2,n3;
  sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
  sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
  sub_module2 U3 (.a(b), .b(d) , .y(n3));
  assign y = c | (b & n1); 
endmodule
```

- **Command to optimize**: `opt_clean -purge`

##### Lab steps for multiple_module_opt.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
opt_clean -purge
show
write_verilog -noattr multiple_module_opt_netlist.v
```

Before optimization flatten: <br>
![opttaskshow](/images/Day1/opttaskshow.png)

After optimization flatten: <br>
![opttaskflatten](/images/Day1/opttaskflatten.png) <br>
![opttaskshow2](/images/Day1/opttaskshow2.png)

### 3. Sequential Logic Optimizations

#### Example 1 - dff_const1.v

```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
 if(reset)
  q <= 1'b0;
 else
  q <= 1'b1;
end
endmodule
```

- For dff_const1.v, q=0 as long as reset=1. However, when reset=0 q does not immediately becomes 1 rather at the next rising edge of the clk as shown below. So the optimization cannot be applied.

**Simulation Waveform**:
![dffc1sim](/images/Day1/dffc1sim.png)

- **Command to optimize**: `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

##### Lab steps for dff_const1.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const1.v
```

![dffc1stat](/images/Day1/dffc1stat.png)
![dffc1show](/images/Day1/dffc1show.png)

#### Example 2 - dff_const2.v

```
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
 if(reset)
  q <= 1'b1;
 else
  q <= 1'b1;
end
endmodule
```

- For dff_const2.v, q remains 1 even if reset is applied or not and value does not have to wait for clock posedge. So optimization can be applied.

**Simulation Waveform**:
![dffc2sim](/images/Day1/dffc2sim.png)

- **Command to optimize**: `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

##### Lab steps for dff_const2.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const2.v
```

![dffc2stat](/images/Day1/dffc2stat.png)
![dffc2stat2](/images/Day1/dffc2stat2.png)
![dffc2show](/images/Day1/dffc2show.png)

#### Example 3 - dff_const3.v

```
module dff_const3(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
begin
 if(reset)
 begin
  q <= 1'b1;
  q1 <= 1'b0;
 end
 else
 begin
  q1 <= 1'b1;
  q <= q1;
 end
end
endmodule
```

- For dff_const3.v, there are two flip-flops.  q1=0 as long as reset=1. However, when reset=0 q1 does not immediately becomes 1 rather at the next rising edge of the clk with some propagation delay as shown below. q=1 as long as reset=1, acting as set rather than reset. However, when reset=0 q samples q1 as 0 as there are some propagation delay for q1as shown below. At the next clk edge q samples q1 as 1. So the optimization cannot be applied.

![dffc3circuit](/images/Day1/dffc3circuit.png)

**Simulation Waveform**:
![dffc3sim](/images/Day1/dffc3sim.png)

- **Command to optimize**: `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

##### Lab steps for dff_const3.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const3.v
```

![dffc3stat](/images/Day1/dffc3stat.png)
![dffc3show](/images/Day1/dffc3show.png)

#### Example 4 - dff_const4.v

```
module dff_const4(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
begin
 if(reset)
 begin
  q <= 1'b1;
  q1 <= 1'b1;
 end
 else
 begin
  q1 <= 1'b1;
  q <= q1;
 end
end
endmodule
```

**Simulation Waveform**:
![dffc4sim](/images/Day1/dffc4sim.png)

- **Command to optimize**: `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

##### Lab steps for dff_const4.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const4.v
synth -top dff_const4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const4.v
```

![dffc4stat](/images/Day1/dffc4stat.png)
![dffc4stat2](/images/Day1/dffc4stat2.png)
![dffc4show](/images/Day1/dffc4show.png)

#### Example 5 - dff_const5.v

```
module dff_const5(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
begin
 if(reset)
 begin
  q <= 1'b0;
  q1 <= 1'b0;
 end
 else
 begin
  q1 <= 1'b1;
  q <= q1;
 end
end
endmodule
```

**Simulation Waveform**:
![dffc5sim](/images/Day1/dffc5sim.png)

- **Command to optimize**: `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

##### Lab steps for dff_const5.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const5.v
synth -top dff_const5
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const5.v
```

![dffc5stat](/images/Day1/dffc5stat.png)
![dffc5show](/images/Day1/dffc5show.png)

### 4. Sequential Logic Optimizations For Unused Outputs

#### Example 1 - counter_opt.v

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
 if(reset)
  count <= 3'b000;
 else
  count <= count + 1;
end
endmodule

```

**Expected Waveform**:
![counter1ckt](/images/Day1/counter1ckt.png)

**Simulation Waveform**:
![counter1sim](/images/Day1/counter1sim.png)

- **Command to optimize**: `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

##### Lab steps for counter_opt.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt.v
```

![counter1stat](/images/Day1/counter1stat.png)
![counter1show](/images/Day1/counter1show.png)

#### Example 1 - counter_opt2.v

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);
always @(posedge clk ,posedge reset)
begin
 if(reset)
  count <= 3'b000;
 else
  count <= count + 1;
end
endmodule
```

**Expected Waveform**:
![counter2ckt](/images/Day1/counter2ckt.png)

**Simulation Waveform**:
![counter2sim](/images/Day1/counter2sim.png)

- **Command to optimize**: `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

##### Lab steps for counter_opt2.v synthesis using optimization

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt2.v
synth -top counter_opt2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt2.v
```

![counter2stat](/images/Day1/counter2stat.png)
![counter2show](/images/Day1/counter2show.png)

</details>

<details>
<summary>Day 4 -  GLS, Blocking vs Non-Blocking and Sythesis-Simulation Mismatch </summary>

## Overview

Day 4: Focused on Gate-Level Simulation (GLS), the distinctions between blocking and non-blocking statements, and identifying synthesis-simulation mismatches. These concepts are essential for debugging and enhancing the accuracy of RTL designs throughout the synthesis process.

### 1. GLS concepts and Flow using Iverilog

#### Gate Level Simulation Concepts

- Gate Level refers to the netlist representation of a design after synthesis has been completed.
- RTL simulations occur before synthesis, while Gate Level Simulation (GLS) takes place afterward. In RTL simulation, the Device Under Test (DUT) is the RTL design, while in GLS, it is the netlist generated post-synthesis.
- The RTL code and the synthesized netlist are logically intended to be equivalent, allowing the same testbenches to verify both.
- Although the synthesized netlist is expected to maintain the same logical accuracy as the RTL design, mismatches (known as synthesis-simulation mismatches) can sometimes arise. GLS is therefore necessary to detect and resolve these issues, ensuring logical correctness after synthesis.
- To perform GLS, the Gate-level netlist, testbench, and Gate-level Verilog models are supplied to the simulator.

##### GLS can operate in various delay modes

- **Functional validation (zero gate delay model, similar to RTL simulation)**: If the Gate-level Verilog models lack timing data for different process corners, GLS can verify only the functional correctness of the design. Faster process as there is no timing validation involved.

- **Full timing validation (delay annotation)**: When the Gate-level Verilog models include necessary timing details (gate delays, setup/hold violations in standard delay format), GLS can verify both functional correctness and timing behavior of the design. Slower process due to detailed timing process.

#### Gate Level Simulation Flow using Iverilog

![GLSBD](/images/Day4/GLSBD.png)

- To perform Gate-Level Simulation (GLS) using Icarus Verilog (Iverilog), we need a testbench to evaluate the design. A gate-level netlist synthesized from RTL code, then these files are provided to  Iverilog. Finally the results are verified for timing and functional accuracy. In essence, GLS verifies that the synthesized design operates correctly with real gate delays and satisfies timing constraints.

![GLSBD2](/images/Day4/GLSBD2.png)

#### Synthesis and Simulation Mismatch

Some of the common causes for Synthesis - Simulation mismatch (mismatch between pre-synthesis and post-synthesis simulations):

- **Incomplete sensitivity list**: An incomplete sensitivity list in RTL code (such as for combinational logic in Verilog `always` blocks) can also cause synthesis-simulation mismatches. The sensitivity list defines the signals that trigger the execution of the `always` block. If any relevant signal is omitted from this list, it can lead to simulation mismatches due to incorrect behavior in pre-synthesis RTL simulation compared to post-synthesis.To prevent this, it's best to use the `always @(*)` syntax in Verilog, which ensures that all signals involved in combinational logic are automatically included in the sensitivity list.
- **Blocking vs. Non-Blocking Assignments**: The incorrect use of blocking (=) and non-blocking (<=) assignments is a common cause of mismatches between RTL simulation and synthesized design behavior.
  - **Blocking Assignments (=)**: In blocking assignments, statements execute sequentially within an always block. The assignment occurs immediately, "blocking" the next line of code until the current one completes. Using blocking assignments in sequential logic (like inside a clocked always block for flip-flops) can lead to unintended dependencies. The sequential update behavior in a synthesized design may differ from the RTL simulation if the execution order changes unexpectedly.
  - **Non-blocking Assignments (<=)**: Non-blocking assignments update the value of a variable at the end of the simulation time step, allowing all right-hand side expressions in the always block to evaluate before any left-hand side variable is updated. If non-blocking assignments are misused in combinational logic, it can lead to inconsistencies in simulation, as values may not update immediately within the always block.
  - Common Issues from Misuse: Race conditions, incorrect timing, unexpected results in pipelining.
  - To avoid mismatches: Use blocking assignments (=) for combinational logic. Use non-blocking assignments (<=) for sequential (clocked) logic.
- **Non-standard verilog coding**: Using non standard verilog code can cause mismatch between simulation and synthesis.
  - Misuse of blocking statements in conditional structures (like if, case, for, generate) can lead to mismatch.
  - Missing cases in if conditions may cause unintended latches, complicating the synthesis outcome.
  - Missing or overlapping conditions in case statements can similarly result in unintended latches or misinterpretations during synthesis.

#### Missing sensitivity list

![MSL](/images/Day4/MSL.png)

- In the example, the `always` block only triggers when `sel` changes. As a result, the output `y` does not update if only `i0` or `i1` change while `sel` remains constant, effectively causing it to behave like a latch. The corrected design on the right shows the proper coding for a mux, where the `always(*)` block responds to any changes in the relevant signals.

#### Blocking Statements Leading to Synthesis Simulation Mismatch

![BANBA](/images/Day4/BANBA.png)

- The example above, on the left side uses non-blocking assignments, which is a recommended best practice. Conversely, the code on the right side directly assigns d to q, which can cause significant issues as it will create only one latch as q0 is already assigned with d before assigning it to q.

#### Caveats with Blocking Statements

![BANBA2](/images/Day4/BANBA2.png)

- In the above example, y in the left-side code gets the previous value of q0, creating a delay effect similar to a flop. However, in synthesis, no actual flop will be generated. If the order is reversed (as shown in the right-side code), y would instead receive the latest q0 value.

Although both versions yield the same circuit upon synthesis, their simulation behaviors differ. In the left-side code, y takes the old q0 value, whereas the right-side code provides y with the updated q0 value, which may lead to synthesis-simulation mismatches. Using non-blocking assignments helps prevent these discrepancies.

### 2. Labs on GLS and Synthesis Simulation Mismatch

#### Lab GLS Synth Sim Mismatch

- The Gate level verilog model(s) need to be provided as shown below to do GLS using iverilog:

```
Syntax:
    iverilog <path-to-gate-level-verilog-model(s)> <netlist_file.v> <tb_top.v>

Example using ternary_operator_mux_netlist.v:
    iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_netlist.v tb_ternary_operator_mux.v
```

#### Example 1: ternary_operator_mux.v

```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
 assign y = sel?i1:i0;
endmodule
```

##### Simulation Output

![e1sim](/images/Day4/e1sim.png)

##### Lab steps for ternary_operator_mux.v synthesis and GLS generation

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr ternary_operator_mux_netlist.v
!gvim ternary_operator_mux_netlist.v
```

##### Netlist Post Synthesis

```
module ternary_operator_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```

![e1stat](/images/Day4/e1stat.png)
![e1show](/images/Day4/e1show.png)

#### Simulation Post GLS

Command used for GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_netlist.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![e1simgls](/images/Day4/e1simgls.png)

- Clearly pre-synthesis and post-synthesis design simulation match.

#### Example 2: bad_mux.v

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
 if(sel)
  y <= i1;
 else 
  y <= i0;
end
endmodule
```

##### Simulation Output

![e1sim](/images/Day4/e2sim.png)

##### Lab steps for bad_mux.v synthesis and GLS generation

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr bad_mux_netlist.v
!gvim bad_mux_netlist.v
```

##### Netlist Post Synthesis

```
module bad_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```

![e2stat](/images/Day4/e2stat.png)
![e2show](/images/Day4/e2show.png)

#### Simulation Post GLS

Command used for GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_netlist.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![e2simgls](/images/Day4/e2simgls.png)

- In this scenario, there’s a noticeable discrepancy between the pre-synthesis and post-synthesis simulation results. The pre-synthesis simulation behaves similarly to a positive-edge triggered D flip-flop, where the "sel" signal acts as the clock and "i1" as the data input. However, the synthesized design yields a 2-input multiplexer, not a flip-flop.

- Additionally, Yosys generates a warning about potential missing signals in the sensitivity list, assuming the circuit is purely combinational, which could contribute to the mismatch.

![yosysmissmatcherror](/images/Day4/e2missmatcherror.png)

### 3. Labs on Synthesis Simulation Mismatch Blocking Statement

#### Lab Synth Sim Mismatch Blocking Statement

Example - blocking_caveat.v

```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
 d = x & c;
 x = a | b;
end
endmodule
```

##### Simulation Output

- RTL Simulation shows, the d takes the old value of x causing incorrect functionality and the blocking assignments make it seem as if there is a flop in the design.

![bcsim](/images/Day4/bcsim.png)

##### Lab steps for blocking_caveat.v synthesis and GLS generation

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr blocking_caveat_netlist.v
!gvim blocking_caveat_netlist.v
```

##### Netlist Post Synthesis

```
module blocking_caveat(a, b, c, d);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  output d;
  wire d;
  sky130_fd_sc_hd__o21a_1 _5_ (
    .A1(_2_),
    .A2(_1_),
    .B1(_3_),
    .X(_4_)
  );
  assign _2_ = b;
  assign _1_ = a;
  assign _3_ = c;
  assign d = _4_;
endmodule
```

![bcstat](/images/Day4/bcstat.png)
![bcshow](/images/Day4/bcshow.png)

#### Simulation Post GLS

Command used for GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_netlist.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![bcsimgls](/images/Day4/bcsimgls.png)

- Here, the mismatch between pre- and post-synthesis simulations arises due to the use of blocking assignments. For a combinational logic where the output is meant to be `d = (a + b) * c`:
- In the RTL simulation, blocking assignments create the appearance of a flip-flop in the design.
- In the gate-level simulation, however, synthesis optimizes the design to an `O2A1` gate that implements `d = (a + b) * c` without any flip-flops, leading to a discrepancy between the simulations.
- **Learning Note** - Always be careful in design phase with the use of blocking statements.

</details>

<details>
<summary>VSDBabySoC - Introduction and Pre Synthesis</summary>

## Overview

The VSDBabySoC is a basic System-on-Chip (SoC) design that includes a RISC-V processor (rvmyth), a Phase-Locked Loop (PLL) module (pll), and a Digital-to-Analog Converter (DAC) module (dac). This project showcases the integration of these IP cores and focuses on simulating and verifying the design's behavior through both pre-synthesis and post-synthesis simulations.

### System-on-chip (SoC)

- A System on Chip (SoC) is an integrated circuit that consolidates all or most of the components required for a complete electronic system into a single chip. It combines a microprocessor or microcontroller with various other essential components like memory, input/output (I/O) interfaces, analog components, digital logic, and sometimes even specialized processors (e.g., graphics processors or machine learning accelerators).

- SoCs are widely used in devices where space, power efficiency, and performance are key concerns, such as smartphones, tablets, wearables, and embedded systems. The integration of multiple functionalities into a single chip reduces power consumption, lowers costs, and enhances processing speed by reducing the distance data must travel between components.

### VSDBabySoC

- VSDBabySoC is a RISCV-based SoC consisting of one RVMYTH microprocessor, an 8x-PLL to generate a stable clock, and a 10-bit DAC to communicate with other analog devices.
  - **RISC-V Processor (rvmyth)**: RISC-V is an open standard instruction set architecture based on established reduced instruction set computer(RISC) principles. It was first started by Prof. Krste Asanović and graduate students Yunsup Lee and Andrew Waterman in May 2010 as part of the Parallel Computing Laboratory, at UC Berkeley. Unlike most other ISA designs, the RISC-V ISA is provided under open source licenses that do not require fees to use, which provides it a huge edge over other commercially available ISAs. RVMYTH core is a simple RISCV-based CPU, introduced in a workshop by RedwoodEDA and VSD.
  - **PPL Module (pll)**: A Phase-Locked Loop (PLL) is a control system designed to produce an output signal with a phase that aligns with an input signal's phase. PLLs are frequently applied in synchronization tasks, such as clock generation and distribution.
  - **DAC Module (dac)**: A Digital-to-Analog Converter (DAC) is a system that translates digital signals into analog form, commonly used in communication systems to create digitally-defined transmission signals. High-speed DACs are critical for mobile communications, while ultra-high-speed DACs are integral in optical communication systems.

![BabySoC](/images/BabySoC/VSDBabySoC.png)

### Project Structure

The project is structured as follows:

- **src/include/**: Contains header files (.vh) that define essential macros and parameters for the SoC setup.
- **src/module/**: Holds the Verilog files defining the behavior and logic of each SoC module, including the RISC-V processor, PLL, DAC, and any other supporting modules.
- **output/**: Serves as the directory for storing all files generated through the compilation and simulation processes, including compiled binaries, waveform files, and simulation results.

### Requirements

The project is designed to run in a Unix-like environment (Linux or macOS), open source tool for compiling and simulating Verilog code (Icarus Verilog) and a waveform viewer for examining simulation outputs (GTKWave)

### Simulations

The project involves two key stages of simulation:

- **Pre-Synthesis**: Stage where you simulate and verify the design using its Register Transfer Level (RTL) verilog code. Simulations at this stage allow designers to verify the functional correctness of the RTL code without worrying about timing delays or physical constraints.
- **Post-Synthesis**: Stage where the RTL code has been synthesized, thats is it’s been transformed into a gate-level netlist with specific logic gates mapped to a target technology (standard cell library). Post-synthesis simulations ensure that the synthesized design meets both functional and timing requirements. It helps verify that the design hasn’t lost logical integrity during synthesis (catching synthesis-simulation mismatches).
- In short, pre-synthesis focuses on verifying functional correctness in a high-level RTL design and post-synthesis ensures the synthesized gate-level design meets both functional and timing requirements.

### Project Step-by-Step Guide

#### 1. Setup and Prepare Project Directory

- Clone the VSDBabySoC Repo and setup the directory structure as follows:"

[Github - VSDBabySoC](https://github.com/manili/VSDBabySoC.git)

```
VSDBabySoC/
├── src/
│   ├── include/
│   │   ├── sandpiper.vh
│   ├── module/
│   │   ├── vsdbabysoc.v      # Top-level module
│   │   ├── rvmyth.v          # RISC-V core module
│   │   ├── avsdpll.v         # PLL module
│   │   ├── avsddac.v         # DAC module
│   │   └── testbench.v       # Testbench for simulation
└── output/
```

#### 2. Important Files Description

- **vsdbabysoc.v (Top-Level SoC Module)** - This is the top-level module that integrates the rvmyth, pll, and dac modules. Consisting of 6 inputs and 1 output. Inputs include, core processor reset (reset), pll control signals (VCO_IN, ENb_CP, ENb_VCO, REF) and DAC reference voltage (VREFH). Outputs include analog output from DAC (OUT). [Reference - Github](https://github.com/manili/VSDBabySoC.git)

- **rvmyth.v (RISC-V Core)**: The rvmyth module is a simple RISC-V based processor. It takes input clock singal generated by the PLL (CLK) and (reset). It outputs a 10-bit digital signal (OUT) to the DAAC. [Reference - Github](https://github.com/kunalg123/rvmyth/)

- **avsdpll.v (PLL Module)**: The pll module is a phase-locked loop that generates a stable clock (CLK) for the RISC-V core. Control and reference signals are the inputs for the PLL (VCO_IN, ENb_CP, ENb_VCO, REF) and a stable clock (CLK) output is generated for core and synchronizing other modules. [Reference 1 - Github](https://github.com/ireneann713/PLL.git) and [Reference 2 - Github](https://github.com/lakshmi-sathi/avsdpll_1v8.git)

- **avsddac.v (DAC Module)**: The dac module converts the 10-bit digital signal from the rvmyth core to an analog output. Takes 2 inputs, D and VREFH, and gives out one output signal (Analog signal). [Reference - Github](https://github.com/vsdip/rvmyth_avsddac_interface.git)

- **testbench.v**: The testbench file serves as a test module for validating the functionality of `vsdbabysoc`. It handles signal initialization, generates the clock signal, and sets up waveform dumping for simulations conducted both before and after synthesis. The output generates files such as `pre_synth_sim.vcd` or `post_synth_sim.vcd`, depending on the simulation conditions.

#### 3. What is Modeling and understanding modeling of VSDBabySOC

**Understanding Modeling**  
Modeling involves creating a physical or logical representation of a system to generate data, guide decision-making, or predict system behavior. These models are essential for defining, analyzing, and communicating. Modeling and simulation (M&S) play a significant role in the VLSI domain, as they enable tasks like analysis, design specification, verification, and validation of systems. By using models, engineers can gain insights into system functionality and performance.

**Modeling the VSDBabySoC**  
In the case of the VSDBabySoC, modeling and simulation involve defining the behavior of its components. Initial input signals trigger the `vsdbabysoc` module, starting the PLL to generate the correct clock signal for the circuit. The clock drives the `rvmyth` processor to execute instructions, resulting in output values. These values are passed to the DAC core, which generates the final output signal, `OUT`. The design includes three primary elements (IP cores) encapsulated in a wrapper to form the SoC, along with a testbench module for verification. Through this modeling process, the system’s functionality and interaction between components are accurately represented and analyzed. Modeling 3 Main IP Cores

- RVMYTH modeling
- PLL modeling
- DAC modeling

#### 4. Simulation Steps

**Pre-synthesis Simulation Steps**:

```
$ pip3 install pyyaml click sandpiper-saas
$ git clone https://github.com/manili/VSDBabySoC.git
$ cd VSDBabySoC
$ sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
$ mkdir output
$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM -I src/include -I src/module src/module/testbench.v
$ cd output
$ ./pre_synth_sim.out
$ gtkwave pre_synth_sim.vcd
```

![Command1](/images/BabySoC/command1.png)
![Command2](/images/BabySoC/command2.png)
![Waveform](/images/BabySoC/presynthsimwave.png)

**Note**:

- **-DPRE_SYNTH_SIM**: Defines the PRE_SYNTH_SIM macro for conditional compilation in the testbench.
- make file have the detailed steps/commands to preform different synthesis. Also can try,

```
$ cd VSDBabySoC
$ make pre_synth_sim
$ gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

</details>

<details>
<summary>VSDBabySoC - Post Synthesis</summary>

## Overview

Post-Synthesis: Stage where the RTL code has been synthesized, thats is it’s been transformed into a gate-level netlist with specific logic gates mapped to a target technology (standard cell library). Post-synthesis simulations ensure that the synthesized design meets both functional and timing requirements. It helps verify that the design hasn’t lost logical integrity during synthesis (catching synthesis-simulation mismatches).

## Steps in synthesis
1. Run Yosys and Read required verilog files

```
./yosys
read_verilog ../VSDBabySoC/src/module/vsdbabysoc.v 
read_verilog -I ../VSDBabySoC/src/include ../VSDBabySoC/src/module/rvmyth.v 
read_verilog -I ../VSDBabySoC/src/include/ ../VSDBabySoC/src/module/clk_gate.v
```

![ps_read1](/images/BabySoC/ps_read1.png) <br>
![ps_read2](/images/BabySoC/ps_read2.png) <br>
![ps_read3](/images/BabySoC/ps_read3.png)

2. Load lib file required for synthesis

```
read_liberty -lib ../VSDBabySoC/src/lib/avsdpll.lib 
read_liberty -lib ../VSDBabySoC/src/lib/avsddac.lib 
read_liberty -lib ../VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

![ps_read4](/images/BabySoC/ps_read4.png)


3. Run top level synthesis

```
synth -top vsdbabysoc 
```

![ps_topsynth1](/images/BabySoC/ps_topsynth1.png) <br>
![ps_topsynth2](/images/BabySoC/ps_topsynth2.png) <br>
![ps_topsynth3](/images/BabySoC/ps_topsynth3.png) <br>
![ps_topsynth4](/images/BabySoC/ps_topsynth4.png)

4. Map D Flip-Flops to Standard Cells

```
dfflibmap -liberty ../VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

![ps_dffmap](/images/BabySoC/ps_dffmap.png)

5. Perform Optimization and Technology Mapping

```
opt
abc -liberty ../VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}
```

![ps_opt](/images/BabySoC/ps_opt.png) <br>
![ps_abcmap](/images/BabySoC/ps_abcmap.png)

6. Clean up

```
flatten
setundef -zero
clean -purge
rename -enumerate
```

![ps_clean](/images/BabySoC/ps_cleanup.png)

7. Check Statistics and Write the Synthesized Netlist

```
stat
write_verilog -noattr ../VSDBabySoC/output/synth/vsdbabysoc.synth.v
```

![ps_writeverilog](/images/BabySoC/ps_writeverilog.png)

## Steps in post-synthesis simulation

Simulation Steps -

```
$ iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -I src/include -I src/module -I src/gls_modles src/module/testbench.v
$ cd output/post_synth_sim
$ ./post_synth_sim.out
$ gtkwave post_synth_sim.vcd
```

![ps_stat1](/images/BabySoC/ps_stat.png) <br>
![ps_stat2](/images/BabySoC/ps_stat2.png) <br>
![ps_gtkwave](/images/BabySoC/ps_gtkwave.png)

- It is observed that pre-synthesis and post-synthesis simulation graphs matched using same testbench.

</details>

<details>
<summary>VSDBabySoC - Static timing analysis using OpenSTA</summary>

## Overview

OpenSTA is a gate level static timing verifier. As a stand-alone executable it can be used to verify the timing of a design using standard file formats. For more info about the OpenSTA see [here](https://github.com/The-OpenROAD-Project/OpenSTA).

## OpenSTA Installation - Using Docker

![sta](/images/BabySoC/sta.png)

## Static timing analysis using OpenSTA

### Example1 - Timing Analysis using in line commands

```
# delay calc example
read_liberty /OpenSTA/examples/nangate45_slow.lib.gz
read_verilog /OpenSTA/examples/example1.v
link_design top
create_clock -name clk -period 10 {clk1 clk2 clk3}
set_input_delay -clock clk 0 {in1 in2}
report_checks
```

![example1](/images/BabySoC/staexample1.png)

### Example2 - Timing Analysis using TCL file (Reading tcl_file as command line argument)

```
docker run -i -v $HOME:/data opensta /OpenSTA/examples/min_max_delays.tcl
```

![example2](/images/BabySoC/staexample2.png)

### Example3 - VSDBabySoC basic timing analysis

```
read_liberty -min /OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -max /OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -min /OpenSTA/examples/timing_libs/avsdpll.lib
read_liberty -max /OpenSTA/examples/timing_libs/avsdpll.lib
read_liberty -min /OpenSTA/examples/timing_libs/avsddac.lib
read_liberty -max /OpenSTA/examples/timing_libs/avsddac.lib
read_verilog /OpenSTA/examples/BabySoC/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc /OpenSTA/examples/BabySoC/vsdbabysoc_synthesis.sdc
report_checks
```

```
docker run -i -v $HOME:/data opensta /OpenSTA/examples/BabySoC/sta_timing_checks.tcl
```

![example3](/images/BabySoC/staexample3.png)

### Example4 - VSDBabySoC PVT Corner Analysis (Post-Synthesis Timing)

The STA checks are performed across all the corners to confirm the design meets the target timing requirements.

- The worst max path (Setup-critical) corners in the sub-40nm process nodes are usually: ss_LowTemp_LowVolt, ss_HighTemp_LowVolt (Slowest corners)
- The worst min path (Hold-critical) corners being: ff_LowTemp_HighVolt,ff_HighTemp_HighVolt (Fastest corners)
- The below tcl script can be run to performt the STA across the PVT corners for which the sky130 lib files are available.
- Download the timing libraries from https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master/timing

```
set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

read_liberty /OpenSTA/examples/timing_libs/avsdpll.lib
read_liberty /OpenSTA/examples/timing_libs/avsddac.lib

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty /OpenSTA/examples/timing_libs/$list_of_lib_files($i)
read_verilog /OpenSTA/examples/BabySoC/vsdbabysoc.synth.v
link_design vsdbabysoc
current_design
read_sdc /OpenSTA/examples/BabySoC/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /OpenSTA/examples/BabySoC/STA_OUPUT/min_max_$list_of_lib_files($i).txt

exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_worst_max_slack.txt
report_worst_slack -max -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_worst_max_slack.txt

exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_worst_min_slack.txt
report_worst_slack -min -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_worst_min_slack.txt

exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_tns.txt
report_tns -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_tns.txt

exec echo "$list_of_lib_files($i)" >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_wns.txt
report_wns -digits {4} >> /OpenSTA/examples/BabySoC/STA_OUPUT/sta_wns.txt
}
```

![babysocpvt1](/images/BabySoC/PVT_table.png)
![babysocpvt2](/images/BabySoC/WorstSetupSlack.png)
![babysocpvt3](/images/BabySoC/WorstHoldSlack.png)
![babysocpvt4](/images/BabySoC/WNS.png)
![babysocpvt5](/images/BabySoC/TNS.png)

</details>

<details>
<summary> Day 1 - Basics of NMOS Drain Current (Id) vs Drain-to-source Voltage (Vds)</summary>

## Overview

Understanding the concept of circuit design and SPICE simulations. Why do we need SPICE simulation. Basic element in Circuit Design - NMOS Transistor, their region of operations. Building SPICE setup for basic NMOS.

<details>

<summary>Theory</summary>

### 1. Introduction to Circuit Design and SPICE simulations

#### Why do we need SPICE simulations?

**Circuit Design**: The interconnection of the logic gates, like NAND, NOR, NOT, which are made of NMOS and PMOS, to get required functionality is Circuit design. Example, an inverter, pull up PMOS, pull down NMOS, drains are connected to OUT with capaciot load, gates are connected to INPUT, Source of PMOS conncted to Supply and Source of NMOS connected to GND.

![CircuitDesign](/images/CMOS/CircuitDesign.png)

**SPICE Simulation**: SPICE simulations provide a platform to test and validate that design in a virtual environment. Consider same circuit when provided with a input waveform using SPICE simulator, output characteristics waveform are generated. These waveform can tell you about the delay of the cell, and by changing the value of design parameter (width,length of transistor), delay parameter can be determined. (W/L determines current flow which determines delay)

![SPICESimulation](/images/CMOS/SPICESIMULATION.png)

**Why do we need SPICE**: Consider the following example

![Example1](/images/CMOS/Example1.png)

- CBUF 1 and CBUF 2 difference comes from circuit design (might have different drive strengths, different W/L).
- Source of delay table values is from SPICE simulations. Same SPICE simulator is used to verify the delays on the circuit design.

![WhySpice](/images/CMOS/WhySpice.png)

#### Introduction to basic element in Circuit Design - NMOS Transistor

- N type Metal Oxide Semiconductor transistor

![NMOS](/images/CMOS/NMOS.png)

#### Strong Inversion, Threshold Voltage and Threshold Voltage with Substrate Potential

- Threshold Voltage: Important topic (models) which drive all the spice simulations

![TH1](/images/CMOS/Threshold1.png)
![TH2](/images/CMOS/Threshold2.png)

- Consider potential to body terminal

![BodyNeg1](/images/CMOS/BodyTerminalNeg.png)
![BodyNeg2](/images/CMOS/BodyTerminalNeg2.png)
![BodyNeg3](/images/CMOS/BodyTerminalNeg3.png)

- Thershold voltage at Vsb = 0, Body effect coefficient and Fermi Potential are constants provided by foundry which will form the model and feed to SPICE simulation for various calculations

![BodyNeg4](/images/CMOS/BodyTerminalNeg4.png)

### 2. NMOS Resistive and Saturation Regions of Operation

#### Resistive region of operation with small drain-source voltage

![RR1](/images/CMOS/vgs1v.png)
![RR2](/images/CMOS/vgs1v5.png)
![RR3](/images/CMOS/vgs2v.png)
![RR4](/images/CMOS/ResistiveRegion1.png)

#### Drift current theory

- Cox is constant value comes from foundry
- Drift Current: Current due to potential difference
- Diffusion Current: Current due to difference in carrier concentration

![DC1](/images/CMOS/DC1.png)
![DC2](/images/CMOS/DC2.png)

#### Drain current model for linear region of operation

![DC3](/images/CMOS/DC3.png)
![DC4](/images/CMOS/DC4.png)
![DC5](/images/CMOS/DC5.png)

#### SPICE conclusion to resistive operation

- How do we calculate the Id for different values of Vgs and at every value of Vgs, sweep Vds till (Vgs-Vt)?
- SPICE simulation can help us doing these calculations and generate graphs.

#### Saturation Region/Pinch-off region condition

![SR1](/images/CMOS/SR1.png)
![SR2](/images/CMOS/SR2.png)
![SR3](/images/CMOS/SR3.png)

#### Drain current model for saturation region of operation

- In Saturation Region, channel voltage is no longer dependent on Vds, the channel voltage is constant at Vgs-Vt.

![SR4](/images/CMOS/SR4.png)
![LinearFinal](/images/CMOS/LinearFinal.png)
![SR6](/images/CMOS/SR6.png)
![SR5](/images/CMOS/SR5.png)

### 3. Introduction to SPICE

#### Basic SPICE setup

Step 1: Proper SPICE setup by providing spice model parameters
Step 2: Check whether the parameters are coming from correct set of technolgy node. First set of inputs to SPICE.
Step 3: Give SPICE Netlist
Step 4: Define Technology file (Contains all the foundry constant values)
Step 5: Provide simulation commands

![SP1](/images/CMOS/Spice1.png)
![SP2](/images/CMOS/Spice2.png)
![SP3](/images/CMOS/Spice3.png)

#### Circuit description in SPICE syntax

- SPICE Netlist: Define components and nodes. Nodes, in,n1,vdd,0 in below example.
- M1 vdd n1 0 0 nmos W=1.8u L=1.2u represents MXX=MOSFETXX, drain, gate, source, substrate, MOSFET name in technology file, Width of gate and Length of gate respectively
- R1 in n1 55 represents RXX=ResistorXX, first terminal, second terminal and resistance value respectively
- Vdd vdd 0 2.5 represents VXX=Voltage Source XX, positive terminal, negative terminal and voltage source value respectively.
- Vin in 0 2.5 represents VXX=Voltage Source XX, positive terminal, negative terminal and voltage source value respectively.

```
*** NETLIST Description ***
M1 vdd n1 0 0 nmos W=1.8u L=1.2u 
R1 in n1 55 
Vdd vdd 0 2.5 
Vin in 0 2.5
*** .include xxxx_1um_model.mod ***
.LIB ""xxxx_025um_model.mod" CMOS_MODELS
```

![Netlist1](/images/CMOS/Netlist1.png)
![Netlist2](/images/CMOS/Netlist2.png)

#### Define Technology Parameters/File

- Model to get NMOS

![Netlist3](/images/CMOS/Netlist3.png)
![Netlist4](/images/CMOS/Netlist4.png)

</details>

<details>

<summary>Labs</summary>

### 1. Introduction to SPICE

#### SPICE Lab with sky130 models

- CLone the following github repo: https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
- 3 Important files:
  - /sky130CircuitDesignWorkshop/design/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.pm3.spice:
  - /sky130CircuitDesignWorkshop/design/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.corner.spice:
  - /sky130CircuitDesignWorkshop/design/sky130_fd_pr/models/sky130.lib.pm3.spice:
- Installation Guide for ngspice - https://www.seas.upenn.edu/~ese3700/spring2024/handouts/Spice_Install_MAC.pdf

**Example**

```

*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control

run
display
setplot dc1
.endc
.end
```

```
ngspice day1_nfet_idvds_L2_W5.spice
plot -vdd#branch
```

- The plot of Ids vs Vds over constant Vgs:

![spiceresult](/images/CMOS/spiceexample.png)

</details>
</details>

<details>
<summary> Day 2 - Velocity saturation and basics of CMOS inverter VTC</summary>

## Overview

Concepts covered are short channel behavior of MOSFET, Velocity saturation, MOSFET as switch and CMOS voltage transfer charcteristics from PMOS and NMOS load curves.

<details>
<summary>Theory</summary>

### 1. SPICE simulation for lower nodes and velocity saturation effect

#### SPICE simulation for lower nodes

![LowerNode1](/images/CMOS/LowerNode1.png)
![LowerNode2](/images/CMOS/LowerNode2.png)
![LowerNode3](/images/CMOS/LowerNode3.png)

#### Drain current vs gate voltage for long and short channel device

- Quadratic Dependence on gate voltage in long channel.
- Quadratic Dependence at lower gate voltage and linear dependence at higher voltage in short channel due to velocity saturation.

![LowerNode4](/images/CMOS/LowerNode4.png)

#### Velocity saturation at lower and higher electric fields

- Lower values of Electric fields, velocity tends to linear function of Electric field and reaches a point where the velocity becomes constant.

![VS1](/images/CMOS/VS1.png)
![VS2](/images/CMOS/VS2.png)
![VS3](/images/CMOS/VS3.png)

#### Velocity saturation drain current model

- Technology parameter: Saturation voltage (Vdsat) i.e., voltage which device velocity saturates and is independent of Vgs or Vds.
- Vgt is minimum means FET is in Saturation region (Both for long and short channel)
- Vds is minimum means FET is in Linear region (Both for long and short channel)
- Vdsat is minimum means FET device velocity saturates (Only for short channel)

![VS4](/images/CMOS/VS4.png)
![VS5](/images/CMOS/VS5.png)

**Observation 1**: For short channel, at higher electric fields device eneter velocity saturation and Ids remains constant as Vds increases and Id is linear function of Vgs when compared to quadratic in long channel. <br>
**Observation 2**: Velocity Saturation causes device to saturate early.

Source - 1. http://www.seas.upenn.edu/~jan/spice/spice.MOSparamlist.html / https://www.seas.upenn.edu/~jan/spice/spice.overview.html <br>
Source - 2. http://km2000.us/franklinduan/articles/ecee.colorado.edu/~bart/book/book/chapter7/ch7_5.htm

### 2. CMOS Voltage Transfer Characteristics (VTC)

#### MOSFET as a switch

![cmosvtc1](/images/CMOS/cmosvtc1.png)
![cmosvtc2](/images/CMOS/cmosvtc2.png)
![cmosvtc3](/images/CMOS/cmosvtc3.png)

#### Introduction to standard MOS voltage current parameters

- Rp and Rn are non linear resistor function of drain current.

![cmosvtc4](/images/CMOS/cmosvtc4.png)
![cmosvtc5](/images/CMOS/cmosvtc5.png)

#### PMOS/NMOS drain current v/s drain voltage

![cmosvtc6](/images/CMOS/cmosvtc6.png)

#### Step1: Convert PMOS gate-source-voltage to Vin

- Below are the steps to obtain Voltage-Transfer Charcteristics (VTC) for Static CMOS Inverter
- Because we dont have internal node voltages access in simulations, hence need to convert everything into Vin, Vdd, Vss and Vout.

![cmosvtc7](/images/CMOS/cmosvtc7.png)

#### Step2 & Step3: Convert PMOS and NMOS drain-source-voltage to vout

![cmosvtc8](/images/CMOS/cmosvtc8.png)
![cmosvtc9](/images/CMOS/cmosvtc9.png)

#### Step4: Merge PMOS,NMOS load curves and plot VTC

![cmosvtc10](/images/CMOS/cmosvtc10.png)

</details>
<details>
<summary>Labs</summary>

### 1.SPICE simulation for lower nodes and velocity saturation effect

#### Sky130 Id-Vgs

**Example**

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control

run
display
setplot dc1
.endc
.end
```

- The plot of Ids vs Vds over constant Vgs:

![spiceresult2](/images/CMOS/spiceexample2.png)

**Example**

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.1 

.control

run
display
setplot dc1
.endc
.end
```

- The plot of Ids vs Vgs over constant Vds:

![spiceresult3](/images/CMOS/spiceexample3.png)
</details>
</details>

<details>
<summary> Day 3 - CMOS Switching threshold and dynamic simulations</summary>

## Overview

Understanding of CMOS Switching threshold, static and dynamic simulations. Analytical derivation of Switching threshold as a function of W/L of PMOS and NMOS. 

<details>
<summary>Theory</summary>

### 1. Voltage transfer characteristics: SPICE simulations

#### SPICE deck creation for CMOS inverter

- SPICE Deck contains
  - Component connectivity
  - Components values
  - Identify 'nodes'
  - Name 'nodes'

![spicedeck1](/images/CMOS/spicedeck1.png)

#### SPICE simulation for CMOS inverter

- SPICE Netlist for CMOS Inverter

![spicedeck2](/images/CMOS/spicedeck2.png)

- Same Wn/Ln = Wp/Lp = 1.5. Plot out vs in:

![spicedeck3](/images/CMOS/spicedeck3.png)

- Now, Wn/Ln = 1.5 and Wp/Lp = 3.75. Plot out vs in

![spicedeck4](/images/CMOS/spicedeck4.png)

### 2. Static behavior evaluation: CMOS inverter robustness, Switching Threshold

#### Switching Threshold, Vm

- Switching Threshold (Vm) where Vin = Vout. The point where maximum power is drawn due to large current. Both the transistors are saturation region.
- The switching threshold, Vm, is defined as the point where Vin=Vout.
- Graphically it can be found from the intersection of the VTC with the Vin=Vout line.
- In the region around Vm, both PMOS and NMOS are in saturation, since VDS=VGS.
- An analytical expression for Vm can be obtained by equating the currents through the PMOS and NMOS transistors, IDSn=IDSp.

![switchingthreshold](/images/CMOS/switchingthreshold.png) <br>
![switchingthreshold2](/images/CMOS/switchingthreshold2.png)

#### Analytical expression of Vm as a function of (W/L)p and (W/L)n

- Depending on the supply voltage, VDD and the Channel length, L, of the devices, there can be two cases:
  - Devices are Velocity Saturated
  - Velocity Saturation does not occur

Note: For the following derivations, we ignore the effects of Channel Length Modulation for simplicity.

Case 1: Devices are Velocity-Saturated - VDSAT<(VM−VT)

This case is applicable to short-channel devices or when the supply voltage is high so that the devices are in velocity saturation.

![vmanalysis1](/images/CMOS/vmanalysis1.png)
![vmanalysis2](/images/CMOS/vmanalysis2.png)
![vmanalysis3](/images/CMOS/vmanalysis3.png)
![vmanalysis4](/images/CMOS/vmanalysis4.png)

$I_{DSn} = -I_{DSp}$  
$i.e.,$  
$~~~~ I_{DSn} + I_{DSp} = 0$  

$k_n \left[ (V_M - V_{THn})V_{DSATn} - \dfrac{V_{DSATn}^2}{2} \right] + k_p \left[ (V_M - V_{DD} - V_{THp})V_{DSATp} - \dfrac{V_{DSATp}^2}{2} \right] = 0$

$k_n V_{DSATn} \left[ V_M - V_{THn} - \dfrac{V_{DSATn}}{2} \right] + k_p V_{DSATp} \left[ V_M - V_{DD} - V_{THp} - \dfrac{V_{DSATp}}{2} \right] = 0$

<br>

$Solving for V_M:$  

$\boxed{V_M = \dfrac{\left( V_{THn}+\dfrac{V_{DSATn}}{2} \right) + r \left( V_{DD}+V_{THp}+\dfrac{V_{DSATp}}{2} \right)}{1+r}},$  
  
$where, ~ \boxed{r=\dfrac{k_p V_{DSATp}}{k_n V_{DSATn}} = \dfrac{\upsilon_{satp} W_p}{\upsilon_{satn} W_n}}$ _(assuming for identical oxide thickness for PMOS and NMOS transistors)_  

  - Now, for large values of $V_{DD}$ compared to the threshold voltages $(V_{THp}, V_{THn})$ and saturation voltages $(V_{DSATp}, V_{DSATn})$, the above equation can be approximated to:

$~~~~~~~~~~~~~~~~ \boxed{V_M \approx \dfrac{rV_{DD}}{1+r}}$  

  - The switching threshold is determined by the ratio, $r$ - which is a measure of the relative drive strengths of the PMOS and NMOS transistors.
  - For comparable values for low and high noise margins, $V_M$ is desired to be located around the centre of the available voltage swing (or at $V_{DD}/2$ as CMOS logic has rail-to-rail swing). This implies:  
$~~~~~~~~ r \approx 1$  
$~~~~~~~~ i.e., k_p V_{DSATp} = k_n V_{DSATn}$  
$\boxed{(W/L)\_p = (W/L)\_n * \dfrac{{k_n}^\prime V_{DSATn}}{{k_p}^\prime V_{DSATp}}}$  

  - To move the $V_M$ upwards, a larger value of $r$ is needed, which in other words is to make the PMOS wider.
  - On the other hand, to move the $V_M$ downwards, the NMOS must be made wider.
  - If a target design value for $V_M$ is desired, we can derive the required ratio of PMOS vs. NMOS transistor sizes in a similar manner:

$~~~~~~~~ \boxed{\dfrac{(W/L)\_p}{(W/L)\_n} = \dfrac{k_n^\prime V_{DSATn} \left[ V_M - V_{THn} - \dfrac{V_{DSATn}}{2} \right]}{k_p^\prime V_{DSATp} \left[ V_{DD} - V_M + V_{THp} + \dfrac{V_{DSATp}}{2}\right]}}$  
$~~~~~~~~ Note:$ _Make sure that the assumption that both devices are velocity-saturated still holds for the chosen operation point._


**<ins>Case 2:</ins> Velocity Saturation does not occur - $V_{DSAT}>(V_M-V_{TH})$**  
  - This case is applicable for long-channel devices or when the supply voltages are low, that the devices are not velocity saturated.
  - Using a similar analysis done in Case 1 above, we derive the switching threshold, $V_M$ to be:

$~~~~~~~~ \boxed{V_M = \dfrac{V_{THn} + r(V_{DD} + V_{THp})}{1+r}}, ~~~~ where ~ r = \sqrt{\dfrac{-k_p}{k_n}}$  

#### Static and dynamic simulation of CMOS inverter with increased PMOS width

  - The switching threshold, $V_M$ **is relatively insensitive to variations in the device ratio**.
  - Small variations of the ratio (e.g., 3 or 2.5) do not disturb the transfer characteristic that much.

![switchingthreshold3](/images/CMOS/switchingthreshold3.png)

#### Applications of CMOS inverter in clock network and STA

![appclockinv1](/images/CMOS/appclockinv1.png)
![appclockinv2](/images/CMOS/appclockinv2.png)
![appclockinv3](/images/CMOS/appclockinv3.png)
![appclockinv4](/images/CMOS/appclockinv4.png)
![appclockinv5](/images/CMOS/appclockinv5.png)

</details>
<details>
<summary>Labs</summary>

### 1. Voltage transfer characteristics: SPICE simulations

#### Sky130 SPICE simulation for CMOS - VTC

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15
Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc
.end
```

```
ngspice day3_inv_vtc_Wp084_Wn036.spice
plot out vs in in
```

![vtcsim](/images/CMOS/vtcsim.png)

- The "switching threshold" of a CMOS inverter refers to the voltage level on the input (Vin) where the output voltage (Vout) is exactly equal to the input voltage, essentially the point on the voltage transfer curve where the two lines intersect; this is also often called the midpoint voltage (VM) of the inverter.

#### Sky130 SPICE simulation for CMOS - Transient Analysis

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15
Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands
.tran 1n 10n
.control
run
.endc
.end
```

```
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs time in
```

- PULSE (0 to 1.8V, shift of zero, rise and fall times 0.1ns, pulse width 2ns, total time 4ns)

![transientsim](/images/CMOS/transientsim.png)

- The propagation delay of a logic gate e.g. inverter is the difference in time (calculated at 50% of input-output transition), when output switches, after application of input.

![risefalldelay](/images/CMOS/risefalldelay.png)

- The propagation delay high to low (tpHL) is the delay when output switches from high-to-low, after input switches from low-to-high. The delay is usually calculated at 50% point of input-output switching, as shown in above figure.

</details>
</details>

<details>
<summary> Day 4 - CMOS Noise Margin robustness evaluation</summary>

## Overview

Understanding CMOS noise margin, calculations of noise margin, importance of noise margin. Various of of noise margin with respective to device ratio.

<details>
<summary>Theory</summary>

### 1. Static behavior evaluation: CMOS inverter robustness, Noise margin

#### Introduction to noise margin

- In digital circuits, if the magnitude of the "noise voltage" at a node is too large, logic errors can be introduced into the system.
- However, if the noise amplitude is less than a specified value, called the noise margin, the noise signal will be attenuated as it passes through the logic gate or circuit, and the logic signals will be transmitted without any errors.
- Noise margin is the amount of noise that a CMOS circuit could withstand without compromising the operation of circuit.
- Noise margin makes sure that, any signal which is logic 1 with finite noise added to it, is still recognized as logic 1 and not logic 0. Similarly, any signal which is logic 0 with finite noise added to it, is still recognized as logic 0 and not logic 1.

#### Noise Margin

- Let us consider the case of two CMOS inverters connected back-to-back with one driving the next.
- The following images show an ideal, a piece-wise linear and a realistic VTC of a CMOS inverter:

![noisemargin1](/images/CMOS/noisemargin1.png)
![noisemargin2](/images/CMOS/noisemargin2.png)
![noisemargin3](/images/CMOS/noisemargin3.png)
![noisemargin4](/images/CMOS/noisemargin4.png)
![noisemargin5](/images/CMOS/noisemargin5.png)

- $V_{IL}$ and $V_{IH}$ (or to be more precise, $V_{IL-MAX}$ and $V_{IH-MIN}$) are defined to be the operational points of the inverter where $\dfrac{dV_{out}}{dV_{in}} = -1$. Or, from an analog design perspective, these are the points where the gain of the inverting amplifier formed by the inverter is equal to -1.
  - Any input voltage level between 0 and $V_{IL}$ will be treated as **logic 0**
  - Any input voltage level between $V_{IH}$ and $V_{DD}$ will be treated as **logic 1**
  - The point $V_{IL}$ occurs when the NMOS is biased in saturation region and the PMOS is biased in the linear region.
  - Similarly, the point $V_{IH}$ occurs when the NMOS is biased in linear region and the PMOS is biased in the saturation region.

  ```
  Output Characteristics:

  VOL_Min : Minimum output voltage that the logic gate can drive for a logic "0" output.
  VOL_Max : Maximum output voltage that the logic gate will drive corresponding to a logic "0" output.
  VOH_Min : Minimum output voltage that the logic gate will drive corresponding to a logic "1" output.
  VOH_Max : Maximum output voltage that the logic gate can drive for a logic "1" output.

  Input Characteristics:

  VIL_Min : The minimum input voltage to the gate corresponding to logic "0" -- is equal to the VSS
  VIL_Max : The maximum input voltage to the gate that will be recognized as logic "0"
  VIH_Min : The minimum input voltage to the gate that will be recognized as logic "1"
  VIH_Max : The maximum input voltage to the gate corresponding to logic "1" -- is equal to the VDD
  ```

- Obviosuly, for proper operation of the logic gate in the presence of noise:
  - $V_{OL-MAX} < V_{IL-MAX}$
  - $V_{OH-MIN} > V_{IH-MIN}$

- For $V_{in} \le V_{IL}$ , the inverter gain magnitude is less than unity, and the output change is minimal for a given change in the input voltage in this range.
- Similarly, for $V_{in} \ge V_{IH}$ , the output change is minimal for a given input voltage in this range, again because of the same reason that the gain magnitude is less than unity.
- However, when the input voltage is in the range $V_{IL} < V_{in} < V_{IH}$ , the gain magnitude is greater than one, and the output signal amplitude changes drastically.
  - This region is called the **undefined range** (from a digital design standpoint), since if the input voltage is inadvertently pushed into this range by a noise signal, the output may change logic state introducing an error.

- **The noise margins are defined as thus defined as follows:**  
  - Low-level Noise Margin, $~ NM_L ~ = V_{IL-MAX} - V_{OL-MAX}$  
  - High-level Noise Margin, $NM_H = V_{OH-MIN} - V_{IH-MIN}$  
  - Noise Margin, $NM = Min(NM_L, NM_H)$

![noisemargin6](/images/CMOS/noisemargin6.png)
![noisemargin7](/images/CMOS/noisemargin7.png)
![noisemargin8](/images/CMOS/noisemargin8.png)
![noisemargin9](/images/CMOS/noisemargin9.png)

#### Noise Margin Robustness against variations in Device Ratio

![noisemargin10](/images/CMOS/noisemargin10.png)

</details>
<details>
<summary>Labs</summary>

### 1. Static behavior evaluation: CMOS inverter robustness, Noise margin

#### Noise Margin - sky130 Inverter (Wp/Lp=1u/0.15u, Wn/Ln=0.36u/0.15u)

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15
Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01
.control
run
setplot dc1
display
let dVout = deriv(V(out))
meas dc vil find V(in) when dVout=-1 cross=1
meas dc vih find V(in) when dVout=-1 cross=2
meas dc voh find V(out) when dVout=-1 cross=1
meas dc vol find V(out) when dVout=-1 cross=2

let NML = vil - vol
let NMH = voh - vih
print NML
print NMH
.endc
.end
```

```
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot out vs in in
```

![noisemargin](/images/CMOS/noisemarginsim.png)

</details>
</details>

<details>
<summary> Day 5 - CMOS power supply and device variation robustness evaluation</summary>

## Overview

Evaluating the Robustness of the CMOS Inverter with power supply variations/scaling and device variations.

<details>
<summary>Theory</summary>

### 1. Static behavior evaluation: CMOS inverter robustness, Power supply variation

#### Smart SPICE simulation for power supply variations

- Here, we take a look at the effect of power supply scaling on the static behaviour of the CMOS Inverter.

![supplyvariation1](/images/CMOS/supplyvariation1.png)
![supplyvariation2](/images/CMOS/supplyvariation2.png)

- We can see from the simulations that:
  - The CMOS Inverter continues to work well at even 0.8V (below half the original supply voltage of 1.8V) close to the transistor threshold voltages!
  - Since the transistor ratio, $r$ is fixed, the switching threshold, $V_M$ is approximately proportional to the $V_{DD}$.
  - The gain of the inverter in the transition region increases with a reduction of the supply voltage.
  - The width of the transition region also reduces when the supply voltage is scaled down compared to the original $V_{DD}$.

![supplyvariation3](/images/CMOS/supplyvariation3.png)
![supplyvariation4](/images/CMOS/supplyvariation4.png)

- However, given these improvements in DC characteristics, we cannot choose to operate all our digital circuits at these low supply voltages:
  - Reducing the supply voltage yields a significant reduction in the energy dissipation, but it is absolutely detrimental to the performance of the gate, increasing the transition times greatly.
  - The DC characteristic becomes increasingly sensitive to variations in the device parameters such as the transistor threshold, once supply voltages and intrinsic voltages become comparable.
  - Scaling the supply voltage means reducing the signal swing. While this typically helps to reduce the internal noise in the system (such as caused by crosstalk), it makes the design more sensitive to external noise sources that do not scale.

![supplyvariation5](/images/CMOS/supplyvariation5.png)

### 2. Static behavior evaluation: CMOS inverter robustness, Device variation

- While we design a gate for nominal operation conditions and typical device parameters, the actual operating temperatures might very over a large range, and the device parameters after fabrication will deviate from the nominal values used in the design process.

- The DC characteristics of the static CMOS inverter turn out to be rather insensitive to these variations, and the gate remains functional over a wide range of operating conditions.

#### Sources of variation: Etching process

![etch1](/images/CMOS/etch1.png)<br>
![etch2](/images/CMOS/etch2.png)<br>
![etch3](/images/CMOS/etch3.png)<br>
![etch4](/images/CMOS/etch4.png)

#### Sources of variation: Oxide Thickness

![oxidethickness1](/images/CMOS/oxidethickness1.png) <br>
![oxidethickness2](/images/CMOS/oxidethickness2.png)

</details>
<details>
<summary>Labs</summary>

### 1. Static behavior evaluation: CMOS inverter robustness, Power supply variation

#### Smart SPICE simulation for power supply variations

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15
Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

.control

let powersupply = 1.8
alter Vdd = powersupply
	let voltagesupplyvariation = 0
	dowhile voltagesupplyvariation < 6
	dc Vin 0 1.8 0.01
	let powersupply = powersupply - 0.2
	alter Vdd = powersupply
	let voltagesupplyvariation = voltagesupplyvariation + 1
      end
 
plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "input voltage(V)" ylabel "output voltage(V)" title "Inveter dc characteristics as a function of supply voltage"
plot deriv(dc1.V(out)) deriv(dc2.V(out)) deriv(dc3.V(out)) deriv(dc4.V(out)) deriv(dc5.V(out)) deriv(dc6.V(out)) vs in 
+ xlabel 'Input Voltage(V)' ylabel 'Gain (dVout/dVin)' title 'Inverter Gain for different VDD'
.endc
.end
```

![supplyvariationsim](/images/CMOS/supplyvariationsim.png)

Note: After one point the gain starts decreasing even the supply vvoltage is reduced beacause the supply voltage is not sufficient to turn on both PMOS and NMOS.

### 2. Static behavior evaluation: CMOS inverter robustness, Device variation

- Understand the CMOS Inverter's robustness in the case of a ridiculously extreme case of device width variation.

![devicevariationlab](/images/CMOS/devicevariationlab.png)
![devicevariationlab2](/images/CMOS/devicevariationlab2.png)

```
*** Model Description **
.param temp=27
.param Wp=0.84
.param Wn=0.42

*** Including sky130 library files ***
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*** Netlist Description ***
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w={Wp} l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w={Wn} l=0.15
Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

*** Simulation Commands ***
.dc Vin 0 1.8 0.01

.control
alterparam Wp=0.84
alterparam Wn=0.42
reset
run

alterparam Wp=0.42
alterparam Wn=7
reset
run

alterparam Wp=7
alterparam Wn=0.36
reset
run

plot dc1.V(out) dc2.V(out) dc3.V(out) vs in in
+ xlabel 'Input Voltage(V)' ylabel 'Output Voltage(V)' title 'Inverter VTC for Extreme Device Variations'
.endc
.end
```

![devicevariationsim](/images/CMOS/devicevariationsim.png)

**From the simulation results:**

- The shift in $V_M$ for such a large change in device variations is relatively very small.
- The shift in Vm is towards right (high) if PMOS width is greater than NMOS width and Vm is towards left (low) if NMOS width is greater than PMOS width.
- The variation in the $NM_H$ and $NM_L$ is also very small.

</details>
</details>

<details>
<summary> Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK </summary>

### 1. Introduction

### Introduction to QFN-48 Package, chip, pads, core, die and IPs

**IC terminologies**:

- Chip Package
- Pads & Padring
- Core, die
- IPs, Macros
- Foundry

| ![PDIntro_1](/images/PD/pdintro1.png) | ![PDIntro_2](/images/PD/pdintro2.png) |
|:---|:---|

| ![PDIntro_3](/images/PD/pdintro3.png) | ![PDIntro_4](/images/PD/pdintro4.png) |
|:---|:---|

| ![PDIntro_1](/images/PD/pdintro1.png) | ![PDIntro_2](/images/PD/pdintro2.png) |
|:---|:---|

### Introduction to RISC-V

- **Open-Source ISA**: RISC-V is an open-source instruction set architecture (ISA) widely adopted in computer architecture and processor design.  
- **Origin**: Developed at the University of California, Berkeley, in 2010, it has since become a global collaboration among researchers and industry experts.  
- **Simplicity and Modularity**: RISC-V is built on the Reduced Instruction Set Computer (RISC) philosophy, emphasizing a small, streamlined set of instructions for easy decoding and execution.  
- **Base and Optional Instruction Sets**:
  - Includes a base set, *RV32I*, for general-purpose computing.  
  - Offers optional sets like *RV32F* for single-precision floating-point operations and *RV64G* for 64-bit computing, enabling customization for specific applications.  
- **Open and Accessible**: The ISA specifications, reference implementations, and software tools are freely available, allowing users to study, modify, or develop RISC-V processors without licensing fees.  
- **Ecosystem of Collaboration**: Its open nature has fostered a thriving community of hardware designers, software developers, and researchers who collaborate and innovate around the architecture.  

| ![RISCIntro_1](/images/PD/riscintro.png) | |
|:---|:---|

### How Do Software Applications Run on Hardware?

- **Hardware Dependence**: All software applications require hardware to function, with system software acting as the bridge to translate applications into a format hardware understands.  
- **System Software Components**:  
  - **Operating System (OS)**: Provides an environment for applications to run and handles memory management, process scheduling, and I/O operations. It also translates programs into architecture-specific assembly language instructions for execution.  
  - **Compiler**: Converts high-level programming languages (e.g., C, Java) into assembly-level instructions compatible with the target hardware architecture (e.g., MIPS, x86, RISC-V).  
  - **Assembler**: Translates assembly-level code into binary machine code (0s and 1s), which hardware can directly execute.  

| ![SWtoHW_1](/images/PD/swtohw1.png) | ![SWtoHW_2](/images/PD/swtohw2.png) |
|:---|:---|

| ![SWtoHW_3](/images/PD/swtohw3.png) | ![SWtoHW_4](/images/PD/swtohw4.png) |
|:---|:---|

### 2. Overview of ASIC Design Flow using OpenLane

### OpenLANE ASIC Design Flow

Main requirements of Digital ASIC Design:

- RTL Design
- EDA Tools
- PDK

| ![flow1](/images/PD/flow1.png) | ![flow2](/images/PD/flow2.png) | ![flow3](/images/PD/flow3.png) |
|:---|:---|:---|

#### RTL IPs (Register Transfer Level Intellectual Property)

- **Definition**: RTL IPs are reusable and pre-verified digital hardware blocks described at the Register Transfer Level, representing data flow between registers and associated operations.  
- **Purpose**: In IC design, they serve as building blocks integrated into larger designs, allowing abstraction and focus on higher-level features.  
- **Advantages**:  
  - Accelerates design productivity and reduces time-to-market.  
  - Improves design reliability by leveraging pre-tested components.  
  - Encourages reuse, enabling designers to build complex systems through IP integration.  
- **Sources**: RTL IPs can be developed in-house or licensed from vendors, streamlining design efforts while maintaining reliability.  

#### EDA Tools (Electronic Design Automation)

- **Role**: EDA tools aid in designing and analyzing electronic systems, including ICs and PCBs, by automating design tasks to enhance efficiency and reduce development time.  

Note: Open Source RTL IPs and competitive EDA tools have been available. However, an OpenSource PDK was not available until Google collaborated with SkyWater to open source the skywater-130nm PDK.

#### Process Design Kit (PDK)

- **Definition**: A PDK is a set of files modeling a semiconductor fabrication process, used by design tools for IC development.  
- **Traditional vs. Open-Source**:  
  - Traditional PDKs are proprietary and tied to specific foundries, often requiring non-disclosure agreements.  
  - Open-source PDKs promote collaboration, innovation, and accessibility by allowing customization to meet designer needs.  
- **Benefits of Open-Source PDKs**:  
  - Enhance knowledge sharing and lower barriers to entry for new designers.  
  - Foster participation in IC development within a collaborative community.  
- **Examples**: Notable open-source PDKs include SKY130, GFU180, and ASAP7.
- Interface between FAB and the designer, Process Design Kit, include:
  - Process Design Rule: DRC, LVS, PEX
  - Device Models
  - Digital Standard Cell Libraries
  - I/O Libraries

#### SKY130 Stackup

| ![sky130stackup](/images/PD/sky130stackup.png) | |
|:---|:---|

### Simplified RTL to GDSII ASIC Design Flow

- ASIC implementation consists of numerous steps involving lots of detailed sub-processes at each step.
- A design methodology is needed for a successful ASIC implementation without any hiccups.
- The methodology is implemented through a flow that pieces together different tools to carry out the different steps of the design process from RTL to GDSII tapeout. 
- Also, called Automated PnR and/or Physical Implementation.

| ![flow4](/images/PD/flow4.png) | |
|:---|:---|

#### RTL to GDSII Flow Steps  

- **RTL Design**:  
  - The process starts with the RTL design, where the digital circuit's behavior is defined using a hardware description language (HDL) such as VHDL or Verilog.  
  - This stage captures the functional behavior, logic, and data paths of the circuit.  

- **RTL Synthesis**:  
  - Converts the high-level RTL description into a gate-level netlist by mapping the RTL to a library of standard cells (SCL - Standard Cell Library).  
  - The process focuses on optimizing for area, power, and timing.  
  - The resulting gate-level netlist serves as the input for subsequent stages.  

- **Floor and Power Planning**:  
  - Chip Floor-planning: Partition chip's area and determining the placement of key components, functional blocks, building macros, IPs and place the I/O pads in the pad ring.
  - Macro Floor-planning: Defines chip dimensions, pin locations, the internal row definitions of the macro, locations of critical modules, power grid layout, and I/O pin placement.  
  - Power-Planning: Design the Power Distribution Network (PDN) to ensure stable and efficient power delivery taking into account the different voltage rails, power usage trends and requirements of different blocks within the digital core, I/O and other AMS circuits.  
  - Key objectives include area partitioning, power distribution, signal flow optimization, and setting design constraints.

- **Placement**:  
  - Assigns physical locations to each gate-level cell on the chip layout.
  - Place the cells on the floorplan rows aligned with the sites, non-overlapping with each other.
  - Focuses on minimizing wirelength, optimizing signal delay, and ensuring design rule compliance.  
  - Techniques like global and detailed placement are used to achieve optimal results.  

- **Clock Tree Synthesis (CTS)**:  
  - Constructs a clock distribution network to ensure balanced and efficient clock signal delivery to sequential elements (e.g., flip-flops, registers).  
  - Aims to minimize clock skew and meet timing requirements.  
  - Different types: H-tree, X-tree, Fish bone, Clock mesh.

- **Routing**:  
  - Implement the interconnections using the available metal layers. Metal tracks form a routing grid, which is huge.
  - Connects gates and components using wires and vias based on placement data.  
  - Perform routing using a divide and conquer approach: Global routing generates routing guides and Detailed routing uses the routing guides to implement the actual wires
  - Ensures compliance with design rules, avoids congestion, and optimizes factors like signal integrity and manufacturability.  

- **Sign-off Analysis**:  
  - The final stage of verification, involving thorough checks and simulations to ensure the design meets all specifications.  
  - Confirms functionality, performance, power, and reliability before fabrication.  
  - Physical Verification
    - Design Rule Checking (RDC)
    - Layout vs. Schematic (LVS)
  - Timing Verification
    - Static Timing Analysis (STA)

- **GDSII File Generation**:
  - Produces the GDSII file, the standard format containing the chip's geometric layout and manufacturing details.  
  - Includes information on shapes, layers, masks, and other fabrication requirements.  

| ![rtlgds1](/images/PD/rtlgds9.png) | ![rtlgds2](/images/PD/rtlgds8.png) | ![rtlgds3](/images/PD/rtlgds7.png) |
|:---|:---|:---|

| ![rtlgds4](/images/PD/rtlgds6.png) | ![rtlgds5](/images/PD/rtlgds5.png) | ![rtlgds6](/images/PD/rtlgds4.png) |
|:---|:---|:---|

| ![rtlgds7](/images/PD/rtlgds3.png) | ![rtlgds8](/images/PD/rtlgds2.png) | ![rtlgds9](/images/PD/rtlgds1.png) |
|:---|:---|:---|

### Introduction to OpenLANE

#### OpenLane Overview

- **Purpose and Capabilities**:
  - OpenLane is a fully automated RTL to GDSII flow designed for ASIC implementation.  
  - It encompasses all steps from RTL design to generating the GDSII layout file.  

- **Key Components**:
  - Utilizes various tools such as OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, and KLayout.  
  - Includes custom scripts to enhance design exploration and optimization.
  - The design exploration utility is also used for regression testing (CI).

- **Supported PDKs**:
  - Supports the A and B variants of the Sky130 PDK and the C variant of the GF180MCU PDK.  
  - Provides documentation to extend support for other PDKs, including proprietary ones.  

- **User Configuration**:
  - Abstracts the complexity of open-source utilities by allowing users to manage configurations through a single file.

- **Other points**:
  - Started as an Open-Source Flow for a True Open Source Tape-out Experiment
  - Main Goal: Produce a clean GDSII with no human intervention (no-human-in-the-loop)
  - Clean means: No LVS Errors and No DRC Errors
  - Tuned for SkyWater 130nm Open PDK. Also supports XFAB180 and GF130G
  - Containerized: Functional out of the box
  - Can be used to harden Macros and Chips
  - Two modes of operation: Autonomous or Interactive
  - Supports Design Space Exploration: Find the best set of flow configurations

- OpenLANE project GitHub Page: [OpenLANE GitHub](https://github.com/efabless/openlane)
- OpenLANE Readme Page: [OpenLANE ReadMe](https://openlane.readthedocs.io/en/latest/flow_overview.html)

### OpenLANE ASIC Flow and Architecture

| ![OpenLANEflow1](/images/PD/OpenLANEflow.png) |  |
|:---|:---|

OpenLane flow consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages. OpenLane can also be run interactively as shown [here](https://github.com/The-OpenROAD-Project/OpenLane/blob/master/docs/source/reference/interactive_mode.md). Everything in Floorplanning through Routing is done using OpenROAD and its various sub-utilities.

1. **Synthesis**
    1. [`yosys/abc`](https://github.com/YosysHQ/yosys) - Perform RTL synthesis, technology mapping and formal verification.
    2. [`OpenSTA`](https://github.com/The-OpenROAD-Project/OpenSTA) - Performs static timing analysis on the resulting netlist (post synthesis) to generate timing reports.
2. **Floorplaning**
    1. [`init_fp`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/ifp) - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing).
    2. [`ioplacer`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/ppl) - Places the macro input and output ports.
    3. [`pdngen`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/pdn) - Generates the power distribution network.
    4. [`tapcell`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/tap) - Inserts welltap and decap cells in the floorplan.
    5. [`magic`](https://github.com/RTimothyEdwards/magic) - Fake antenna diode insertion.
3. **Placement**
    1. [`RePLace`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/replace) - Performs global placement
    2. [`Resizer`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/rsz) - Performs optional optimizations on the design
    3. [`OpenDP`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/dpl) - Performs detailed placement to legalize the globally placed components
4. **CTS**
    1. [`TritonCTS`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/cts) - Synthesizes the clock distribution network (the clock tree)
5. **Logical Equivalence Check (LEC)**
    1. [`yosys`](https://github.com/YosysHQ/yosys) - Every time the netlist is modified (ECO), verification must be performed. CTS modifies the netlist and Post Placement optimizations modifies the netlist. LEC is used to formally confirm that the function did not change by modifying the netlist.
6. **Routing**
    1. [`FastRoute`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/cts) - Performs global routing to generate a guide file for the detailed router
    2. [`TritonRoute`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/TritonRoute) - Performs detailed routing
    3. [`OpenRCX`](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/rcx) - Performs SPEF extraction
7. **Fake Antenna Diodes removal**
    1. `Custom scripts in OpenROAD`
8. **RC Extraction**
    1. `DEF2SPEF`
9. **Post-route STA**
    1. `OpenSTA`
10. **Tapeout**
    1. [`Magic`](https://github.com/RTimothyEdwards/magic) - Streams out the final GDSII layout file from the routed def
    2. [`KLayout`](https://github.com/KLayout/klayout) - Streams out the final GDSII layout file from the routed def as a back-up
11. **Signoff**
    1. [`Magic`](https://github.com/RTimothyEdwards/magic) - Performs DRC Checks & Antenna Checks
    2. [`KLayout`](https://github.com/KLayout/klayout) - Performs DRC Checks
    3. [`Netgen`](https://github.com/RTimothyEdwards/netgen) - Performs LVS Checks
    4. [`CVC`](https://github.com/d-m-bailey/cvc) - Performs Circuit Validity Checks

| ![olflow1](/images/PD/olflow1.png) | ![olflow2](/images/PD/olflow2.png) | ![olflow3](/images/PD/olflow3.png) |
|:---|:---|:---|

| ![olflow4](/images/PD/olflow4.png) | ![olflow5](/images/PD/olflow5.png) | ![olflow6](/images/PD/olflow6.png) |
|:---|:---|:---|

| ![olflow7](/images/PD/olflow7.png) | ![olflow8](/images/PD/olflow8.png) | ![olflow9](/images/PD/olflow9.png) |
|:---|:---|:---|

| ![olflow10](/images/PD/olflow10.png) | ![olflow11](/images/PD/olflow11.png) | ![olflow12](/images/PD/olflow12.png) |
|:---|:---|:---|

Other key OpenLane integrated open source tools over the execution stages:

- Placement: [OpenPhySyn](https://github.com/scale-lab/OpenPhySyn) (formerly)
- Routing: [CU-GR](https://github.com/cuhk-eda/cu-gr) (formerly) and [DR-CU](https://github.com/cuhk-eda/cu-gr) (detailed)
- SPEF Extraction: [SPEF-Extractor](https://github.com/AUCOHL/spef-extractor) (formerly)

| ![OpenLANEflow2](/images/PD/OpenLANEflow2.png) |  |
|:---|:---|

### OpenROAD

- Everything in Floorplanning through Routing in the OpenLANE flow is done using [OpenROAD](https://github.com/The-OpenROAD-Project/OpenROAD) and its various sub-utilities.
- Also called automated PnR (Place and Route)
- Performs:
  - Floor/Power Planning
  - End Decoupling Capacitors and Tap cells insertion
  - Placement: Global and Detailed
  - Post placement optimization
  - Clock Tree Synthesis (CTS)
  - Routing: Global and Detailed

#### Handling of Antenna Rules Violations in OpenLANE flow

- When a metal wire segment is fabricated, it can act as an antenna.
  - Reactive ion etching causes charge to accumulate on the wire.
  - Transistor gates can be damaged during fabrication

| ![antennarule1](/images/PD/antennarule1.png) |
|:---|

#### Two solutions

| ![antennarule2](/images/PD/antennarule2.png) |
|:---|

- Bridging attaches a higher layer intermediary
  - Requires Router awareness (not there yet!)
- Add antenna diode cell to leak away charges
  - Antenna diodes are provided by the SCL

**Methodology followed**: a preventive approach

- Add a Fake Antenna Diode next to every cell input after placement
- Run the Antenna Checker (Magic) on the routed layout
- If the checker reports a violation on the cell input pin, replace the Fake Diode cell

| ![antennarule3](/images/PD/antennarule3.png) |
|:---|

### 3. Familiarize with OpenLANE flow

The OpenLANE flow can be configured using the following available variables for design
  * **Flow Configuration variables**: [https://openlane.readthedocs.io/en/latest/reference/configuration.html](https://openlane.readthedocs.io/en/latest/reference/configuration.html)
  * **PDK Configuration variables**: [https://openlane.readthedocs.io/en/latest/reference/pdk_configuration.html](https://openlane.readthedocs.io/en/latest/reference/pdk_configuration.html)

### OpenLane Output

All output run data is placed by default under ./designs/design_name/runs. Each flow cycle will output a timestamp-marked folder containing the following file structure:

```
<design_name>
├── config.json/config.tcl
├── runs
│   ├── <tag>
│   │   ├── config.tcl
│   │   ├── {logs, reports, tmp}
│   │   │   ├── cts
│   │   │   ├── signoff
│   │   │   ├── floorplan
│   │   │   ├── placement
│   │   │   ├── routing
│   │   │   └── synthesis
│   │   ├── results
│   │   │   ├── final
│   │   │   ├── cts
│   │   │   ├── signoff
│   │   │   ├── floorplan
│   │   │   ├── placement
│   │   │   ├── routing
│   │   │   └── synthesis
```

To delete all generated runs under all designs:
`make clean_runs`

### Installation of Ubuntu using UTM for MacOS

Reference Links - [Link1](https://github.com/AnupriyaKrishnamoorthy/NASSCOM-PD-ANU.git), [Link2](https://github.com/arunkpv/vsd-hdp/blob/main/docs/Day_20.md) and [Link3](https://www.youtube.com/watch?v=MVLbb1aMk24)

### OpenLane Installation

Prior to the installation of the OpenLane install the dependencies and packages using the command shown below :</br>
``` 
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
```

Docker Installation :</br>

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 

# Check for successful installation
sudo docker run hello-world 
```

**Steps to install OpenLane, PDKs and Tools**</br>
```
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane --recurse-submodules 
cd OpenLane
make
make test
cd /home/anupriyavsd/OpenLane/designs/ci
cp -r * ../
```
After a successful ```make``` when you run ```make test``` you will have a ```Basic test passed``` on your terminal. At this step when openlane is running, include the picorv32a.
In order to start open lane and run synthesis we would follow the next steps.

### OpenLANE Terminal

The entry point for OpenLANE is the `./flow.tcl` script. This script is used to run the flow, start interactive sessions, select the configuration and create OpenLane design files.
  * To run the automated flow for a design

```
./flow.tcl -design <design_name>
```

  * To start an [**interactive session**](https://openlane.readthedocs.io/en/latest/reference/interactive_mode.html)

```
 ./flow.tcl -interactive
```

### Steps to run synthesis in OpenLane Interactive mode

```
cd ~/OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```

| ![openlane](/images/PD/openlane1.png) |
|:---|

To view nelist
```
cd /home/anupriyavsd/OpenLane/designs/picorv32a/runs/RUN_2024.05.20_09.42.05/results/synthesis
vim picorv32a.v
```

To view the report:
```
cd /home/anupriyavsd/OpenLane/designs/picorv32a/runs/RUN_2024.05.20_09.42.05/reports/synthesis
vim 1-synthesis.AREA_0.stat.rpt
```

```
63. Printing statistics.

=== picorv32 ===

   Number of wires:               8989
   Number of wire bits:           9371
   Number of public wires:        1512
   Number of public wire bits:    1894
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:               9269
     sky130_fd_sc_hd__a2111o_2       8
     sky130_fd_sc_hd__a211o_2       66
     sky130_fd_sc_hd__a211oi_2       8
     sky130_fd_sc_hd__a21bo_2       17
     sky130_fd_sc_hd__a21boi_2       5
     sky130_fd_sc_hd__a21o_2       248
     sky130_fd_sc_hd__a21oi_2      144
     sky130_fd_sc_hd__a221o_2      104
     sky130_fd_sc_hd__a221oi_2       2
     sky130_fd_sc_hd__a22o_2       182
     sky130_fd_sc_hd__a22oi_2        3
     sky130_fd_sc_hd__a2bb2o_2       8
     sky130_fd_sc_hd__a311o_2       14
     sky130_fd_sc_hd__a31o_2       136
     sky130_fd_sc_hd__a31oi_2        3
     sky130_fd_sc_hd__a32o_2        35
     sky130_fd_sc_hd__a41o_2         5
     sky130_fd_sc_hd__a41oi_2        1
     sky130_fd_sc_hd__and2_2       129
     sky130_fd_sc_hd__and2b_2       31
     sky130_fd_sc_hd__and3_2       118
     sky130_fd_sc_hd__and3b_2       49
     sky130_fd_sc_hd__and4_2        48
     sky130_fd_sc_hd__and4b_2       10
     sky130_fd_sc_hd__and4bb_2       4
     sky130_fd_sc_hd__buf_1       2530
     sky130_fd_sc_hd__buf_2         14
     sky130_fd_sc_hd__conb_1       106
     sky130_fd_sc_hd__dfxtp_2     1596
     sky130_fd_sc_hd__inv_2         64
     sky130_fd_sc_hd__mux2_2      1445
     sky130_fd_sc_hd__mux4_2       537
     sky130_fd_sc_hd__nand2_2      239
     sky130_fd_sc_hd__nand2b_2       3
     sky130_fd_sc_hd__nand3_2       24
     sky130_fd_sc_hd__nand3b_2       4
     sky130_fd_sc_hd__nor2_2       217
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        10
     sky130_fd_sc_hd__nor3b_2        3
     sky130_fd_sc_hd__nor4_2         2
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o2111ai_2      1
     sky130_fd_sc_hd__o211a_2      152
     sky130_fd_sc_hd__o211ai_2       3
     sky130_fd_sc_hd__o21a_2       177
     sky130_fd_sc_hd__o21ai_2       82
     sky130_fd_sc_hd__o21ba_2       11
     sky130_fd_sc_hd__o21bai_2       6
     sky130_fd_sc_hd__o221a_2       23
     sky130_fd_sc_hd__o22a_2        67
     sky130_fd_sc_hd__o2bb2a_2       4
     sky130_fd_sc_hd__o311a_2        9
     sky130_fd_sc_hd__o31a_2        11
     sky130_fd_sc_hd__o31ai_2        7
     sky130_fd_sc_hd__o32a_2         3
     sky130_fd_sc_hd__o41a_2        15
     sky130_fd_sc_hd__or2_2        294
     sky130_fd_sc_hd__or2b_2        30
     sky130_fd_sc_hd__or3_2         41
     sky130_fd_sc_hd__or3b_2        18
     sky130_fd_sc_hd__or4_2         25
     sky130_fd_sc_hd__or4b_2         5
     sky130_fd_sc_hd__or4bb_2        3
     sky130_fd_sc_hd__xnor2_2       70
     sky130_fd_sc_hd__xor2_2        37

   Chip area for module '\picorv32': 98792.249600

```

```
Flop ratio = Number of D Flip flops = 1596  = 0.1721868 or 17.21868%
             ______________________   _____
             Total Number of cells    9269
```

**NOTE:** The order of precedence of the config files in the OpenLANE flow is as follows, with the settings in the highest priority config overriding the values set in the previous config files:  From lowest to highest:
  * Default OpenLANE config values
  * openlane/designs/<design-name>/config.tcl
  * openlane/designs/<design-name>/sky130A_sky130_fd_sc_hd_config.tcl

</details>

<details>
<summary> Day 2: Good Floorplan Vs Bad Floorplan and Introduction to Library Cells </summary>

### 1. Chip Floor planning considerations

### Utilization factor and Aspect ratio

| ![chipfloorplanning1](/images/PD/chipfloorplanning1.png) | ![chipfloorplanning2](/images/PD/chipfloorplanning2.png) | ![chipfloorplanning3](/images/PD/chipfloorplanning3.png) |
|:---|:---|:---|

| ![chipfloorplanning4](/images/PD/chipfloorplanning4.png) | ![chipfloorplanning5](/images/PD/chipfloorplanning5.png) | ![chipfloorplanning6](/images/PD/chipfloorplanning6.png) |
|:---|:---|:---|

  * Define W, H of core and die
  * Utilization Factor = (Area occupied by netlist)/(Total area of the core)
    * Usually we aim for 50-60 % Utilization Factor
  * Aspect Ratio = Height of the core/ Width of the core
    * When the aspect ratio is 1, the chip is squared in shape

### Preplaced Cells

| ![preplacedcells1](/images/PD/preplacedcells1.png) | ![preplacedcells2](/images/PD/preplacedcells2.png) | ![preplacedcells3](/images/PD/preplacedcells3.png) |
|:---|:---|:---|

| ![preplacedcells4](/images/PD/preplacedcells4.png) | ![preplacedcells5](/images/PD/preplacedcells5.png) | |
|:---|:---|:---|

- Pre-placed cells are large, complex modules (e.g., memory blocks, functional units) that are designed separately and positioned in the layout before placement and routing (PnR) begins.
- These cells are strategically placed to optimize performance, integrate IP blocks, and enhance power efficiency.
- They are often reused across different designs to save time and maintain consistency.
- Pre-placed cells occupy fixed positions in the design, as specified by the user, to ensure optimal placement for performance and power distribution.
- Automated PnR tools handle the placement of the remaining standard logical cells around the pre-placed blocks.

### Decoupling Capacitor

| ![decouplingcap1](/images/PD/decouplingcapacitor1.png) | ![decouplingcap2](/images/PD/decouplingcapacitor2.png) | ![decouplingcap3](/images/PD/decouplingcapacitor3.png) |
|:---|:---|:---|

| ![decouplingcap4](/images/PD/decouplingcapacitor4.png) | ![decouplingcap5](/images/PD/decouplingcapacitor5.png) | ![decouplingcap6](/images/PD/decouplingcapacitor6.png) |
|:---|:---|:---|


- Stabilize power supply voltage and mitigate noise in integrated circuits (ICs) and printed circuit boards (PCBs).
- Prevent voltage drops by providing immediate current during switching activities.
- Act as local reservoirs of electrical charge to handle sudden shifts in current demand.
- Isolate circuits from the main power supply during switching, ensuring consistent power delivery.
- Filter out noise and voltage fluctuations, preserving signal integrity.
- During Switching:
  - Capacitors discharge to supply current, supporting the circuit when the power rail cannot meet instantaneous demands.
- During Idle Periods:
  - The capacitors recharge from the main power source, maintaining readiness for future switching events.
- Proximity:
  - Decoupling capacitors are placed as close as possible to power-hungry components (e.g., microprocessors, memory modules).
- Impact of Placement:
  - Close placement minimizes inductance and resistance, ensuring efficient energy transfer and reducing impedance.
- Capacitance Selection:
  - The capacitance value should align with the current demands and frequency of the circuit.
  - Higher capacitance enhances noise suppression but may lead to larger physical size and higher costs.
- Component Protection:
  - Helps reduce ground disturbances, signal interference, and electromagnetic noise.
- Pre-placed cells are surrounded by decoupling capacitors to handle switching current demands (di/dt).
- Capacitors decouple circuits from the main power rail (VDD) to maintain performance stability.
- They lower power distribution network impedance (Zpdn) for efficient operation across required frequency ranges.

### Power planning

| ![powerplanning1](/images/PD/powerplanning1.png) | ![powerplanning2](/images/PD/powerplanning2.png) | ![powerplanning3](/images/PD/powerplanning3.png) |
|:---|:---|:---|

| ![powerplanning4](/images/PD/powerplanning4.png) | ![powerplanning5](/images/PD/powerplanning5.png) | ![powerplanning6](/images/PD/powerplanning6.png) |
|:---|:---|:---|

| ![powerplanning7](/images/PD/powerplanning7.png) | | |
|:---|:---|:---|

- Ensures even distribution of power (VDD) and ground (GND) connections across the chip.  
- Guarantees reliable performance and functionality by minimizing voltage fluctuations.  
- **Efficient Power Delivery:**  
  - Distributes power uniformly across the integrated circuit (IC).  
  - Reduces voltage drops, enhancing overall power efficiency.  
- **Minimization of Ground Bounce:**  
  - Prevents variations in GND voltage levels caused by transient currents.  
- **Reduction of Power Supply Noise:**  
  - Mitigates VDD fluctuations resulting from switching activities.  
  - Enhances circuit stability and prevents functional errors.  
- Improves power integrity by reducing localized voltage drops.  
- Lowers the risk of performance degradation due to noise or transient events.  
- Enhances the overall reliability and longevity of the chip.  
- Minimizes the likelihood of functional failures during high-speed switching.  
- Ensures consistent voltage levels across the IC, optimizing operational efficiency.  
- Supports better handling of dynamic current demands during circuit operation.

### Pin Placement

| ![pinplacement1](/images/PD/pinplacement1.png) | ![pinplacement2](/images/PD/pinplacement2.png) |
|:---|:---|

| ![pinplacement3](/images/PD/pinplacement3.png) | ![pinplacement4](/images/PD/pinplacement4.png) |
|:---|:---|

- Involves determining the location of input/output (I/O) pins on a chip or circuit board.  
- Crucial for optimizing signal flow, minimizing interference, and simplifying manufacturing and testing processes.  
- **Signal Integrity:** Ensures signals remain strong and clear as they travel across the chip.  
- **Power Distribution:** Facilitates even distribution of power to prevent localized voltage drops.  
- **Thermal Management:** Strategically places pins to aid in heat dissipation.  
- **Compatibility:** Aligns with standard connectors and packaging requirements for ease of integration.
- Enhances system reliability and performance.  
- Simplifies assembly and testing processes.  
- Improves user experience by ensuring robust design.
- **Directional Flow:**  
  - East to West  
  - North to South  
  - Combination: East/North to West/South  
- **Random Ordering:**  
  - Pin arrangement may be random unless specified otherwise.  
- Requires coordination between Front-End and Back-End teams to ensure optimal pin placement.  
- Effective communication prevents design conflicts and ensures smooth integration.  
- Clock pins are typically larger to minimize net resistance.  
- Reducing resistance enhances clock signal integrity and minimizes delays.

### Logical Cell placement blockage

- So that no cells are placed by the PnR tool inside the IP blocks/ macro area.

### Lab: Run floorplan using OpenLANE and review the layout in Magic

### Floorplanning: To perform floorplanning

```
run_floorplan
```

In the workshop they were mentioning about how to change the FP_IO_HMETAL and FP_IO_VMETAL, however, this was for the old openlane and in the new version it is changed as HLAYER and VLAYER. You can learn all about it [here](https://github.com/The-OpenROAD-Project/OpenLane/blob/master/docs/source/reference/pdk_configuration.md)

![floorplan1](/images/PD/floorplan1.png)

To view the floorplan in magic :

```
/home/akhilgunda/OpenLane/designs/picorv32a/runs/<LATEST_RUN_FOLDER>/results/floorplan

magic -T /home/akhilgunda/OpenLane/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32a.def &
```

![magicfloorplan1](/images/PD/magicfloorplan1.png) <br>
![magicfloorplan2](/images/PD/magicfloorplan2.png)

### 2. Library Binding and Placement

### Placement and Routing

1) Bind netlist with physical cells
  * Library files: Shape, dimension info, power & timing/ delay info. Various flavors of all available std cells

2) Placement
  * The location of pre-placed cells are fixed and PnR tools will not place any cells inside that area.
  * Initial global placement based on the input, output pins to reduce wire lengths

| ![placement1](/images/PD/placement1.png) | ![placement2](/images/PD/placement2.png) | ![placement3](/images/PD/placement3.png) |
|:---|:---|:---|

3) Optimize placement
  * Ensure that Signal Integrity is maintained: Estimate the wire length and capacitance and insert repeaters/ buffers. MaxTrans delay/ signal slew

| ![placement4](/images/PD/placement4.png) | ![placement5](/images/PD/placement5.png) | ![placement3](/images/PD/placemen6.png) |
|:---|:---|:---|

| ![placement7](/images/PD/placement7.png) | ![placement8](/images/PD/placement8.png) | |
|:---|:---|:---|

4) Congestion aware placement using RePlAce followed by detailed placement using OpenDP.
  * Global placement: HPWL (Half-Parameter Wire Length) based

| ![placement9](/images/PD/placement9.png) | ![placement10](/images/PD/placement10.png) |
|:---|:---|

### Lab: Run placement using OpenLANE and review the layout in Magic

### Placement: To perform placement

```
run_placement
```

- The `run_placement` command runs the global placement followed by detailed placement.
- First the global placement happens, where the main objective is to reduce the wire length. Algorithm used is Half-Parameter Wire Length (HPWL).
- Then detailed placement is performed to legalize the globally placed components.

![placement11](/images/PD/placement11.png)

To view the placement in magic :

```
/home/akhilgunda/OpenLane/designs/picorv32a/runs/<LATEST_RUN_FOLDER>/results/placement

magic -T /home/akhilgunda/OpenLane/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32a.def &
```

![magicplacement1](/images/PD/magicplacement1.png) <br>
![magicplacement2](/images/PD/magicplacement2.png)

### 3. Cell design and characterization flows

Library is a place where we get information about every cell. It has differents cells with different size, functionality,threshold voltages. Different stages.

- Inputs : PDKS(process design kit) : DRC & LVS, SPICE Models, library & user-defined specs.
- Design Steps :Circuit design, Layout design (Art of layout Euler's path and stick diagram), Extraction of parasitics, Characterization (timing, noise, power).
- Outputs: CDL (circuit description language), LEF, GDSII, extracted SPICE netlist (.cir), timing, noise and power .lib files

| ![celldesign3](/images/PD/celldesign3.png) | ![celldesign4](/images/PD/celldesign4.png) |
|:---|:---|

| ![celldesign5](/images/PD/celldesign5.png) | ![celldesign1](/images/PD/celldesign1.png) |
|:---|:---|

1) **Inputs for Cell Design Flow**  
From foundry, PDKs:  
  - DRC, LVS rules (eg: lambda-based design rules)
  - SPICE modelsfor the NMOS & PMOS devices
  - Library and user-defined specifications
    - Cell height (separation b/w VDD and GND rails) must be maintained
    - Cell width is determined by the drive strengths required
    - Supply voltage
    - Metal layers requirements
    - Pin locations
    - Drawn gate-length

2) **Design Steps**
- Circuit Design
  - Transistor sizing based on current drive, voltage transfer and other performance/ quality parameter requirements
- Layout Design
  - Art of layout - Euler's path and stick diagram
- Characterization
  1) Read the nmos, pmos models
  2) Read the extracted spice netlist
  3) Recognize the behaviour of the cell
  4) Read the sub-circuits
  5) Attach the power sources
  6) Apply the stimulus
  7) Provide the necessary output capacitance
  8) Provide the necessary simulation commands

- Now, feed in all this to [**GUNA characterization tool**](https://www.paripath.com/Products/Guna)  --> to generate Timing, noise, power .libs, function

3) **Outputs**
- CDL (Circuit Description Language)
- GDSII, LEF (dimensions), extracted spice netlist (.cir)
- Timing, noise, power .libs, function

| ![celldesign6](/images/PD/celldesign6.png) | ![celldesign7](/images/PD/celldesign7.png) | ![celldesign8](/images/PD/celldesign8.png) |
|:---|:---|:---|

| ![celldesign9](/images/PD/celldesign9.png) | ![celldesign10](/images/PD/celldesign10.png) | ![celldesign11](/images/PD/celldesign11.png) |
|:---|:---|:---|

| ![celldesign12](/images/PD/celldesign12.png) | ![celldesign13](/images/PD/celldesign13.png) | ![celldesign14](/images/PD/celldesign14.png) |
|:---|:---|:---|

| ![celldesign15](/images/PD/celldesign15.png) | ![celldesign16](/images/PD/celldesign16.png) |  |
|:---|:---|:---|

Standard Cell Characterization Flow

A typical standard cell characterization flow that is followed in the industry includes the following steps:

- Read in the models and tech files
- Read extracted spice Netlist
- Recognise behavior of the cells
- Read the subcircuits
- Attach power sources
- Apply stimulus to characterization setup
- Provide neccesary output capacitance loads
- Provide neccesary simulation commands

Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing characterization, power characterization and noise characterization.

| ![celldesign2](/images/PD/celldesign2.png) |
|:---|

### 4. General timing characterization parameters

In standard cell characterisation, One of the classification of libs is timing characterisation.

### Timing threshold definitions

Timing defintion |	Value
-------------- | --------------
slew_low_rise_thr	| 20% value
slew_high_rise_thr | 80% value
slew_low_fall_thr |	20% value
slew_high_fall_thr |	80% value
in_rise_thr	| 50% value
in_fall_thr |	50% value
out_rise_thr |	50% value
out_fall_thr | 50% value

#### Propagation Delay and Transition Time

**Propagation Delay**

- The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. Poor choice of threshold values lead to negative delay values. Even thought you have taken good threshold values, sometimes depending upon how good or bad the slew, the dealy might be still +ve or -ve.
- Problematic cases:
  - Choice of threshold levels
  - Large RC delays (due to improper designs and/ or large loads, long routes)

```
Propagation delay = time(out_thr) - time(in_thr)
```

**Transition Time**

The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.

```
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```

| ![celldesign17](/images/PD/celldesign17.png) |
|:---|

</details>

<details>
<summary> Day 3: Design library cell using Magic Layout and ngspice characterization </summary>

### 1. Labs for CMOS inverter ngspice simulations

### IO Placer revision

In Openlane, PnR is a iterative flow and hence, we can make changes to the environment variables in the fly to observe the changes in our design.

### SPICE deck creation for CMOS inverter

Before performing a SPICE simulation we need to create SPICE Deck SPICE Deck provides information about the following:

- Component connectivity - Connectivity of the Vdd, Vss,Vin, substrate. Substrate tunes the threshold voltage of the MOS.
- Component values - values of PMOS and NMOS, Output load, Input Gate Voltage, supply voltage.
- Node Identification and naming - Nodes are required to define the SPICE Netlist
- Simulation commands
- Model file - information of parameters related to transistors Simulation of CMOS using different width and lengths. From the waveform, irrespective of switching the shape of it are almost the same.

| ![spice1](/images/PD/spice1.png) | ![spice2](/images/PD/spice2.png) |
|:---|:---|

### Switching Threshold (Vm)

In physical design, the switching threshold (Vm) is the critical voltage level at which a CMOS inverter transitions between outputting a "0" and a "1". This threshold is essential for defining how efficiently and reliably the inverter operates within a computer chip. Vm impacts the inverter’s performance by influencing signal integrity, noise margins, and overall stability. Maintaining the right Vm ensures proper logic levels and reduces the likelihood of errors during signal transitions.  

To evaluate the behavior of a CMOS inverter, two types of tests are conducted – static and dynamic. Static testing assesses the inverter’s performance under stable conditions, focusing on factors like propagation delay, power consumption, and noise tolerance. Dynamic testing, on the other hand, examines the inverter during active switching to evaluate speed, signal strength, and detect potential issues like glitches. Both tests are crucial for ensuring the CMOS inverter functions accurately and efficiently, contributing to the overall performance and reliability of the chip.

| ![spice3](/images/PD/spice3.png) | ![spice4](/images/PD/spice4.png) |
|:---|:---|

Through transient analysis, we calculate the rise and fall delays of the CMOS by SPICE Simulation. As we know delays are calculated at 50% of the final values.

### Lab steps to git clone vsdstdcelldesign

First, clone the required mag files and spicemodels of inverter,pmos and nmos sky130.
```

git clone https://github.com/nickson-jose/vsdstdcelldesign.git

```

To view the layout of the inverter in magic :

```

magic -T ./libs/sky130A.tech sky130_inv.mag &

```

Upon running that command we would be able to see the ```CMOS Inverter``` in magic.

![magicInv](/images/PD/magicInv.png)

### 2. Inception of Layout: CMOS fabrication process (16-Mask CMOS Process)

- The fabrication of an integrated circuit requires a variety of physical and chemical processes performed on a semiconductor substrate. In general, the various processes used to make an IC fall into three categories: film deposition, patterning, and semiconductor doping.
- Films of both conductors (such as polysilicon, aluminum, copper etc.) and insulators (various forms of silicon dioxide, silicon nitride, and others) are used to connect and isolate transistors and their components.
- Selective doping of various regions of silicon allow the conductivity of the silicon to be changed.
- Fundamental to all of these processes is photolithography - the formation of three-dimensional relief images on the substrate for subsequent transfer of the pattern to the substrate [Ref](https://www.lithoguru.com/scientist/lithobasics.html).

The 16-mask CMOS design fabrication process involves several steps to create integrated circuits. Here is a brief description of each step:

- Selecting a substrate with suitable properties
- Creating Active Region for the transistors
- N-Well and P-Well Formation
- Formation of Gate
- Lightly-Doped Drain (LDD) Formation
- Source and Drain Formation
- Formation of Contacts and Local Interconnects
- Formation of Higher Level Metal Layers

### 1) Selecting a substrate with suitable properties

- The process begins with preparing a silicon wafer, which serves as the substrate for the integrated circuit
- P-type substrate with high resistivity (5~50ohms)
- Doping level ($10^{15} cm^{-3}$)
- Orientation (100)
- Substrate doping should be less than the well doping.

| ![PSubstrate](/images/CMOS_Process/psubstrate.png) | |
|:---|:---|

### 2) Creating Active Region for the transistors

- SiO2: Insulator
- Si3N4: Protection Layer for Local Oxidation of Silicon (Oxide Growth)

| ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion1.png) | ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion2.png) |
|:---|:---|
| ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion3.png) | ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion4.png) |
| ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion5.png) | ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion6.png) |
| ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion7.png) | ![Creating_ActiveRegion](/images/CMOS_Process/creatingactiveregion8.png) |

### 3) N-Well and P-Well Formation

- The N-well regions are created on the substrate by introducing impurities, typically phosphorus, through ion implantation or diffusion and P-well regions are created using ion implantation or diffusion with boron or other suitable dopants.
- Energy required for phosphorus ions is more than boron ions due to its size.
- Ion-implantation is used to further change the channel doping to adjust the threshold voltage, $V_{TH}$ as required.

| ![NWellPWellformation1](/images/CMOS_Process/NWellPWellformation1.png) | ![NWellPWellformation2](/images/CMOS_Process/NWellPWellformation2.png) |
|:---|:---|
| ![NWellPWellformation3](/images/CMOS_Process/NWellPWellformation3.png) | ![NWellPWellformation4](/images/CMOS_Process/NWellPWellformation4.png) |
| ![NWellPWellformation5](/images/CMOS_Process/NWellPWellformation5.png) | ![NWellPWellformation6](/images/CMOS_Process/NWellPWellformation6.png) |
| ![NWellPWellformation7](/images/CMOS_Process/NWellPWellformation7.png) | ![NWellPWellformation8](/images/CMOS_Process/NWellPWellformation8.png) |
| ![NWellPWellformation9](/images/CMOS_Process/NWellPWellformation9.png) | ![NWellPWellformation10](/images/CMOS_Process/NWellPWellformation10.png) |
| ![NWellPWellformation11](/images/CMOS_Process/NWellPWellformation11.png) | ![NWellPWellformation12](/images/CMOS_Process/NWellPWellformation12.png) |
| ![NWellPWellformation13](/images/CMOS_Process/NWellPWellformation13.png) | ![NWellPWellformation14](/images/CMOS_Process/NWellPWellformation14.png) |

### 4) Formation of Gate

| ![gateformation1](/images/CMOS_Process/gateformation1.png) | ![gateformation2](/images/CMOS_Process/gateformation2.png) |
|:---|:---|
| ![gateformation3](/images/CMOS_Process/gateformation3.png) | ![gateformation4](/images/CMOS_Process/gateformation4.png) |
| ![gateformation5](/images/CMOS_Process/gateformation5.png) | ![gateformation6](/images/CMOS_Process/gateformation6.png) |
| ![gateformation7](/images/CMOS_Process/gateformation7.png) | ![gateformation8](/images/CMOS_Process/gateformation8.png) |
| ![gateformation9](/images/CMOS_Process/gateformation9.png) | ![gateformation10](/images/CMOS_Process/gateformation10.png) |
| ![gateformation11](/images/CMOS_Process/gateformation11.png) | ![gateformation12](/images/CMOS_Process/gateformation12.png) |
| ![gateformation13](/images/CMOS_Process/gateformation13.png) | ![gateformation14](/images/CMOS_Process/gateformation14.png) |

### 5) Lightly-Doped Drain (LDD) Formation

- LDD is formed to prevent hot electron effect and short channel effects.
  - PMOS Doping profile: P+, P-, N
  - NMOS doping profile: N+, N-, P

- Hot Electron Effect
  - When device size reduces, the electric field in the channel, $E = V/d$ also increases, and the energy of electron and holes attains tremendous become high enough to break si-si bonds leading to some more addition of electron and holes.
  - Their enegry might be so high that it crosses 3.2eV barrier between Si conduction band and SiO2 conduction band. If it crosses this band it might enters into oxide layer which is present above substate cause reliability issues.

- Short Channel Effects
  - WHen device size reduces, the drain field starts to penetrate into the channel and thus reducing the controlling effect of the Gate terminal voltage on the Drain current.

- Nowadays, the Source/Drain Extension is used to mitigate the effects of short channel effects.

| ![lddformation1](/images/CMOS_Process/lddformation1.png) | ![lddformation2](/images/CMOS_Process/lddformation2.png) |
|:---|:---|
| ![lddformation3](/images/CMOS_Process/lddformation3.png) | ![lddformation4](/images/CMOS_Process/lddformation4.png) |
| ![lddformation5](/images/CMOS_Process/lddformation5.png) | ![lddformation6](/images/CMOS_Process/lddformation6.png) |
| ![lddformation7](/images/CMOS_Process/lddformation7.png) | ![lddformation8](/images/CMOS_Process/lddformation8.png) |
| ![lddformation9](/images/CMOS_Process/lddformation9.png) | ![lddformation10](/images/CMOS_Process/lddformation10.png) |

### 6) Source and Drain Formation

| ![sourcedrainformation1](/images/CMOS_Process/sourcedrainformation1.png) | ![sourcedrainformation2](/images/CMOS_Process/sourcedrainformation2.png) |
|:---|:---|
| ![sourcedrainformation3](/images/CMOS_Process/sourcedrainformation3.png) | ![sourcedrainformation4](/images/CMOS_Process/sourcedrainformation4.png) |
| ![sourcedrainformation5](/images/CMOS_Process/sourcedrainformation5.png) | ![sourcedrainformation6](/images/CMOS_Process/sourcedrainformation6.png) |
| ![sourcedrainformation7](/images/CMOS_Process/sourcedrainformation7.png) | ![sourcedrainformation8](/images/CMOS_Process/sourcedrainformation8.png) |

### 7) Formation of Contacts and Local Interconnects

| ![contactslocalinterconnectformation1](/images/CMOS_Process/contactslocalinterconnectformation1.png) | ![contactslocalinterconnectformation2](/images/CMOS_Process/contactslocalinterconnectformation2.png) |
|:---|:---|
| ![contactslocalinterconnectformation3](/images/CMOS_Process/contactslocalinterconnectformation3.png) | ![contactslocalinterconnectformation4](/images/CMOS_Process/contactslocalinterconnectformation4.png) |
| ![contactslocalinterconnectformation5](/images/CMOS_Process/contactslocalinterconnectformation5.png) | ![contactslocalinterconnectformation6](/images/CMOS_Process/contactslocalinterconnectformation6.png) |
| ![contactslocalinterconnectformation7](/images/CMOS_Process/contactslocalinterconnectformation7.png) | ![contactslocalinterconnectformation8](/images/CMOS_Process/contactslocalinterconnectformation8.png) |
| ![contactslocalinterconnectformation9](/images/CMOS_Process/contactslocalinterconnectformation9.png) | ![contactslocalinterconnectformation10](/images/CMOS_Process/contactslocalinterconnectformation10.png) |
| ![contactslocalinterconnectformation11](/images/CMOS_Process/contactslocalinterconnectformation11.png) | ![contactslocalinterconnectformation12](/images/CMOS_Process/contactslocalinterconnectformation12.png) |
| ![contactslocalinterconnectformation13](/images/CMOS_Process/contactslocalinterconnectformation13.png) | |

### 8) Formation of Higher Level Metal Layers

| ![higherlevelmetalformation1](/images/CMOS_Process/higherlevelmetalformation1.png) | ![higherlevelmetalformation2](/images/CMOS_Process/higherlevelmetalformation2.png) |
|:---|:---|
| ![higherlevelmetalformation3](/images/CMOS_Process/higherlevelmetalformation3.png) | ![higherlevelmetalformation4](/images/CMOS_Process/higherlevelmetalformation4.png) |
| ![higherlevelmetalformation5](/images/CMOS_Process/higherlevelmetalformation5.png) | ![higherlevelmetalformation6](/images/CMOS_Process/higherlevelmetalformation6.png) |
| ![higherlevelmetalformation7](/images/CMOS_Process/higherlevelmetalformation7.png) | ![higherlevelmetalformation8](/images/CMOS_Process/higherlevelmetalformation8.png) |
| ![higherlevelmetalformation9](/images/CMOS_Process/higherlevelmetalformation9.png) | ![higherlevelmetalformation10](/images/CMOS_Process/higherlevelmetalformation10.png) |
| ![higherlevelmetalformation11](/images/CMOS_Process/higherlevelmetalformation11.png) | ![higherlevelmetalformation12](/images/CMOS_Process/higherlevelmetalformation12.png) |
| ![higherlevelmetalformation13](/images/CMOS_Process/higherlevelmetalformation13.png) | ![higherlevelmetalformation14](/images/CMOS_Process/higherlevelmetalformation14.png) |
| ![higherlevelmetalformation15](/images/CMOS_Process/higherlevelmetalformation15.png) | ![higherlevelmetalformation17](/images/CMOS_Process/higherlevelmetalformation17.png) |
| ![higherlevelmetalformation18](/images/CMOS_Process/higherlevelmetalformation18.png) | ![higherlevelmetalformation19](/images/CMOS_Process/higherlevelmetalformation19.png) |
| ![higherlevelmetalformation20](/images/CMOS_Process/higherlevelmetalformation20.png) | |

### 9) Final Wafer after Fabrication

| ![higherlevelmetalformation21](/images/CMOS_Process/higherlevelmetalformation21.png) |
|:---|

### Lab: Introduction to Sky130 basic layers layout and LEF using inverter

- Clone a custom standard cell design from the following github repo for this exercise
  [https://github.com/nickson-jose/vsdstdcelldesign.git](https://github.com/nickson-jose/vsdstdcelldesign.git)
- To open the design in magic: `magic -T sky130A.tech sky130_inv.mag`
- Get familiarized with the different layers in sky130 technology, refer the github link.
- To get the details about any drawn element in the layout, hover the mouse pointer over it and press `s` to select it (pressing multiple times selects the elements hierarchically).
  Then, from the **tkcon shell**, use the command `what` to print the details: <br>

**sky130 Layers in Magic for an Inverter** <br>  

|![Magicsky130Layers1](/images/PD/Magicsky130Layers1.png) |
|:---|

### Library exchange format (.lef)

- The layout of a design is defined in a specific file called LEF. It includes design rules (tech LEF) and abstract information about the cells.
  - ```Tech LEF``` -  Technology LEF file contains information about the Metal layer, Via Definition and DRCs.
  - ```Macro LEF``` -  Contains physical information of the cell such as its Size, Pin, their direction.

### Lab: Create the Inverter Standard Cell layout and extract the SPICE netlist

- The steps to layout a custom inverter standard cell in Magic is explained in this github repo: [vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign?tab=readme-ov-file#standard-cell-layout-design-in-magic)
  - Magic has an interactive DRC engine - DRC violations are updated continuously in Magic every time we make a change (draw, erase, move) in the layout.
  - When we make small changes to an existing layout, we can find out immediately if we have introduced errors, without having to completely recheck the entire layout.

- To extract the SPICE netlist of the layout including the parasitics, switch to the **tkcon shell** of Magic:
  
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

![ngspiceextract](/images/PD/ngspiceextract.png)

### Lab: Create a SPICE deck to run a simple transient simulation using ngspice

- Open the extracted netlist and correct the following:
  - Ensure the Scaling: Open the layout in magic, enable the grid (Window menu -> Grid on), and zoom in to select one unit box. Find its dimensions by the command `box` from the **tkcon shell**
  - Include the SPICE models for sky130 short-channel PMOS and NMOS.
  - Change the PMOS and NMOS model names to match the ones in the included model files - `pshort_model.0, nshort_model.0`.
- Modify the spice file to run a sample transient simulation using ngspice:
  - Add VDD and GND:

```
VDD VPWR 0 3.3V
VSS VGND 0 0V
```

  - Add a pulse source to the input node: `Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)`
  - Transient simulation: `.tran 1n 20n`
  - Since we are using the separate NMOS and PMOS models and not the sub-circuits from the lib files, modify the instance names to M0 and M1

The extracted spice list has to be modified as shown below to use ngspice to perform simulation:

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 A VPWR 0.0774f
C1 VPWR Y 0.117f
C2 A Y 0.0754f
C3 Y VGND 2f
C4 A VGND 0.45f
C5 VPWR VGND 0.781f
//.ends

.tran 1n 20n
.control
run
.endc
.end
```

To simulate the spice netlist type the following command in the terminal:

```

ngspice sky130_inv.spice

```

![ngspice1](/images/PD/ngspice1.png)

To plot the wave form type

```

plot y vs time a

```

![ngspice2](/images/PD/ngspice2.png)

- Characterization of the inverter standard cell depends on four timing parameters
  - **Rise Transition**: Time taken for the output to rise from 20% to 80% of max value
  - **Fall Transition**: Time taken for the output to fall from 80% to 20% of max value
  - **Cell Rise delay**: difference in time(50% output rise) to time(50% input fall)
  - **Cell Fall delay**: difference in time(50% output fall) to time(50% input rise)

### Lab: Introduction to (Skywater) DRC using Magic tool

The lab work includes

- In-depth overview of Magic's DRC engine
- Introduction to Google/Skywater DRC rules
- Lab : Warm-up exercise : Fixing a simple rule error
- Lab : Main exercie : Fixing or create a complex error
- Obtain the tutorial files for DRC labs from the following link:
```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
```
- The Design Rules for Skywater 130nm technology can be found here: [**https://skywater-pdk.readthedocs.io/en/main/rules.html**](https://skywater-pdk.readthedocs.io/en/main/rules.html) and [**https://github.com/google/skywater-pdk**](https://github.com/google/skywater-pdk)
- Magic: [here](http://opencircuitdesign.com/)

### DRC Lab 1: met3.mag

- To launch Magic with OpenGL or Cairo graphical interfaces, use the `-d` flag:  
  - For OpenGL: `magic -d XR &`  
  - For Cairo: `magic -d OGL &`  

- Open the tutorial file `met3.mag` in Magic through the command line or via the GUI by selecting **File -> Open...**  

- To inspect DRC (Design Rule Check) errors for a specific region:  
  - Use the left and right mouse buttons to position the cursor box around the desired area.  
  - Without exiting the main layout interface, press `:` to bring up the **tkcon console** and input the necessary command.  
  - For example, to check the DRC error for the `m3.2` area, position the cursor box and type: `:drc why`  
  - The **console** will then display the violated DRC rule.

**Rule M3.2: Spacing of metal 3 to metal 3 - 0.300µm** <br>  

|![DRClab1](/images/PD/DRCLab1.png) |
|:---|

- Vias in Magic are treated as derived layers. The drawn via represents a region filled with contact cuts, although the actual contact cuts (which correspond to the VIA2 mask layer in the GDS output) are not directly visible in the layout view. These cuts are generated based on CIF output rules in the technology file.  
  - To create an M3 contact area, enclose a region using the cursor box (left and right mouse buttons), hover over the `m3contact` icon on the sidebar, and click the middle mouse button (or press the `p` key).  
  - Alternatively, after defining the desired area with the cursor box, use the command: `paint m3contact`.  
  - To visualize the M3 contact cuts, enter `:cif see VIA2` in the layout window.  
  This visualization, known as a **feedback entry**, can be removed with the command: `feedback clear`.  
  - As a note, such rules are inherently accurate. To verify them, measure the distance from the contact cut to the M3 contact boundary by drawing a cursor box.  
  - To align the cursor box precisely with the via edge in CIF view, use the command: `snap int`.
  
|![DRClab2](/images/PD/DRCLab2.png) |
|:---|

### DRC Lab 2: poly.mag - Exercise to fix poly.9 error in Sky130 tech-file
 
- **[poly.9](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#poly)**: The required spacing for poly resistors to poly or their distance (without overlap) to diffusion/tap must be 0.480 µm.  
- This task focuses on addressing an incomplete DRC rule in the `sky130A.tech` file.  
- The section below violates the poly.9 DRC rule but isn't flagged as an error because the rule is only partially defined in the `sky130A.tech` file.  

**Rule poly.9: Poly resistor spacing to poly or diffusion/tap (without overlap) must be 0.480 µm**  

|![DRClab3](/images/PD/DRCLab3.png) |
|:---|

- In the `sky130A.tech` file:  
  - Spacing rules for poly resistors relative to alldiffusion and nsd (nsubstratediff or N-tap) are already implemented. However, the spacing rules for poly resistors to poly are missing and need to be added. 

  | **sky130A.tech** | |
  |:---|:---|
  | ![DRClab4](/images/PD/DRCLab4.png) | ![DRClab5](/images/PD/DRCLab5.png) |

  - By examining the aliases, we can identify a definition for `allpolyres` as:  
    `allpolyres mrp1,xhrpoly,uhrpoly,rmp`  

  - To implement the missing poly.9 spacing rules, add the following lines in the appropriate sections of the tech file:  

  ```  
  spacing npres allpolynonres 480 touching_illegal \  
          "poly.resistor spacing to poly < %d (poly.9)"  

  spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \  
        "xhrpoly/uhrpoly resistor spacing to poly < %d (poly.9)"  
  ```  

  - After making these changes, reload the tech file by running the following in the console:  
    `tech load sky130A.tech`  
  - Perform a new DRC check by executing:  
    `drc check`  

**The Magic DRC engine will now highlight the poly resistor to poly spacing violations**  

|![DRClab6](/images/PD/DRCLab6.png) |
|:---|

### DRC Lab 3: Poly.mag – Poly Resistor Spacing to Diffusion and Tap

- The modifications made to address the poly.9 DRC rule are not fully comprehensive. This can be verified by duplicating the three resistor types—**npolyres, ppolyres,** and **xhrpolyres**.  

- Next, place **ndiffusion, pdiffusion, nsubstratendiff,** and **psubstratepdiff** around the duplicated poly resistors, as illustrated.  

- Additionally, draw an **nwell** beneath the **pdiffusion** and **N-tap (nsubstratendiff)** to prevent unnecessary DRC errors related to diffusion. These errors are not the focus of this task.  

- Upon inspection of the layout, DRC spacing violations are observed for all poly resistors except for **npolyres**. The **npolyres** only flags a spacing violation concerning the **N-tap**.  

- To resolve this, adjust the **npres** spacing rule to account for all diffusion layers rather than limiting it to **nsd (nsubstratediff)** alone.

| Before |
|:---|
|  <pre>spacing npres *nsd 480 touching_illegal \ <br>   "poly.resistor spacing to N-tap < %d (poly.9)"</pre> |
| ![DRClab7](/images/PD/DRCLab7.png) |
| **After** |
| <pre>spacing npres alldiff 480 touching_illegal \ <br>   "poly.resistor spacing to N-tap < %d (poly.9)"</pre> |
| ![DRClab8](/images/PD/DRCLab8.png) |

### DRC Lab 4: nwell.mag - Challenge exercise to describe DRC error as geometrical construct

- **Reference**: [SkyWater PDK Documentation – Nwell Rules](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#nwell)  
- **nwell.5**: Deep nwell must be enclosed by nwell by at least **0.400µm**.  
  *Exemptions apply within UHVI or areaid.lw. Nwells may merge over deep nwell if spacing is too small, as outlined in rule nwell.2.*  
- **nwell.6**: Minimum enclosure of nwell hole by deep nwell outside UHVI – **1.030µm**.  
- Relevant DRC rules in `sky130A.tech` file

  | ![DRClab9](/images/PD/DRCLab9.png) | ![DRClab10](/images/PD/DRCLab10.png) |
  |:---|:---|

### Key Points from sky130A.tech DRC File:  
- The DRC layers in `cifoutput` style are defined as **templayers** rather than actual physical layers.  

- **nwell_missing dnwell**:  
  - This **templayer** relies on another layer—**dnwell_shrink**—defined earlier in the tech file:

```
templayer dnwell_shrink dnwell
shrink 1030
```  

  - This command shrinks the deep nwell by **1.03µm**, representing the minimum overlap required between nwell and deep nwell internally. The result highlights the largest possible open space within the deep nwell area.  

- **nwell_missing** expands from the **dnwell** by **400nm**:

```
grow 400
```

  - This command generates the minimum required nwell area to enclose the deep nwell.  

- **and-not dnwell_shrink**:
  - This creates a ring of overlap that highlights areas where nwell must exist to meet both internal overlap and external enclosure requirements. If any portion of this area lacks nwell, a violation is flagged.  

- **and-not nwell**:  
  - Any remaining area that violates the rule is passed to the **cifmaxwidth** rule for the deep nwell layer.  

### Running DRC Checks for nwell.6
1. Position the cursor over the **nwell.6** layout.
2. Execute the following commands to visualize errors:  
    ```
    cif ostyle drc
    cif see dnwell_shrink
    feed clear
    cif see nwell_missing
    ```  
### Visual Breakdown
| **nwell.6 Layout**  | **dnwell_shrink Visualization**  |
|:---:|:---:|  
| ![DRClab11](/images/PD/DRCLab11.png)  | ![DRClab12](/images/PD/DRCLab12.png)  |  
| **nwell_missing Visualization**  |  |  
| ![DRClab13](/images/PD/DRCLab13.png)  |  |

### Important Considerations

- **Edge-Based Rules**:
  - Although **cifoutput** operators can define edge-based rules, generating layers this way can demand high computational resources.  
  - To maintain Magic’s performance, reserve complex geometric rules for separate **style variants** that can be selectively applied, preventing unnecessary slowdowns during interactive layout.  

- **DRC Rule Styles in sky130A.tech**:  
  - **drc fast**: Optimized for backend metal layers and large digital designs. Avoids lower-layer checks.  
  - **drc full**: Performs comprehensive checks across all layers. Suitable for smaller designs but may slow down large layouts.  

- **Switching DRC Modes**:

```
drc style drc(fast)
drc style drc(full)
```  

### DRC Lab 5: nwell.mag - Challenge  to find missing or incorrect rules and fix them

- **nwell.4**: All n-wells will contain metal-contacted tap (rule checks only for licon on tap) . Rule exempted from high voltage cells inside UHVI.
- Every nwell must have an n-tap layer contact inside it, which is called `nsubstratencontact` or `nsc`.
- Since there is no distance/ spacing associated with this rule, it is not possible to write this as an edge-based DRC rule. But it can be written as a cifoutput rule.
  - To the NWELL rules section, add the following cifoutput rule:

```
  variant (full)   # -> Run this rule only for drc(fast) style
  cifmaxwidth nwell_untapped 0 bend_illegal \
          "N-well missing tap (nwell.4))"
  variant *        # -> Run the rules below for all styles, unless explicitly specified
```

  - Now, in the style drc section, define the following templayers:

```
templayer nwell_tapped           # Create a templayer nwell_tapped
bloat-all nsc nwell              # that takes all nsc layers and expand it to fill any nwell underneath it
templayer nwell_untapped nwell   # Now, create a second templayer nwell_untapped that starts with allnwell geometries
and-not nwell_tapped             # and subtract all nwell_tapped geometries
```

| **untapped nwell being flagged for DRC violation. <br>**  ![DRClab14](/images/PD/DRCLab14.png) | **tapped nwell showing no DRC violation.** <br>  ![DRClab15](/images/PD/DRCLab15.png) |
|:---|:---|

</details>

<details>
<summary> Day 4: Pre-layout timing analysis and importance of good clock tree</summary>

## 1. Timing modelling using delay tables

### Lab steps to convert grid info to track info

**Objective**: Generate a LEF file for the custom standard cell `sky130_inv.mag` and integrate it into the OpenLANE flow.  

**Overview**:

- From a Place and Route (PnR) perspective, the key requirements are to define the PnR boundary of the standard cell, establish the power and ground rails, and accurately place the input and output ports. The LEF (Library Exchange Format) file encapsulates all this essential information.  

**Layout Guidelines**:

- **Port Alignment**: Input and output ports must align precisely with the intersections of vertical and horizontal tracks.
- **Cell Width**: The width of the standard cell should be an odd multiple of the track pitch.  
- **Cell Height**: The height must align with a multiple of the vertical track pitch.  

**Track Information**:  

- Tracks are essential during routing as they define the permissible paths for signal connections.  
- Details about track configurations for local interconnects and various metal layers are specified in the file:  

```
/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info
```  

![trackinfo](/images/PD/trackinfo.png)

- To ensure the ports lie on the point we open up TkCon and write the following

```
grid 0.46um 0.34um 0.23um 0.17um
```

![grid](/images/PD/grid.png)

**Port Definition and Attribute Setup**:  
Before extracting the LEF file, ports must be defined, and attributes for class and usage must be assigned.  

**Steps to Define Ports**:  

1. For each layer requiring port creation, draw a box over the desired layer. Label the box with the port name and apply a sticky label with the corresponding layer name.

  - Ensure the "Port enable" checkbox is selected while the "Default" option remains unchecked.  

| A                                                                                        |   Y       |
| --------------------------------------------------------------------------------------------| ------------- |
| ![grid](/images/PD/portA.JPG)  | ![grid](/images/PD/portY.JPG) |

| VPWR                                                                                        |   VGND        |
| --------------------------------------------------------------------------------------------| ------------- |
| ![grid](/images/PD/portVPWR.JPG)  | ![grid](/images/PD/portVGND.JPG) |

## Set port class and port use attributes for a layout

2. Assign port class and port use attributes. These attributes align with the LEF/DEF (Design Exchange Format) conventions and are critical for reading and writing macro cell pins.

  - **Port Class Options**: `default, input, output, tristate, bidirectional, inout, feedthrough, feedthru`  
  - **Port Use Options**: `default, analog, signal, digital, power, ground, clock`  
  - Assign attributes through the tkcon console window after selecting each port in the layout view:

```
# Example for ports A and Y
port class input
port use signal

# Example for power and ground ports
port class inout
port use power
```  

![grid](/images/PD/port_class_use.JPG)

3. You can delete or remove any port by first selecting the port (key s) and then executing below two commands in order (in tkcon window):

```
port remove
label erase
```

**LEF File Extraction**:  

- Once ports and attributes are correctly configured, generate the LEF file by executing the following command in the console window:  

```
lef write
```

![lefwrite](/images/PD/lefwrite.png)

</deatils>




















<details>
<summary> Day 5: Final steps for RTL2GDS using tritonRoute and openSTA </summary>

## 1.

###

</deatils>
