<details>
<summary>Advanced Physical Design using OpenLane using Sky130</summary>
  
  <details>
<summary> DAY 1: Introduction to open-source EDA, OpenLANE and Sky130 PDK </summary>


### Synthesis in OPENLANE
```c
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```
![Screenshot from 2024-11-12 18-03-29](https://github.com/user-attachments/assets/1d0e95a2-fb72-4542-b048-75742db9dded)

### For Netlist:

```
cd designs/picorv32a/runs/09-11_06-33/results/synthesis/
gedit picorv32a.synthesis.v
```
![Screenshot from 2024-11-12 18-45-02](https://github.com/user-attachments/assets/c3d8f2cf-78a4-496a-88e2-402fba71e741)


![Screenshot from 2024-11-12 18-43-21](https://github.com/user-attachments/assets/f459ce1f-d2a6-4ea4-85e0-2d9a3ae599f9)

### FOR YOSYS:
```c
cd ../..
cd reports/synthesis
gedit 1-yosys_4.stat.rpt
```
![Screenshot from 2024-11-12 18-58-40](https://github.com/user-attachments/assets/99fc8587-f33e-4a1e-bde0-01579e4ba476)

![Screenshot from 2024-11-12 19-00-15](https://github.com/user-attachments/assets/690992f5-6df3-466a-8b7a-7de4491964ec)

</details>

<details> 
<summary> Day-2: Good floorplan vs bad floorplan and introduction to library cells</summary>

## Floor Planning using OPENLANE:

```c
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
```
![Screenshot from 2024-11-12 19-11-30](https://github.com/user-attachments/assets/c8f89a3e-a402-45c5-8846-d94940d50095)

![Screenshot from 2024-11-12 19-12-08](https://github.com/user-attachments/assets/95ea073d-b14e-4de2-a99d-e34ed77f7ad2)

Now, run the below commands in a new terminal:
```c

cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-11_07-10/results/floorplan
gedit picorv32a.floorplan.def
```
![Screenshot from 2024-11-12 19-20-22](https://github.com/user-attachments/assets/d0a212eb-8ab1-480a-be91-df2fd7bc2e0d)

![Screenshot from 2024-11-12 19-30-34](https://github.com/user-attachments/assets/ddd92519-d776-4ada-90b2-896b1f420fd2)

### Equidistant placement of ports

![Screenshot from 2024-11-12 21-48-18](https://github.com/user-attachments/assets/f9eb5a92-de20-40d9-925d-64644ed61630)


### Decap Cells and Tap Cells

![Screenshot from 2024-11-12 20-18-52](https://github.com/user-attachments/assets/cf6da022-82d8-46a4-a663-1b830cd78c53)

### Unplaces standard cells at origin:

![Screenshot from 2024-11-12 20-20-25](https://github.com/user-attachments/assets/77150f63-263c-4e27-956c-2d17dbed945c)

### Command to run placement:
```c
run_placement
```
![Screenshot from 2024-11-12 20-26-16](https://github.com/user-attachments/assets/753988e6-c7f7-4e54-bb84-7806b070d565)


To view the placement in magic:
```c
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![Screenshot from 2024-11-12 20-29-01](https://github.com/user-attachments/assets/433596e9-b01d-4d39-88a1-3407290fa635)


![Screenshot from 2024-11-12 20-30-36](https://github.com/user-attachments/assets/878895ba-ffc6-47e1-a069-5a9460a6a2fe)

Commands to exit from current run
```c
exit # Exit from OpenLANE flow
exit # Exit from OpenLANE flow docker sub-system
```
## Cell Design and Characterization Flow
A library contains information about each cell, encompassing different sizes, functionalities, and threshold voltages. Below are the steps involved in a standard cell design flow.

### Inputs
PDKs (Process Design Kits): Includes DRC & LVS (Design Rule Checks & Layout Versus Schematic), SPICE Models, library data, and user-defined specifications.

### Design Steps

* 1.Circuit Design
* 2.Layout Design: Employ techniques such as Euler's path and stick diagrams.
* 3.Parasitic Extraction
* 4.Characterization: Evaluate timing, noise, and power.

### Outputs

* 1.CDL (Circuit Description Language)
* 2.LEF (Library Exchange Format)
* 3.GDSII (for layout)
* 4.Extracted SPICE netlist (.cir)
* 5.Timing, noise, and power .lib files

## Standard Cell Characterization Flow

The following steps are typical in standard cell characterization:

1.Load Models and Technology Files

2.Read the Extracted SPICE Netlist

3.Identify Cell Behavior

4.Load Subcircuits

5.Connect Power Sources

6.Apply Stimuli to Characterization Setup

7.Provide Necessary Output Capacitance Loads

8.Add Required Simulation Commands

These steps are compiled into a configuration file and input into a characterization tool, such as GUNA, which then generates timing, noise, and power models. These .lib files are categorized based on their characterization type: timing, power, or noise.

### Timing parameters

Timing definition	Value

![Screenshot 2024-11-12 215721](https://github.com/user-attachments/assets/a6becace-ce2b-4650-adf7-96fcfe38bf39)

### Propagation Delay: 
It refers to the time it takes for a change in an input signal to reach 50% of its final value to produce a corresponding change in the output signal to reach 50% of its final value of a digital circuit.
```c
rise delay =  time(out_fall_thr) - time(in_rise_thr)
```
### Transistion time: 
The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.
```c
Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)
Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```
</details>

<details>

  <summary>Day 3: Design library cell using Magic Layout and ngspice characterization</summary>

  ## Spice Deck:

![image](https://github.com/user-attachments/assets/f884a747-3092-4267-a168-ba6ae190f585)

  

  
</details>

</details>
