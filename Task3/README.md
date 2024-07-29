
# 📜 Instructions Breakdown

The instruction set given to me was as follows:

---

## Instruction Set

### ➕ ADD r5, r4, r5
- **Type:** R
- **Opcode:** 0110011
- **rd:** r5 (00101)
- **rs1:** r4 (00100)
- **rs2:** r5 (00101)
- **func3:** 000
- **func7:** 0000000
- **Instruction:** 0000000 00101 00100 000 00101 0110011
- **32-bit Code:** 0x00A282B3

### ➖ SUB r5, r5, r4
- **Type:** R
- **Opcode:** 0110011
- **rd:** r5 (00101)
- **rs1:** r5 (00101)
- **rs2:** r4 (00100)
- **func3:** 000
- **func7:** 0100000
- **Instruction:** 0100000 00100 00101 000 00101 0110011
- **32-bit Code:** 0x40A282B3

### 🔒 AND r4, r5, r5
- **Type:** R
- **Opcode:** 0110011
- **rd:** r4 (00100)
- **rs1:** r5 (00101)
- **rs2:** r5 (00101)
- **func3:** 111
- **func7:** 0000000
- **Instruction:** 0000000 00101 00101 111 00100 0110011
- **32-bit Code:** 0x00A2F293

### 🔓 OR r8, r4, r5
- **Type:** R
- **Opcode:** 0110011
- **rd:** r8 (01000)
- **rs1:** r4 (00100)
- **rs2:** r5 (00101)
- **func3:** 110
- **func7:** 0000000
- **Instruction:** 0000000 00101 00100 110 01000 0110011
- **32-bit Code:** 0x00A282B3

### ❌ XOR r8, r5, r4
- **Type:** R
- **Opcode:** 0110011
- **rd:** r8 (01000)
- **rs1:** r5 (00101)
- **rs2:** r4 (00100)
- **func3:** 100
- **func7:** 0000000
- **Instruction:** 0000000 00100 00101 100 01000 0110011
- **32-bit Code:** 0x0042C433

### 📉 SLT r10, r2, r4
- **Type:** R
- **Opcode:** 0110011
- **rd:** r10 (01010)
- **rs1:** r2 (00010)
- **rs2:** r4 (00100)
- **func3:** 010
- **func7:** 0000000
- **Instruction:** 0000000 00100 00010 010 01010 0110011
- **32-bit Code:** 0x008282B3

### ➕ ADDI r12, r3, 5
- **Type:** I
- **Opcode:** 0010011
- **rd:** r12 (01100)
- **rs1:** r3 (00011)
- **imm[11:0]:** 5 (000000000101)
- **func3:** 000
- **Instruction:** 000000000101 00011 000 01100 0010011
- **32-bit Code:** 0x00518293

### 📝 SW r3, r1, 4
- **Type:** S
- **Opcode:** 0100011
- **rs2:** r3 (00011)
- **rs1:** r1 (00001)
- **imm[11:0]:** 4 (000000000100)
- **func3:** 010
- **Instruction:** 0000000 00011 00001 010 00000 0100011
- **32-bit Code:** 0x00410223

### 🔄 SRL r16, r11, r2
- **Type:** R
- **Opcode:** 0110011
- **rd:** r16 (10000)
- **rs1:** r11 (01011)
- **rs2:** r2 (00010)
- **func3:** 101
- **func7:** 0000000
- **Instruction:** 0000000 00010 01011 101 10000 0110011
- **32-bit Code:** 0x0025C293

### 🚫 BNE r0, r1, 20
- **Type:** B
- **Opcode:** 1100011
- **rs1:** r0 (00000)
- **rs2:** r1 (00001)
- **imm[12:1]:** 20 (000000010100)
- **func3:** 001
- **32-bit Instruction:** 0_000001_00001_00000_001_0100_0_1100011
- **32-bit Code:** 0x0140E263

### ✅ BEQ r0, r0, 15
- **Type:** B
- **Opcode:** 1100011
- **rs1:** r0 (00000)
- **rs2:** r0 (00000)
- **imm[12:1]:** 15 (000000001111)
- **func3:** 000
- **32-bit Instruction:** 0_000000_00000_00000_000_1111_0_1100011

### 📥 LW r13, r11, 2
- **Type:** I
- **Opcode:** 0000011
- **rd:** r13 (01101)
- **rs1:** r11 (01011)
- **imm[11:0]:** 2 (000000000010)
- **func3:** 010
- **Instruction:** 0000000 00010 01011 010 01101 0000011
- **32-bit Code:** 0x0025C293

