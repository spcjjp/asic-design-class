# PVT Corner Analysis for Synthesized VSDBabySoC using OpenSTA

The PVT corner refers to the combination of Process, Voltage, and Temperature variations that a semiconductor chip might encounter during its operation. These variations can affect the performance, power consumption, and reliability of the chip, so they are simulated to ensure the chip functions correctly under different conditions The below tcl script sta_pvt.tcl can be run to performt the STA across the PVT corners for which the sky130 lib files are available:

## The below tcl script sta_pvt.tcl can be run to performt the STA across the PVT corners for which the sky130 lib files are available:

```c
set list_of_lib_files(1) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v76.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__tt_100C_1v80.lib"
```
```c
for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty /home/tushar-katole/VSDBabySoC/src/timing_libs/$list_of_lib_files($i)
read_verilog /home/tushar-katole/VSDBabySoC/src/module/vsdbabysoc.synth.v
link_design rvmyth
read_sdc /home/tushar-katole/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /home/tushar-katole/VSDBabySoC/src/sta_output/min_max_$list_of_lib_files($i).txt

exec echo "$list_of_lib_files($i)" >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_worst_max_slack.txt
report_worst_slack -max -digits {4} >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_worst_max_slack.txt

exec echo "$list_of_lib_files($i)" >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_worst_min_slack.txt
report_worst_slack -min -digits {4} >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_worst_min_slack.txt

exec echo "$list_of_lib_files($i)" >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_tns.txt
report_tns -digits {4} >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_tns.txt

exec echo "$list_of_lib_files($i)" >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_wns.txt
report_wns -digits {4} >> /home/tushar-katole/VSDBabySoC/src/sta_output/sta_wns.txt

}
```

### The SDC file used for generating clock and data constraints is given below:
```c
create_clock -name CLK -period 10 [get_ports CLK]
set_clock_uncertainty [expr 0.05 * 10] -setup [get_clocks CLK]
set_clock_uncertainty [expr 0.08 * 10] -hold [get_clocks CLK]
set_clock_transition [expr 0.05 * 10] [get_clocks CLK]
set_input_transition [expr 0.08 * 10] [all_inputs]

set_input_transition [expr $PERIOD * 0.08] [get_ports ENB_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENB_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

Run below commands on terminal to source the sta_pvt.tcl file

![image](https://github.com/user-attachments/assets/cde08c1d-084f-484b-a41a-5381431d2fd7)


![Screenshot from 2024-11-05 01-19-05](https://github.com/user-attachments/assets/af34b9b1-702f-4160-b7be-8b1b8d8f54d8)

![image](https://github.com/user-attachments/assets/b6d64b49-5b14-480b-83e5-753c7bba0995)




