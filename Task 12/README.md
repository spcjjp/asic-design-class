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

![image](https://github.com/user-attachments/assets/4293e213-2516-4a91-ab2f-e9835b3ee896)

### 1. Clone custom inverter standard cell design from github repository

Pate the foloowing command in terminal:

```c
cd Desktop/work/tools/openlane_working_dir/openlane
git clone https://github.com/nickson-jose/vsdstdcelldesign
cd vsdstdcelldesign
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
ls
magic -T sky130A.tech sky130_inv.mag &

```
Screenshot of the command run:

![Screenshot from 2024-11-12 22-57-47](https://github.com/user-attachments/assets/e11f613b-761d-4193-bc15-99644c6abbea)

### 2. Load the custom inverter layout in magic and explore.
Screenshot of custom inverter layout in magic

![Screenshot from 2024-11-12 22-59-53](https://github.com/user-attachments/assets/6e46f5ed-d585-40d6-bfe5-2b589c0f36d3)

NMOS and PMOS identified

![Screenshot from 2024-11-12 23-11-06](https://github.com/user-attachments/assets/6d7c9bf9-ccc1-447a-a9c1-2bcddc329e7b)

Output Y connectivity to PMOS and NMOS drain verified

![Screenshot from 2024-11-12 23-14-48](https://github.com/user-attachments/assets/0c4e21fc-1c95-49bb-a7a1-453d3415bacb)

PMOS source connectivity to VDD (here VPWR) verified

![Screenshot from 2024-11-12 23-15-32](https://github.com/user-attachments/assets/8b206dbb-0292-497b-a5b3-fc93d3086f25)

NMOS source connectivity to VSS (here VGND) verified

![Screenshot from 2024-11-12 23-16-17](https://github.com/user-attachments/assets/aed1dd7f-6245-40c9-bec6-2b8154763821)

Deleting necessary layout part to see DRC error


![image](https://github.com/user-attachments/assets/ae9a7a73-89e1-4d0b-a7dd-67ae285d79ae)

### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice

Screenshot of tkcon window after running above commands

![Screenshot from 2024-11-13 00-16-49](https://github.com/user-attachments/assets/f5a75e91-c204-4fdf-bf7e-4334cb60ec33)

Screenshot of created spice file

![Screenshot from 2024-11-13 00-20-44](https://github.com/user-attachments/assets/9b9ac2ad-40bc-40d0-a1bb-4b116dfdbdd9)

### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid
![Screenshot from 2024-11-13 00-23-48](https://github.com/user-attachments/assets/1c9cc0e8-9702-4a5f-a44f-78f31d39a231)

Final edited spice file ready for ngspice simulation

![Screenshot from 2024-11-13 00-29-54](https://github.com/user-attachments/assets/1096d186-9cc2-4570-9015-6f9a09006163)

### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```c
ngspice sky130_inv.spice
plot y vs time a
```
Screenshots of ngspice run
![Screenshot from 2024-11-13 00-33-18](https://github.com/user-attachments/assets/bd1b583f-5c2c-4c0a-9072-7ff84b155d4d)

![Screenshot from 2024-11-13 00-33-38](https://github.com/user-attachments/assets/85c5f490-6afd-49c6-afc6-490526c9f7a8)

Screenshot of generated plot

![Screenshot from 2024-11-13 00-37-03](https://github.com/user-attachments/assets/96377829-f77a-44db-8824-49279c16bf92)


* Rise Transition time Calculation

Rise transition time = time taken to output to rise from 80% - Time taken for output to rise to 20%
             20% oF output = 660mv
             80% of output = 2.64v
             

20% Screenshots

![Screenshot from 2024-11-13 01-04-12](https://github.com/user-attachments/assets/e14fef90-602d-4a9b-b9d2-75cf8fe18014)

![Screenshot from 2024-11-13 01-05-16](https://github.com/user-attachments/assets/fecd3344-94d9-461d-b44c-dc17a4ff818d)

80% ScreenShots

![Screenshot from 2024-11-13 01-07-29](https://github.com/user-attachments/assets/ae9e816d-de66-4ef4-be81-5fe8119e45bf)

![Screenshot from 2024-11-13 01-07-15](https://github.com/user-attachments/assets/b221c712-17d3-4879-93b2-9c49ab70b55f)














</details>

</details>