### ⏪ SLL r15, r11, r2
- **Type:** R
- **Opcode:** 0110011
- **rd:** r15 (01111)
- **rs1:** r11 (01011)
- **rs2:** r2 (00010)
- **func3:** 001
- **func7:** 0000000
- **Instruction:** 0000000 00010 01011 001 01111 0110011
- **32-bit Code:** 0x0025C293

---

## 🛠️ Steps to Perform Functional Simulation of RISCV

1. Create a new directory with your name: `mkdir <your_name>`
2. Create two files using the `gedit` command: `asicver.v` and `asicver_tb.v`.
3. Copy the code from the reference GitHub repo and paste it into your Verilog and testbench files.
4. To run and simulate the Verilog code, enter the following commands:
   ```sh
   $ iverilog -o asicver asicver32i.v asicver_tb.v
   $ ./asicver
   ```
5. To see the simulation waveform in GTKWave, enter the following command:
   ```sh
   gtkwave asicver.vcd
   ```
6. The GTKWave will open, and the following window will appear:

   ![Screenshot from 2024-07-28 15-19-11](https://github.com/user-attachments/assets/bd9aac73-b1ec-4a7f-99a2-ca534e1cde68)

### ⚙️ All the instructions in the given Verilog file are hard-coded. Hard-coded means that instead of following the RISCV specifications bit pattern, the designer has hard-coded each instruction based on their own pattern.

   ![Screenshot from 2024-07-28 15-17-56](https://github.com/user-attachments/assets/0b88fb6f-c0cb-4a3e-847c-76efeb4710c3)

### 🔍 Analyzing the Output Waveform of Various Instructions Covered:

- **Instruction 1: ADD r5, r4, r5**

   ![Screenshot from 2024-07-28 15-51-55](https://github.com/user-attachments/assets/1fcd7854-9b95-485f-82c3-d3acf62e7ade)

- **Instruction 2: SUB r5, r5, r4**

   ![Screenshot from 2024-07-28 15-55-21](https://github.com/user-attachments/assets/caee5b98-f8d2-429b-bba1-764299af69e3)

- **Instruction 3: AND r4, r5, r5**

   ![and](https://github.com/user-attachments/assets/5ee015ad-7d19-4d17-9e28-761d44ac7f19)
77-4d3121745d51)

- **Instruction 4: OR r8, r4, r5**

   ![Screenshot from 2024-07-28 15-54-15](https://github.com/user-attachments/assets/f211cdb6-89f5-4d5f-b76f-1e09e06cc7c0)

- **Instruction 5: XOR r8, r5, r4**

   ![Screenshot from 2024-07-28 15-55-47](https://github.com/user-attachments/assets/9b32428c-3bdd-46e3-ae4e-a67dfe21a8ab)

- **Instruction 6: SLT r10, r2, r4**

   ![Screenshot from 2024-07-28 15-54-39](https://github.com/user-attachments/assets/cdec8860-c22c-44da-ad71-596b84860f17)

- **Instruction 7: ADDI r12, r3, 5**

   ![Screenshot from 2024-07-28 15-52-53](https://github.com/user-attachments/assets/a81bac39-5186-4ec7-a187-551fc6de4c1d)

- **Instruction 8: SW r3, r1, 4**

   ![Screenshot from 2024-07-28 18-49-41](https://github.com/user-attachments/assets/9a5f3d78-a79c-493c-aabf-7a66a344e54f)

- **Instruction 9: SRL r16, r11, r2**

   ![Screenshot from 2024-07-28 15-55-08](https://github.com/user-attachments/assets/0c5795a3-8be7-49a0-a5ec-ca6065d74dfc)

- **Instruction 10: BNE r0, r1, 20**

   ![Screenshot from 2024-07-28 15-53-12](https://github.com/user-attachments/assets/cadd4b15-bd22-4086-a15e-a356b4a042a6)

- **Instruction 11: BEQ r0, r0, 15**

   ![Screenshot from 2024-07-28 15-52-53](https://github.com/user-attachments/assets/a81bac39-5186-4ec7-a187-551fc6de4c1d)

- **Instruction 12: LW r13, r11, 2**

   ![Screenshot from 2024-07-28 15-56-05](https://github.com/user-attachments/assets/4363cc79-da98-411e-8c33-cc44e5804e6a)

- **Instruction 13: SLL r15, r11, r2**

   ![Screenshot from 2024-07-28 15-54-53](https://github.com/user-attachments/assets/624b81e3-dcee-49bf-b680-8d0a4d474992)

---

These steps and visualizations should help you effectively perform the functional simulation of RISCV instructions and analyze their output waveforms.
