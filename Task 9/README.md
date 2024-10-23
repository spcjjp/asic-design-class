<details>
<summary>Lab 9 (22/10/24)</summary>
<br>

# AIM : To Synthesize RISC-V and compare output with functional simulations. 

# Here are the steps to follow:

* 1.Copy the src Folder:
        Start by copying the src folder from your VSDBabySoC directory to your VLSI directory using the following commands:

```c

sudo -i
cd /home/satya/vlsi/
cp -r src sky130RTLDesignAndSynthesisWorkshop/
```
* 2.Navigate to the Target Directory:

 Change to the desired directory:
 ```c

cd ~
cd /home/satya/vlsi/sky130RTLDesignAndSynthesisWorkshop/src/module
```
* 3.Synthesis Process:

    Launch Yosys by entering:
```c
  yosys
```
* 4.Load the Library:

    Read the library file:
```c
  read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
* 5.Import Design Files:
 Read the Verilog design files:
```c

  read_verilog clk_gate.v
  read_verilog rvmyth.v
```
* 6.Synthesize the Design:
  Synthesize the design with the following command:
```
  synth -top rvmyth
```
![Screenshot from 2024-10-23 23-51-09](https://github.com/user-attachments/assets/02b950ed-af18-4d75-9c2e-32580f741e23)

### Now Generate the Netlist
```c
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr rvmyth.v
!gvim rvmyth.v
exit
```
![Screenshot from 2024-10-23 23-50-40](https://github.com/user-attachments/assets/c89d92fe-12c6-406b-b461-fe5a8ea50005)

## The Heirarchy of the Net list is shown below :

![Screenshot from 2024-10-21 15-13-35](https://github.com/user-attachments/assets/4065ac6f-af5b-4387-8829-53db7b544057)


## Now let`s Observe the output waveform of synthesised RISC-V
 ### Steps:

 ![Screenshot from 2024-10-24 01-55-56](https://github.com/user-attachments/assets/11293b34-daa3-4655-9035-82eee9f9af2d)

 ![Screenshot from 2024-10-24 01-42-15](https://github.com/user-attachments/assets/c86e9e02-700e-480f-adfc-738a35692790)

 ## Functional Simulations 
 Commands to get the waveform:
 ```c
cd ~
cd VSDBabySoC
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```
![Uploading Screenshot from 2024-10-21 01-42-25.png…]()

![Screenshot from 2024-10-24 01-26-27](https://github.com/user-attachments/assets/c2a2c43c-18eb-4c68-9d93-2c7b409315ee)

## Comparison

![Screenshot from 2024-10-24 01-26-27](https://github.com/user-attachments/assets/ab59ad65-84d3-4fa2-85b8-d1ae3b75cdd3)

## Conclusion:- From the above output we can observe a sawtooth waveform and thus we can say that the output O2 which we get from GLS is same as that of the functional simulation O1. Hence O1 = O2.

