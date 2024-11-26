# OpenRoad Physical Design:

## Cloning and Installing Dependencies

To set up the environment, use the setup.sh script to install all necessary dependencies, including those required for OpenROAD.
```c
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
```
Verifying the Installation:

After setting up the environment, the binaries will be available in your system's path.
```c
source ./env.sh
yosys -help
openroad -help
cd flow
make
```
### To view the final layout in the OpenROAD GUI, use the following command:

```
make gui_final
```

![Screenshot from 2024-11-25 17-54-10](https://github.com/user-attachments/assets/6d8a5092-2a29-4097-b5aa-1988ad12a3fe)

![Screenshot from 2024-11-25 17-50-57](https://github.com/user-attachments/assets/f83f35b4-9abf-4f75-bc08-0105eed5f1cc)


![image](https://github.com/user-attachments/assets/ba96d734-8407-4711-8f1f-94fe0e74ab49)

![image](https://github.com/user-attachments/assets/451ef896-e955-4ad6-99bc-805ab690b3f9)

![image](https://github.com/user-attachments/assets/ff8b6223-d292-4eca-a9ea-0127b764719a)

![image](https://github.com/user-attachments/assets/f68440b4-8909-4e26-8da1-ff991caecbb4)

![image](https://github.com/user-attachments/assets/b8d239f1-1dbd-42e1-9ec1-30878b0c6b06)

![image](https://github.com/user-attachments/assets/85e18aaa-f39e-4fe4-b91b-bf6d583a70e6)

![image](https://github.com/user-attachments/assets/85b28b45-db4f-4e93-86b5-2a5c1ac7e293)




### ORFS Directory Structure and File formats

![Screenshot from 2024-11-25 18-26-50](https://github.com/user-attachments/assets/ecf21ef9-99ee-4ca4-a591-25270d71ca88)

### Automated RTL-to-GDS Flow for VSDBabySoC

Follow these steps to set up the VSDBabySoC design in the OpenROAD-flow-scripts environment:

1. **Create the Directory**:  
   Navigate to `OpenROAD-flow-scripts/flow/designs/sky130hd` and create a new folder named `vsdbabysoc`.

2. **Copy Required Files**:  
   Transfer the following folders and their respective contents from the VSDBabySoC directory on the system to the newly created `vsdbabysoc` directory:  
   - **gds**: Includes `avsddac.gds` and `avsdpll.gds`.  
   - **include**: Contains `sandpiper.vh`, `sandpiper_gen.vh`, `sp_default.vh`, and `sp_verilog.vh`.  
   - **lef**: Includes `avsddac.lef` and `avsdpll.lef`.  
   - **lib**: Contains `avsddac.lib` and `avsdpll.lib`.

3. **Add Constraints File**:  
   Copy `vsdbabysoc_synthesis.sdc` to the `vsdbabysoc` directory.

4. **Include Additional Configuration Files**:  
   Transfer `macro.cfg` and `pin_order.cfg` from the VSDBabySoC folder to the same directory.

5. **Prepare Macro Configuration**:  
   Create a `macro.cfg` file in the `vsdbabysoc` directory with the required configuration details.

By following these steps, we can set up the VSDBabySoC design for RTL-to-GDS implementation. 

![image](https://github.com/user-attachments/assets/27aa576b-9802-4584-88fe-6a6bca888011)

![image](https://github.com/user-attachments/assets/bedc34cc-0a56-4ea6-8c45-b830d8951133)

### Synthesis Netlists:

![image](https://github.com/user-attachments/assets/188af5d8-c3a2-48e3-b41b-3109b89021be)

### Synthesis log:

![image](https://github.com/user-attachments/assets/cb14a5c9-5b84-4059-a209-f74dd2431b8c)

### Synthesis Check:

![image](https://github.com/user-attachments/assets/9246bdf5-5714-4ecc-993c-250924054efc)

### Synthesis Stats:

![image](https://github.com/user-attachments/assets/08f47ec3-1687-4815-86d2-7490a8ec746d)

![image](https://github.com/user-attachments/assets/3d79edc9-28a0-4384-9067-902a296be820)

### Synthesis TCL:

![image](https://github.com/user-attachments/assets/081acf39-cf99-4203-ae24-5859037d2671)




### Floorplan

![image](https://github.com/user-attachments/assets/db38e81e-7e59-4b41-9df4-32c56b7fdab7)

![image](https://github.com/user-attachments/assets/5ef50fb8-e810-47af-bfa9-5da56a247d60)

![image](https://github.com/user-attachments/assets/5e3ef63a-df7b-4f2d-8562-2f9213b45220)

![image](https://github.com/user-attachments/assets/d48a2e79-e57a-4af0-afee-5b6815c411be)

![Screenshot from 2024-11-26 04-20-57](https://github.com/user-attachments/assets/4f1c8981-8630-4d37-87ff-de4df521c000)

### Floorplan log:
![image](https://github.com/user-attachments/assets/bf27aadb-15b6-4ac0-b341-269f807bce03)

### Floorplan Timing Report:

![image](https://github.com/user-attachments/assets/21c97191-5d23-4d3d-bdcd-231fc97ede71)

![image](https://github.com/user-attachments/assets/c70146bf-fffd-4f8a-9414-edc6d4ab1ac1)



```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```
![Screenshot from 2024-11-25 22-45-58](https://github.com/user-attachments/assets/9af333c4-64ae-44f2-939b-73383884f3e5)

![image](https://github.com/user-attachments/assets/c58ead90-0743-4110-9d98-c52f6e59cd60)

```
make gui_place
```
![image](https://github.com/user-attachments/assets/d1155d84-810f-42da-b641-fcaa6f0f6305)

![image](https://github.com/user-attachments/assets/06fbfa1f-96f0-4667-8fd8-8b26fba5af43)

![image](https://github.com/user-attachments/assets/70e5f072-2530-4ccf-82ba-766a5fdeed19)


```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

![image](https://github.com/user-attachments/assets/4931ae1b-d426-40c2-8b0e-4cf85e591068)

```
make gui_cts
```
![image](https://github.com/user-attachments/assets/501fd827-ec2d-47db-a1bf-4d412505d2fb)

![image](https://github.com/user-attachments/assets/0ae9e161-4c5b-4363-a87d-6eab7e223c4d)

### Heatmap:

![image](https://github.com/user-attachments/assets/97a9720a-b521-4a72-bcea-f434d83f936c)

![image](https://github.com/user-attachments/assets/00da3f14-693d-409c-b2d1-ffffc0ef4106)

### Estimated Congestion

![image](https://github.com/user-attachments/assets/ac1f945c-c86f-4f95-b536-9c355e962ca7)

### IR Drop:

![image](https://github.com/user-attachments/assets/83e0537f-61f1-4512-8dd1-0e7221ad4bbd)

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

![image](https://github.com/user-attachments/assets/3c960e9f-b4c5-4f0d-833a-3c474b498452)

![image](https://github.com/user-attachments/assets/6629e33b-8c30-463f-99d1-da6f616b8800)

![image](https://github.com/user-attachments/assets/1bc8fe3f-efa5-4a99-a1d3-de6da4afbe7a)



We also have the floorplan, place and the pdn files as shown below

![image](https://github.com/user-attachments/assets/47191b17-b4f4-4a46-9013-95fab90f79a0)

![image](https://github.com/user-attachments/assets/bc1a7e59-c423-4d51-bf7a-fe6c68387c53)

### The detailed routing file is as follows:-
![image](https://github.com/user-attachments/assets/fd4b325e-1778-41af-8b67-e21ce3df8644)

### Merge File:

![image](https://github.com/user-attachments/assets/2e30f426-0795-4bdb-84fc-abf0316143a0)

### The final report.log file is as follows:-

![image](https://github.com/user-attachments/assets/d150be88-b19f-4f99-827d-36cb8ee94d98)

### The synthesis statistics is as follows:

![image](https://github.com/user-attachments/assets/8893e179-bb1d-4794-889f-eaab3cbe06a3)
