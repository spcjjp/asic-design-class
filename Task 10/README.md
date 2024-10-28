<details>
<summary>Static Timing Analysis for a Synthesized RISC-V Core with OpenSTA </summary>
<br>

## In this task STA was performed on the RV core netlist generated with yosys

### openSTA
```c
 cd
sudo apt-get install cmake clang gcc tcl swig bison flex
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
cmake -DCUDD_DIR=/home/satya/cudd-3.0.0
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
read_liberty /home/satya/OpenSTA/lab10/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog /home/satya/OpenSTA/lab10/satya_riscv_netlist.v
link_design rvmyth

create_clock -name clk -period 9.15 [get_ports clk]
set_clock_uncertainty [expr 0.05 * 9.15] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * 9.15] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * 9.15] [get_clocks clk]
set_input_transition [expr 0.08 * 9.15] [all_inputs]

report_checks -path_delay max
report_checks -path_delay min
```

As we can see in the below snapshots that for a given clock period (9.15) the design is showing setup and hold violations.

![Screenshot from 2024-10-28 22-06-21](https://github.com/user-attachments/assets/29cb3461-c457-4926-8e12-c967b78a2a57)

![image](https://github.com/user-attachments/assets/11155a50-d3e1-4ee5-a682-68b1440e259a)


