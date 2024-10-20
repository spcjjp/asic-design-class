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

![Screenshot from 2024-10-20 15-30-09](https://github.com/user-attachments/assets/fcbfb1d8-73f3-4dc4-83a7-de81717ba795)




  
</details>

