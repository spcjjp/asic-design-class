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
```c
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```
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

Rise Transition Time = 2.2424-2.1819 = 0.0605 = 60.6 ps

### Fall Transition time Calculation

20% of Screenshots:

![Screenshot from 2024-11-13 01-16-52](https://github.com/user-attachments/assets/676cfc41-8fa1-4769-98eb-8093171d96ce)

80% Screenshots:

![Screenshot from 2024-11-13 01-19-24](https://github.com/user-attachments/assets/ef2ee5e3-e7d3-4edd-b09e-6f570fe41769)

                          Fall Transition Time= 4.09439 - 4.0505
                                             = 0.04389
                                             = 43.89 ps
 
 ### Rise Cell Delay Calculation:
      
  Difference in time(50% output rise) to time(50% input fall)

  50% Screenshots:

  ![Screenshot from 2024-11-13 01-26-55](https://github.com/user-attachments/assets/c3eb573d-1e64-401b-a455-8538f5ca6a53)

![Screenshot from 2024-11-13 01-28-52](https://github.com/user-attachments/assets/2dca6696-54db-47a9-a166-2da709296155)

                             Rise Cell Delay =  2.20722 - 2.1501
                                            =   0.05712
                                            = 57.12 ps

### Fall Cell dealay Claclulation:

   Difference in time(50% output fall) to time(50% input rise)

  50% Snapshot:

![Screenshot from 2024-11-13 02-28-52](https://github.com/user-attachments/assets/07c4a691-c8a4-45f9-921a-11325eb0e43f)

                        Fall Cell DElay = 4.069 - 4.05
                                        = 0.019
                                        = 19 ps

## 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections:
```
cd
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
cd drc_tests
ls -al
gvim .magicrc
magic -d XR &
```
Snapshot of the command run on terminal:

![Screenshot from 2024-11-13 02-35-04](https://github.com/user-attachments/assets/4b8d1e03-c0dd-4f46-9194-434b64c4134d)


Screenshot of .magicrc file

![Screenshot from 2024-11-13 02-35-42](https://github.com/user-attachments/assets/a3f0dd28-368c-43dc-af8c-0aaca1a70306)

![Screenshot from 2024-11-13 02-39-36](https://github.com/user-attachments/assets/97912a85-d6c0-4393-8cad-9e65944c101d)

![Screenshot from 2024-11-13 02-40-58](https://github.com/user-attachments/assets/a4a9883b-a82b-465a-995e-1f985f96bc08)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-11-13 02-50-32](https://github.com/user-attachments/assets/9bc6060e-648d-4e29-bffe-6ddf010fb319)

![Screenshot from 2024-11-13 02-53-32](https://github.com/user-attachments/assets/479ce1c0-055a-4777-8e82-c543e82dfa0b)

Screenshot of magic window with rule implemented

![Screenshot 2024-11-13 025622](https://github.com/user-attachments/assets/4fda1a24-b2c1-42ea-b4ae-7cf7b42db5e2)

</details>

<details>
<summary>Day 4:  Pre-layout timing analysis and importance of good clock tree </summary>

## 1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.

Conditions to be verified before moving forward with custom designed cell layout:

Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.
Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.

Commands to open the custom inverter layout
```c
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T sky130A.tech sky130_inv.mag &
  ```
![Screenshot from 2024-11-13 03-57-22](https://github.com/user-attachments/assets/658b900e-1199-4f70-b048-647e25e7300c)

Commands for tkcon window to set grid as tracks of locali layer
```c
help grid
grid 0.46um 0.34um 0.23um 0.17um
```
Snapshot of tht command run
![Screenshot from 2024-11-13 04-01-34](https://github.com/user-attachments/assets/5a2ad6e7-f349-4d12-9d75-1d864575d368)

Condition 1 verified

![Screenshot from 2024-11-13 04-22-31](https://github.com/user-attachments/assets/a08f7133-b7a9-4426-94a1-a028f5c45b68)

Condition 2 Verified

![image](https://github.com/user-attachments/assets/b0086db0-2806-4a06-8204-42aec555c7c6)


Condition 3 Cerified

### 2. Save the finalized layout with custom name and open it.
Command for tkcon window to save the layout with custom name
```c
# Command to save as
save sky130_satyainv.mag
```
![Screenshot from 2024-11-13 04-46-53](https://github.com/user-attachments/assets/6c1e535c-19da-4d31-ad9a-81caeffd44f6)

Command to open the newly saved layout

```
# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_satyainv.mag &
```
![Screenshot from 2024-11-13 04-50-49](https://github.com/user-attachments/assets/f1fc5a1c-cc1c-4b55-8f88-6366683ef61e)

Now, type the following command in tkcon window:
```
lef write
```
![Screenshot from 2024-11-13 04-51-58](https://github.com/user-attachments/assets/d0162d55-380b-4d5d-a628-b22b852ae6da)

![Screenshot from 2024-11-13 04-54-00](https://github.com/user-attachments/assets/b5f33119-f51b-4e2a-9f9f-f3c1c37214f6)

Snapshot of newly created lef File

![Screenshot from 2024-11-13 04-55-06](https://github.com/user-attachments/assets/48d81501-5a62-4e0e-9f14-109c2fa83275)

## 4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory
```
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
Snapshot of the command run

![Screenshot from 2024-11-13 05-01-24](https://github.com/user-attachments/assets/6b7c1070-40ba-4494-b0cd-ba31f34fb582)

### 5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow
```c
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

Edited config.tcl to include the added lef and change library to ones we added in src directory

![Screenshot from 2024-11-13 12-46-08](https://github.com/user-attachments/assets/497f002d-b2e3-4679-9367-ed8e10567009)

### 6. Run openlane flow synthesis with newly inserted custom inverter cell.

Commands to invoke the OpenLANE flow include new lef and perform synthesis
```c
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshots of commands run

![Screenshot from 2024-11-13 12-55-08](https://github.com/user-attachments/assets/580ce004-e135-45f4-b097-d2c66d92c862)

![Screenshot from 2024-11-13 12-57-58](https://github.com/user-attachments/assets/76f699b0-2759-4d9f-b089-e778d14e1f49)


### 7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

Noting down current design values generated before modifying parameters to improve timing

![Screenshot from 2024-11-13 12-59-11](https://github.com/user-attachments/assets/835f3bd3-bf8c-46a2-a207-1afd97e0938a)

![image](https://github.com/user-attachments/assets/010680a6-9978-4753-8389-c2f55641c892)







</details>

</details>
