<details>
  <summary> RISCV Pre synthesis simulation using iverilog and GTKwave </summary>
  <br>
  
#  AIM : Comparision of RISC-V Pre-Synthesis Simulation outputs using Iverilog GTKwave and Makerchip

The RISC-V processor was initially designed using TL-Verilog within the Makerchip IDE. To deploy it on an FPGA, the design had to be converted to Verilog, a task accomplished using the Sandpiper-SaaS compiler. Following this, pre-synthesis simulations were conducted using the GTKWave simulator.

## Simulation procedure, broken down into steps:

### Step 1: Install the Required Packages:

To install the python3, Sandpiper and gtkwave packages with the below commands
```c

sudo apt install make python python3 python3-pip git iverilog gtkwave
sudo apt-get install python3-venv
pip3 install pyyaml click sandpiper-saas
python3 -m venv .venv
source ~/.venv/bin/activate
sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
sudo chmod 666 /var/run/docker.sock

```

### Step 2: Next, clone the provided repository into the home directory.

```c
cd ~
git clone https://github.com/manili/VSDBabySoC.git

```

![Screenshot 2024-08-26 224705](https://github.com/user-attachments/assets/8e2fd0de-f89c-41b7-9e48-f4c493d2735a)

### Step 3: Replace the .tlv file in the VSDBabySoC/src/module directory with the RISC-V .tlv file that we intend to convert into Verilog.

### Step 4: Navigate to the VSDBabySoC directory.

```c

cd VSDBabySoC
```
### Step 5: Convert the RISC-V .tlv file into a .v Verilog file by executing the following command:

```c

sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```
![Screenshot 2024-08-26 231620](https://github.com/user-attachments/assets/a94a4172-7e3e-43f1-b333-f5f4cd4a8c38)

### Step 6: Generate the pre_synth_sim.vcd file by executing the following command:

```c

make pre_synth_sim

```

![Screenshot 2024-08-26 231722](https://github.com/user-attachments/assets/3de78f29-668e-454a-abd5-6c27ab2bcd50)

###  Step 7: Compile and simulate the RISC-V design using the following command:

```c
$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module

```

### Step 8: The simulation result (pre_synth_sim.vcd) will be saved in the output/pre_synth_sim directory. Now, switch to the output directory.

```c
cd output
./pre_synth_sim.out
```
![Screenshot 2024-08-26 232423](https://github.com/user-attachments/assets/98805960-7cb6-4dbb-94cb-79ccf470e86e)

## Makerchip Outaput Waveform

![Screenshot 2024-08-22 053946](https://github.com/user-attachments/assets/9b51e4f4-3e64-49a2-93ea-e281aac922b6)

## GTKWave output waveform

![Screenshot 2024-08-26 234418](https://github.com/user-attachments/assets/b89952a2-2ff0-412b-b284-54dcb28af6bf)


## Conclusion

Conclusion: The output showing the sum of integers from 1 to 9 is clearly visible as 02D in the waveforms of both Makerchip and GTKWave simulations.



</details>
