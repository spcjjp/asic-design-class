# Instructions Breakdown:

 The instruction set givrn to me was as follows:


## ADD r5, r4, r5
Type: R

Opcode for ADD = 0110011

rd = r5 = 00101

rs1 = r4 = 00100

rs2 = r5 = 00101

func3 = 000

func7 = 0000000

Instruction: 0000000 00101 00100 000 00101 0110011

32-bit Code: 0x00A282B3

## SUB r5, r5, r4

Type: R

Opcode for SUB = 0110011

rd = r5 = 00101

rs1 = r5 = 00101

rs2 = r4 = 00100

func3 = 000

func7 = 0100000

Instruction: 0100000 00100 00101 000 00101 0110011

32-bit Code: 0x40A282B3

## AND r4, r5, r5

Type: R

Opcode for AND = 0110011

rd = r4 = 00100

rs1 = r5 = 00101

rs2 = r5 = 00101

func3 = 111

func7 = 0000000

Instruction: 0000000 00101 00101 111 00100 0110011

32-bit Code: 0x00A2F293

## OR r8, r4, r5

Type: R

Opcode for OR = 0110011

rd = r8 = 01000

rs1 = r4 = 00100

rs2 = r5 = 00101

func3 = 110

func7 = 0000000

Instruction: 0000000 00101 00100 110 01000 0110011

32-bit Code: 0x00A282B3

## XOR r8, r5, r4

Type: R

Opcode for XOR = 0110011

rd = r8 = 01000

rs1 = r5 = 00101

rs2 = r4 = 00100

func3 = 100

func7 = 0000000

Instruction: 0000000 00100 00101 100 01000 0110011

32-bit Code: 0x0042C433

## SLT r10, r2, r4

Type: R

Opcode for SLT = 0110011

rd = r10 = 01010

rs1 = r2 = 00010

rs2 = r4 = 00100

func3 = 010

func7 = 0000000

Instruction: 0000000 00100 00010 010 01010 0110011

32-bit Code: 0x008282B3

## ADDI r12, r3, 5

Type: I

Opcode for ADDI = 0010011

rd = r12 = 01100

rs1 = r3 = 00011

imm[11:0] = 5 = 000000000101

func3 = 000

Instruction: 000000000101 00011 000 01100 0010011

32-bit Code: 0x00518293

## SW r3, r1, 4

Type: S

Opcode for SW = 0100011

rs2 = r3 = 00011

rs1 = r1 = 00001

imm[11:0] = 4 = 000000000100

func3 = 010

Instruction: 0000000 00011 00001 010 00000 0100011

32-bit Code: 0x00410223

## SRL r16, r11, r2

Type: R

Opcode for SRL = 0110011

rd = r16 = 10000

rs1 = r11 = 01011

rs2 = r2 = 00010

func3 = 101

func7 = 0000000

Instruction: 0000000 00010 01011 101 10000 0110011

32-bit Code: 0x0025C293

## BNE r0, r1, 20

Type: B

Opcode for BNE = 1100011

rs1 = r0 = 00000

rs2 = r1 = 00001

imm[12:1] = 20 = 000000010100

func3 = 001

32 bits instruction : 0_000001_00001_00000_001_0100_0_1100011

32-bit Code: 0x0140E263

## BEQ r0, r0, 15

Type: B

Opcode for BEQ = 1100011

rs1 = r0 = 00000

rs2 = r0 = 00000

Imm[12:1] = 000000001111

func3 = 000

32 bits instruction : 0_000000_00000_00000_000_1111_0_1100011

## LW r13, r11, 2

Type: I

Opcode for LW = 0000011

rd = r13 = 01101

rs1 = r11 = 01011

imm[11:0] = 000000000010

func3 = 010

Instruction: 0000000 00010 01011 010 01101 0000011

32-bit Code: 0x0025C293

## SLL r15, r11, r2

Type: R

Opcode for SLL = 0110011

rd = r15 = 01111

rs1 = r11 = 01011

rs2 = r2 = 00010

func3 = 001

func7 = 0000000

Instruction: 0000000 00010 01011 001 01111 0110011

32-bit Code: 0x0025C293
