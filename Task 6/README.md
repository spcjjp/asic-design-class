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










  
</details>
