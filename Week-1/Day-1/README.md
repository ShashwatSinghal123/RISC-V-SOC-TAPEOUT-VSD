#  Day 1: Introduction to Verilog RTL Design & Synthesis

Welcome to Day 1 of the RTL Workshop!
Today, you’ll begin your adventure into digital design by exploring Verilog, performing simulations with the open-source Icarus Verilog (iverilog), and understanding the fundamentals of logic synthesis using Yosys. This guide combines hands-on labs, core concepts, and detailed explanations to help you develop a strong foundation in RTL design.

---

##  Table of Contents

1. [What is a Simulator, Design, and Testbench?](#1-what-is-a-simulator-design-and-testbench)
2. [Getting Started with iverilog](#2-getting-started-with-iverilog)
3. [Lab: Simulating a 2-to-1 Multiplexer](#3-lab-simulating-a-2-to-1-multiplexer)
4. [Verilog Code Analysis](#4-verilog-code-analysis)
5. [Introduction to Yosys & Gate Libraries](#5-introduction-to-yosys--gate-libraries)
6. [Synthesis Lab with Yosys](#6-synthesis-lab-with-yosys)
7. [Summary](#7-summary)

---

## 1. What is a Simulator, Design, and Testbench?

###  Simulator

A simulator is a software application that evaluates the behavior of your digital circuit by feeding it test inputs and observing the outputs. This allows you to confirm that your design works correctly before implementing it in hardware.

###  Design

The design refers to your Verilog code that specifies the intended logic behavior of the circuit.
###  Testbench

A **testbench** is a simulation environment that applies various inputs to your design and checks if the outputs are correct.

<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Image_1.png" width="70%">
</div>

---

## 2. Getting Started with iverilog

**iverilog** is an open-source simulator for Verilog. Here’s the typical simulation flow:

<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Image_2.png" width="70%">
</div>

- Both the design and testbench are provided as input to iverilog.
- The simulator produces a `.vcd` file for waveform viewing in GTKWave.
<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Image_4.png" width="70%">
</div>
---

## 3. Lab: Simulating a 2-to-1 Multiplexer

Let’s simulate a simple **2-to-1 multiplexer** using iverilog!

###  Step 1: Clone the Workshop Repository

```shell
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files


###  Step 2: Install Required Tools

```shell
sudo apt install iverilog
sudo apt install gtkwave
```

###  Step 3: Simulate the Design

Compile the design and testbench:

```shell
iverilog good_mux.v tb_good_mux.v
```

Run the simulation:

```shell
./a.out
```

View the waveform:

```shell
gtkwave tb_good_mux.vcd
```

<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Image_6.png" width="70%">
</div>
---

## 4. Verilog Code Analysis

**The code for the multiplexer (`good_mux.v`):**

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

###  **How It Works**

- **Inputs:** `i0`, `i1` (data), `sel` (select line)
- **Output:** `y` (registered output)
- **Logic:** If `sel` is 1, `y` gets `i1`; if `sel` is 0, `y` gets `i0`.

---

## 5. Introduction to Yosys & Gate Libraries

###  What is Yosys?

**Yosys** is a powerful open-source synthesis tool for digital hardware. It takes your Verilog code and converts it into a gate-level netlist—a hardware blueprint.
<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Picture10.png" width="70%">
</div>

#### Yosys Features

- **Synthesis:** Converts HDL to a logic circuit
- **Optimization:** Improves speed or area
- **Technology Mapping:** Matches logic to actual hardware cells
- **Verification:** Checks correctness
- **Extensibility:** Supports custom flows

###  Why Do Libraries Have Different Gate "Flavors"?

A `.lib` file contains many versions of each gate (like AND, OR, NOT) with different properties:

<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Picture12.png" width="70%">
</div>
- **Performance:** Faster gates for critical paths, slower for power savings
- **Power:** Some gates use less energy
- **Area:** Smaller gates for compact chips
- **Drive Strength:** Stronger gates to drive more load
- **Signal Integrity:** Specialized gates for noise/performance
- **Mapping:** Synthesis tools pick the best flavor for your needs

---

## 6. Synthesis Lab with Yosys

Let’s synthesize the `good_mux` design using Yosys!

###  Step-by-Step Yosys Flow

1. **Start Yosys**
    ```shell
    yosys
    ```

2. **Read the liberty library**
    ```shell
    read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```
<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Picture15.png" width="70%">
</div>
3. **Read the Verilog code**
    ```shell
    read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
    ```
    <div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Picture16.png" width="70%">
</div>

4. **Synthesize the design**
    ```shell
    synth -top good_mux
    ```

5. **Technology mapping**
    ```shell
    abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

6. **Visualize the gate-level netlist**
    ```shell
    show
    ```

<div align="center">
  <img src="https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/Week-1/Day-1/Images/Picture17.png" width="70%">
</div>

---

## 7. Summary

- You learned about simulators, designs, and testbenches.
- You ran your first Verilog simulation with iverilog and visualized waveforms.
- You analyzed the 2-to-1 mux code.
- You explored Yosys and learned why gate libraries have various flavors.


---
