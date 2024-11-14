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

![image](https://github.com/user-attachments/assets/c73e89ef-cffd-4ece-8c1d-dedb6a8e9183)


### 2. Load the custom inverter layout in magic and explore.
Screenshot of custom inverter layout in magic

![image](https://github.com/user-attachments/assets/78a6e65b-864c-4d8a-be75-f9a77190b888)


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

![image](https://github.com/user-attachments/assets/f2b714c9-eb89-4b75-853f-4afc0a9e710f)

New commands inserted in sky130A.tech file to update drc

![Screenshot 2024-11-14 120250](https://github.com/user-attachments/assets/04894e71-5626-4840-8c54-a214a12c744c)
![image](https://github.com/user-attachments/assets/df11b014-fca2-44cb-8150-cf03d7e5a12a)


Commands to run in tkcon window
```
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```
![Screenshot 2024-11-14 121804](https://github.com/user-attachments/assets/1b8e8590-9f49-41f1-9513-246085c1c49b)



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

![Screenshot from 2024-11-13 15-12-06](https://github.com/user-attachments/assets/7de5e763-f92b-4872-a689-c804838b3ee1)
Commands to view and change parameters to improve timing and run synthesis
```c
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
Screenshot of merged.lef in tmp directory with our custom inverter as macro

![Screenshot from 2024-11-13 15-22-46](https://github.com/user-attachments/assets/ff46f827-45a2-44d2-b4fb-ab971e913578)


Snaapshot of performed commands:
![Screenshot from 2024-11-13 14-03-30](https://github.com/user-attachments/assets/029d33b1-2518-41b0-92c0-4a39fb0e0503)

![Screenshot from 2024-11-13 14-05-18](https://github.com/user-attachments/assets/32ba72ab-b904-48b8-94d1-2dcb9035cf2b)

![Screenshot from 2024-11-13 14-16-16](https://github.com/user-attachments/assets/2cf0cffb-5d83-49f2-a997-6a3187e5bbbf)

![Screenshot from 2024-11-13 15-18-45](https://github.com/user-attachments/assets/f18cb439-5f1c-4586-b55e-564941087c09)

### 9. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command
```
# Now we can run floorplan
run_floorplan
```

Screenshots of command run
![Screenshot from 2024-11-13 14-27-28](https://github.com/user-attachments/assets/7cbf0181-c2be-464e-a1de-31178f55cd55)

Since we are facing unexpected un-explainable error while using run_floorplan command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on Floorplan Commands section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`
```c
# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

run_placement
```

Snapshot of the command run:

![Screenshot from 2024-11-13 15-25-41](https://github.com/user-attachments/assets/b1827876-125d-4d2b-9923-487c6c50b3f1)

![Screenshot from 2024-11-13 15-26-00](https://github.com/user-attachments/assets/64a70aff-05e3-402a-9d1f-66d3378a4bbd)

![Screenshot from 2024-11-13 15-26-18](https://github.com/user-attachments/assets/6f8955bb-9c6a-4e31-9d06-936b8b15820d)

![Screenshot from 2024-11-13 15-29-37](https://github.com/user-attachments/assets/2cd15893-3733-487a-8d8b-6b4c2d1fe7bb)

![Screenshot from 2024-11-13 15-30-13](https://github.com/user-attachments/assets/f66702b0-e478-436b-bc52-7d77a1b8161c)


### Commands to load placement def in magic in another terminal
```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshot of placement def in magic:

![Screenshot from 2024-11-13 15-32-50](https://github.com/user-attachments/assets/89b5aea1-972b-4b01-8cdc-81d6e7a82891)

Screenshot of custom inverter inserted in placement def with proper abutment:

![Screenshot 2024-11-14 053218](https://github.com/user-attachments/assets/4d64adee-992b-494b-986b-20de246a08b3)
![Screenshot 2024-11-14 053254](https://github.com/user-attachments/assets/60dbbd9f-a1be-4dbb-95da-0c7e4cf1d39a)


Command for tkcon window to view internal layers of cells
```
# Command to view internal connectivity layers
expand
```
Abutment of power pins with other cell from library clearly visible

![Screenshot from 2024-11-13 15-49-36](https://github.com/user-attachments/assets/abe6729d-68fe-4623-81aa-54695f5b660f)

### 9. Do Post-Synthesis timing analysis with OpenSTA tool.

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis
```c
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

```
Snap shot of the commands:

![Screenshot from 2024-11-13 15-52-55](https://github.com/user-attachments/assets/10b1bcb2-89d1-43b2-8582-4a3b99059d97)

![Screenshot from 2024-11-13 15-54-20](https://github.com/user-attachments/assets/c40a1fbd-4879-4365-a97d-d0ee8104b2ce)

Newly created pre_sta.conf for STA analysis in openlane directory

![Screenshot from 2024-11-13 16-09-01](https://github.com/user-attachments/assets/b1c0ef89-5891-492a-8a93-121613c76c5b)


Newly created `my_base.sdc` for STA analysis in `openlane/designs/picorv32a/src directory` based on the file `openlane/scripts/base.sdc`

![Screenshot from 2024-11-13 15-57-59](https://github.com/user-attachments/assets/c0991011-109d-4118-962a-b6a140738f15)


Commands to run STA in another terminal
```
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
Snapshot of the commands run
![Screenshot from 2024-11-13 16-39-30](https://github.com/user-attachments/assets/4af08701-b9fa-48d8-8f41-b43daa35ccaa)

![Screenshot from 2024-11-13 16-39-34](https://github.com/user-attachments/assets/de221a71-6ef2-4ced-b09a-cc71649ee8d6)

Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis
```c
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 25-03_18-52 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
![Screenshot from 2024-11-13 16-53-07](https://github.com/user-attachments/assets/9c9a6fd8-c463-408a-9abe-dfa03fd4906f)

Commands to run STA in another terminal
```c
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run
![Screenshot from 2024-11-13 16-54-47](https://github.com/user-attachments/assets/b06fdf43-8b83-4637-9479-621bbe59a21e)

![Screenshot from 2024-11-13 16-54-50](https://github.com/user-attachments/assets/74bb9525-e28c-434b-b0e0-0ec98dce60ad)

### 10. Make timing ECO fixes to remove all violations.

OR gate of drive strength 2 is driving 4 fanouts
![Screenshot from 2024-11-13 16-56-07](https://github.com/user-attachments/assets/f3e49e01-23fc-4bf9-b908-ade8ee60eb14)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```c
# Reports all the connections to a net
report_net -connections _11672_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-11-13 16-59-40](https://github.com/user-attachments/assets/c615f016-05f0-453c-9668-7c4a82b42450)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```c
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
Result Slck Reduced:
![Screenshot from 2024-11-13 17-02-29](https://github.com/user-attachments/assets/93cf2e3a-485a-4aae-b223-c076017ddfa1)

OR gate of drive strength 2 driving OA gate has more delay

![Screenshot from 2024-11-13 18-04-37](https://github.com/user-attachments/assets/e6e6bec7-cbc5-408b-a55d-7a7af4ad8c8d)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
Result Slack Reduced:

![Screenshot from 2024-11-13 18-06-09](https://github.com/user-attachments/assets/cca78604-6b30-4c1f-aeae-d296dcef7f7d)

OR gate of drive strength 2 driving OA gate has more delay
![Screenshot from 2024-11-13 18-07-27](https://github.com/user-attachments/assets/d6edef6e-f2c6-4f76-a57f-41829a95a643)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4
```c
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
Result Slcak Reduced:
![Screenshot from 2024-11-13 18-09-12](https://github.com/user-attachments/assets/4a0b69a5-797e-4664-bc06-0a3e5c8ebe86)

Commands to verify instance _14506_ is replaced with sky130_fd_sc_hd__or4_4

```
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```
Screenshot of replaced instance
![Screenshot from 2024-11-13 18-11-00](https://github.com/user-attachments/assets/f9635e53-3b63-476e-a923-59e3b230c7e9)

We started ECO fixes at wns -23.9000 and now we stand at wns -22.6173 we reduced around 1.2827 ns of violation

## 11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use write_verilog and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist
```c
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```
Screenshot of the command run
![Screenshot from 2024-11-13 18-14-00](https://github.com/user-attachments/assets/c7a5eb40-50e0-47f4-8648-277b03e0d6bf)

Commands to write verilog
```
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```
Screenshot of commands run
![Screenshot from 2024-11-13 18-20-04](https://github.com/user-attachments/assets/e57f4749-b935-4fbb-b2c5-b14828288649)

Verified that the netlist is overwritten by checking that instance _14506_ is replaced with sky130_fd_sc_hd__or4_4
![Screenshot from 2024-11-13 18-21-46](https://github.com/user-attachments/assets/f5afc76c-1f49-4687-8fb7-40cf536c4b8e)

Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages
```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts
```
Snapshot of the command run:

![Screenshot from 2024-11-13 18-29-06](https://github.com/user-attachments/assets/ab9618d8-a85f-4970-b4d4-c9d64ece8f15)
![Screenshot from 2024-11-13 18-29-14](https://github.com/user-attachments/assets/fe6ade63-1b2a-4f53-b3ab-94a3099ebaa0)

![Screenshot from 2024-11-13 18-30-28](https://github.com/user-attachments/assets/c84e2c7f-1068-4f67-860f-92465d9fadbc)
![Screenshot from 2024-11-13 18-30-57](https://github.com/user-attachments/assets/8779f1b5-de24-413d-b166-1e9b248a1bff)
![Screenshot from 2024-11-13 18-31-02](https://github.com/user-attachments/assets/b8def725-fbf2-472e-a972-577df4093c0e)

## 12. Post-CTS OpenROAD timing analysis.
Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD
```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
![Screenshot from 2024-11-13 18-37-34](https://github.com/user-attachments/assets/2a9a7a0f-c414-49f7-adb0-d914dd05eae8)
![Screenshot from 2024-11-13 18-37-38](https://github.com/user-attachments/assets/2a3e80da-f11f-40f1-b863-9d960f2c9e54)
![Screenshot from 2024-11-13 18-37-41](https://github.com/user-attachments/assets/4241579e-9095-4ce7-98d9-518fb70ad645)
![Screenshot from 2024-11-13 18-37-47](https://github.com/user-attachments/assets/7025f5f9-30af-4209-8e06-b29f3ec36d3f)
![Screenshot from 2024-11-13 18-37-49](https://github.com/user-attachments/assets/9f37732c-d7aa-4f56-b065-e2fdc850ea67)


## 13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.
Commands to be run in OpenLANE flow to do OpenROAD timing analysis after changing CTS_CLK_BUFFER_LIST
```
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```
Screenshots of commands run and timing report generated

![Screenshot from 2024-11-13 18-41-39](https://github.com/user-attachments/assets/9f9f2237-64d0-4385-9ad6-50c1137f5987)

![Screenshot from 2024-11-13 18-42-32](https://github.com/user-attachments/assets/4e63133b-fd95-4ab0-b8b7-0175c8a6f1e4)
![Screenshot from 2024-11-13 18-43-36](https://github.com/user-attachments/assets/c676f1b5-c622-47c3-8878-eb98de70edf0)
![Screenshot from 2024-11-13 18-43-56](https://github.com/user-attachments/assets/082512c2-b560-4125-94e6-563533b0be2e)
![Screenshot from 2024-11-13 18-44-18](https://github.com/user-attachments/assets/ed4cfc93-6f2e-4aca-987f-27c4bcfe52fa)
![Screenshot from 2024-11-13 18-44-39](https://github.com/user-attachments/assets/7493d1ad-8426-4e51-b6b7-43e1400bdf8d)




</details>

<details>
  
<summary>Day 5: Final steps for RTL2GDS using tritonRoute and openSTA </summary>

### 1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.
Commands to perform all necessary stages up until now
```
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
gen_pdn

```
Screenshots of power distribution network run

![Screenshot from 2024-11-13 19-07-19](https://github.com/user-attachments/assets/aabb3c94-e565-4b19-8a14-7285cb5ea33b)
![Screenshot from 2024-11-13 19-08-05](https://github.com/user-attachments/assets/b55a505e-cfa4-4999-b387-42f6f6c63e9f)
![Screenshot from 2024-11-13 19-09-40](https://github.com/user-attachments/assets/114b235e-3e8a-436c-8e66-02a095fc7c6d)
![Screenshot from 2024-11-13 19-10-01](https://github.com/user-attachments/assets/0b33dc8f-6a38-4a56-88cd-062feddf7ad8)
![Screenshot from 2024-11-13 19-10-20](https://github.com/user-attachments/assets/d8512bf8-f9ec-4c46-8544-c12df55613c6)
![Screenshot from 2024-11-13 19-18-51](https://github.com/user-attachments/assets/92a7d960-55c8-4f75-ba27-a22f3192d4bd)
![Screenshot from 2024-11-13 19-35-39](https://github.com/user-attachments/assets/fbcc9019-d8ef-45a6-a8c1-8c453868f52f)
![Screenshot from 2024-11-13 19-41-30](https://github.com/user-attachments/assets/3b0992d8-6b43-4339-bdf0-45dd94a5d980)

## 2. Perfrom detailed routing using TritonRoute and explore the routed layout.
Command to perform routing
```
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```
Screenshots of routing run
![Screenshot from 2024-11-13 20-01-23](https://github.com/user-attachments/assets/15d601f5-d98e-4ebe-855f-4fc88a8bdc26)
![Screenshot from 2024-11-13 20-06-38](https://github.com/user-attachments/assets/fac7ed65-82ea-4046-9b42-fb5eb5210c36)
![Screenshot from 2024-11-13 20-06-47](https://github.com/user-attachments/assets/d0e5087a-271c-446a-a5c4-e60b829cbd9a)
![Screenshot from 2024-11-13 20-16-10](https://github.com/user-attachments/assets/2fe29bd5-6f60-490b-a11c-b566768adf2e)
![Screenshot from 2024-11-13 20-16-28](https://github.com/user-attachments/assets/477eaf4b-b07d-46b7-bf40-469544ea85f3)


![Screenshot from 2024-11-13 20-17-06](https://github.com/user-attachments/assets/e920ca32-4fee-48ac-9822-bed6611866a0)

Screenshot of fast route guide present in openlane/designs/picorv32a/runs/13-11_14-21/tmp/routing directory

![Screenshot from 2024-11-13 20-51-51](https://github.com/user-attachments/assets/702fae14-1143-43f1-8db4-aef6a5babed4)

## 3. Post-Route parasitic extraction using SPEF extractor.
Commands for SPEF extraction using external tool
```c
# Change directory
cd Desktop/work/tools/SPEF_EXTRACTOR

# Command extract spef
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-21/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-21/results/routing/picorv32a.def
```

## 4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.
Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD
```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_14-21/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_14-21/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_14-21/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/13-11_14-21/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
![Screenshot 2024-11-14 232324](https://github.com/user-attachments/assets/af7765f1-b5da-4759-a5c8-c04429556ede)

![Screenshot 2024-11-14 232450](https://github.com/user-attachments/assets/803ff0f2-2d4d-4883-b290-6e9b69d03c61)

</details>
</details>
