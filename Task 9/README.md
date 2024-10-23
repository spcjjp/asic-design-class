<details>
<summary>Lab 9 (22/10/24)</summary>
<br>

# AIM : To Synthesize RISC-V and compare output with functional simulations. 

# Here are the steps to follow:

* 1.Copy the src Folder:
        Start by copying the src folder from your VSDBabySoC directory to your VLSI directory using the following commands:

```c

sudo -i
cd /home/chandra-shekhar-jha/VLSI/
cp -r src sky130RTLDesignAndSynthesisWorkshop/
```
* 2.Navigate to the Target Directory:

 Change to the desired directory:
 ```c

cd ~
cd /home/chandra-shekhar-jha/VLSI/sky130RTLDesignAndSynthesisWorkshop/src/module
```
* 3.Synthesis Process:

    Launch Yosys by entering:
```c
  yosys
```
* 4.Load the Library:

    Read the library file:
```c
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
* 5.Import Design Files:
 Read the Verilog design files:
```c

  read_verilog clk_gate.v
  read_verilog rvmyth.v
```
* 6.Synthesize the Design:
  Synthesize the design with the following command:
```
  synth -top rvmyth
```
