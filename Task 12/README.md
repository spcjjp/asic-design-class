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

![Screenshot from 2024-11-12 18-45-02](https://github.com/user-attachments/assets/c3d8f2cf-78a4-496a-88e2-402fba71e741)


![Screenshot from 2024-11-12 18-43-21](https://github.com/user-attachments/assets/f459ce1f-d2a6-4ea4-85e0-2d9a3ae599f9)


![Screenshot from 2024-11-12 18-58-40](https://github.com/user-attachments/assets/99fc8587-f33e-4a1e-bde0-01579e4ba476)

![Screenshot from 2024-11-12 19-00-15](https://github.com/user-attachments/assets/690992f5-6df3-466a-8b7a-7de4491964ec)

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

