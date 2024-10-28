<details>
<summary>Static Timing Analysis for a Synthesized RISC-V Core with OpenSTA </summary>
<br>

### openSTA
```c
 cd
sudo apt-get install cmake clang gcc tcl swig bison flex
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
cmake -DCUDD_DIR=/home/likith/cudd-3.0.0
make
app/sta
```
![image](https://github.com/user-attachments/assets/143fbbec-b471-4467-b925-608e87655e20)

## Here are the steps for performing Timing Analysis:

   Clock Period: 9.45 ns
   Setup Uncertainty and Clock Transition: 5% of the clock period
   Hold Uncertainty and Data Transition: 8% of the clock period
```c
 cd /home/satya/OpenSTA/app
./sta
```
```c
read_liberty /home/likith/OpenSTA/lab10/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog /home/likith/OpenSTA/lab10/likith_riscv_netlist.v
link_design rvmyth

create_clock -name clk -period 9.45 [get_ports clk]
set_clock_uncertainty [expr 0.05 * 9.45] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * 9.45] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * 9.45] [get_clocks clk]
set_input_transition [expr 0.08 * 9.45] [all_inputs]

report_checks -path_delay max
report_checks -path_delay min
```
![Screenshot from 2024-10-28 22-06-21](https://github.com/user-attachments/assets/29cb3461-c457-4926-8e12-c967b78a2a57)

![Screenshot from 2024-10-28 22-06-21](https://github.com/user-attachments/assets/0342b7b9-3f69-4437-b3f0-da849b907108)

