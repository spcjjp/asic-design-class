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


</details>
