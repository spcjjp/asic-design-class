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

![image](https://github.com/user-attachments/assets/0acb2587-0b0d-4d47-8b57-8a3103ce12f7)




### Floorplan

![image](https://github.com/user-attachments/assets/7fdeba4b-8517-4771-b91a-a983e2ffb86a)

![image](https://github.com/user-attachments/assets/9c218340-64ee-4064-bbe2-6dab082f8753)

![image](https://github.com/user-attachments/assets/99982f9f-fd2f-47dd-972a-190fa9379187)

![image](https://github.com/user-attachments/assets/45c3e803-d34b-4217-96ec-c790d7a22378)





![image](https://github.com/user-attachments/assets/081acf39-cf99-4203-ae24-5859037d2671)
