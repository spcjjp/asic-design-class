<details>
<summary>Lab 8 (15/10/24)</summary>
<br>
  
# Task : RTL design using Verilog with SKY130 Technology
<details>
<summary>Day-1</summary>
<br>
  
**IVerilog based Simulation flow:**

![image](https://github.com/user-attachments/assets/0e2f8052-f0f8-4cfa-bab0-fc83a490afb9)
Simulator continuously checks for changes in the input. If there is any change in input, the output is evaluated else the simulator will never evaluate the output.


 
# LAB-1:
**Aim: Cloning the required files from github repository:**

**Commands:**
```
sudo -i
sudo apt-get install git
ls
cd /home
mkdir VLSI
cd VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
ls
```

**Screenshot of the terminal window:**

![Screenshot from 2024-10-20 15-15-25](https://github.com/user-attachments/assets/a7d6d8b4-291f-4164-8cac-cdd2b4272dc4)


# LAB-2:
**Aim: Introduction to IVerilog gtkwave:**

In this lab we will implement a 2:1 multiplexer.

The .v files of 2:1 multiplexer and its testbench is already present in the 'verilog_file' folder.

We just need to put few commands as stated below in order to see the waveforms.

```c
iverilog good_mux.v tb_good_mux.v
ls
./a.out
gtkwave tb_good_mux.vcd
```
Below is the Snapshot of the above commands:
![Screenshot from 2024-10-20 15-57-13](https://github.com/user-attachments/assets/9e159fa5-2cd5-4f09-b42c-bbab4786d23c)

TO view the Testbench and Verilog file, Use this Command:
```c
apt install vim-gtk3
gvim tb_good_mux.v -o good_mux.v

```
![Screenshot from 2024-10-20 16-00-00](https://github.com/user-attachments/assets/a60b483f-0864-42d7-82e1-8d47de5b0b69)

# LAB 3: AIM : Synthesis of 2:1 Multiplexer using Yosys and Logic Synthesis.

Yosys

Synthesizer is a tool for converting the RTL to Netlist and here we are using the Yosys as the Synthesizer.

A synthesizer plays a key role in digital design by transforming RTL (Register Transfer Level) code into a gate-level netlist. This netlist provides a detailed description of the circuit, outlining the logical gates and their interconnections, and serves as the foundation for later stages like place and route. In this design flow, the synthesizer being used is Yosys, an open-source tool for Verilog HDL synthesis. Yosys applies several optimization techniques to generate an efficient gate-level implementation from the RTL code.

Block Diagram of Yosys setup :

![Screenshot from 2024-10-20 16-28-40](https://github.com/user-attachments/assets/bb3a11a3-6258-4717-8bc4-b97cbd975376)

Block Diagram of synthesis Verification:

The primary inputs and outputs remain identical in both the RTL design and the synthesized netlist. As a result, the same test bench can be applied to both.

![Screenshot from 2024-10-20 16-29-08](https://github.com/user-attachments/assets/42ed2db4-13cf-474a-9139-adba871ff48f)

Command steps for Yosys

This will invoke/start the yosys

```
 yosys
       
```
Load the sky130 standard library.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib      
```
Read the design files
```
read_verilog good_mux.v        
```

![Screenshot from 2024-10-20 16-35-19](https://github.com/user-attachments/assets/827c7f0b-59a0-4db8-8a51-8ae48b225adb)








  
</details>

