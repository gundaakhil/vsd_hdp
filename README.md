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
