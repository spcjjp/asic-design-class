# Integration of Peripherals for Digital-to-Analog Conversion Using DAC and PLL

## Overview

This guide covers the setup and simulation of a simple System-on-Chip (SoC) environment using open-source tools and the VSDBabySoC platform. The VSDBabySoC is a compact RISCV-based SoC designed for testing three open-source IP cores: the RVMYTH microprocessor, an 8x Phase-Locked Loop (PLL), and a 10-bit Digital-to-Analog Converter (DAC). This SoC is useful for ensuring seamless interaction among these cores and for fine-tuning analog components.

### Key Components

- **RVMYTH Microprocessor**: A compact RISC-V core for embedded applications.
  
- **Phase-Locked Loop (PLL)**: 
  - **Purpose**: Synchronizes an output signal with a reference signal in terms of frequency and phase.
  - **Function**: Continuously adjusts the output frequency to match the phase of the input signal.
  - **Applications**: Commonly used in clock generation, frequency synthesis, and communication systems for stable and precise timing.

- **Digital-to-Analog Converter (DAC)**: 
  - **Purpose**: Converts digital signals into corresponding analog signals.
  - **Function**: Translates binary data into a continuous voltage or current output.
  - **Applications**: Utilized in audio systems, signal processing, and interfacing with analog devices.

## Step 1: Tool Installation

### 1.1. Installing Icarus Verilog (iverilog)
Icarus Verilog is an open-source Verilog simulation and synthesis tool.

```bash
sudo apt-get update
sudo apt-get install iverilog
```

### 1.2. Installing GTKWave
GTKWave is a wave viewer for signal simulation results.

```bash
sudo apt-get update
sudo apt install gtkwave
```

### 1.3. Installing Yosys
Yosys is an open-source synthesis tool for digital hardware designs.

```bash
sudo apt-get update
git clone https://github.com/YosysHQ/yosys.git
cd yosys
sudo apt install make  # Run only if `make` is not installed
sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make 
sudo make install
```

After installation, verify by running:

```bash
yosys
```


## Step 2: VSDBabySoC Simulation

### Overview of the Simulation Process

The VSDBabySoC simulation involves running a testbench for the RVMYTH microprocessor, configured with a PLL and DAC. This simulation will demonstrate the SoC's functionality, including the generation of a stable clock signal and the conversion of digital signals to analog outputs.

### Commands to Run the Simulation

1. **Navigate to the VSDBabySoC Directory:**
   ```bash
   cd VSDBabySoc
   ```

2. **Compile the Simulation:**
   ```bash
   iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
   ```

3. **Execute the Simulation:**
   ```bash
   ./pre_synth_sim.out
   ```

4. **View the Waveform in GTKWave:**
   ```bash
   gtkwave pre_synth_sim.vcd
   ```

### Output:
![Screenshot from 2024-09-02 20-35-40](https://github.com/user-attachments/assets/09287b56-0989-4027-b635-769e0eb29602)
![Screenshot from 2024-09-02 20-34-37](https://github.com/user-attachments/assets/8ae16513-59c0-49f1-9d29-b7e0d412df72)

