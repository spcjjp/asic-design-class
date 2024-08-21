<details>
<summary> Digital logic with TL Verilog in Makerchip </summary>

# Combinational Logic

## 1. Implemting an Inverter
The code for an Inverter

```c
 $reset = *reset;
   $clk_sat = *clk;
   $out = !$in;
```
Below is the snippet 

![Screenshot 2024-08-20 193221](https://github.com/user-attachments/assets/d7039924-1944-495f-b8f9-838a4239c731)

## 2. Implementing a XOR gate
```c
 $reset = *reset;
   $clk_sat = *clk;
   $out = $a ^ $b;
```
Below is the snippet

![Screenshot 2024-08-20 193322](https://github.com/user-attachments/assets/83ab5c98-2b7c-42ce-8af9-e53510695926)









## 3. Implementing a CALCULATOR based on Combinational Logic.

Code is given below

 ```c
 
 $reset = *reset;

   $clk_sat = *clk;

   //define smaller random numbers 4bit and assign to val1/val2
   // 31-4 bits of val1/val2 will be zero and 0-9 bits will be rand1/rand2 values
   
   $val1[31:0] = $rand1[3:0];
   $val2[31:0] = $rand2[3:0];
   $op[1:0] = $rand3[1:0];
   $sum[31:0] = $val1[31:0] + $val2[31:0];
   $diff[31:0] = $val1[31:0] - $val2[31:0];
   $prod[31:0] = $val1[31:0] * $val2[31:0];
   $quot[31:0] = $val1[31:0] / $val2[31:0];
   $out[31:0] =
         ($op == 0)
           ? $sum[31:0] :
         ($op == 1)
           ? $diff[31:0] :
         ($op == 2)
           ? $prod[31:0] :
         ($op == 3)
           ? $quot[31:0] :
         //default
           32'b0;

   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   ```
We will get the following  output after implementing this calculator code :

![combi calc](https://github.com/user-attachments/assets/408d6e29-42c9-47fc-8887-c63c6e33f67f)

# Sequential Logic:

## 1. Implementing Fibinacci Sequence

Below is the code logic:

```c
$reset = *reset;
   $clk_sat = *clk;
 $val[15:0] = $reset ? 1 : >>1$val + >>2$val;
```
The snippet is :

![Screenshot 2024-08-21 021208](https://github.com/user-attachments/assets/2f83bf80-65f1-4ddb-a6e7-3cd9558f3c3e)



## 2. Implementing a CALCULATOR based on Sequential Logic.

The code for implemting Calculator based on Sequential circuit is given beolw

```c
 |calc
      @0
         $reset = *reset;
         $clk_sat = *clk;
         
         
         $val1[31:0] = >>1$out[31:0];
         $val2[31:0] = $rand2[3:0];
         $op[1:0] = $rand3[1:0];
   
         $sum[31:0] = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
   
         $out[31:0] = $reset ? 32'b0 : (($op[1:0]==2'b00) ? $sum :
                                       ($op[1:0]==2'b01) ? $diff :
                                          ($op[1:0]==2'b10) ? $prod : $quot);

```

The snapshot of the implementation of the above code for sequential circuit in Makerchip platfrom is shown below.

We can also observe the generated block diagram as well as the waveform for the simulation of our circuit.

![Screenshot 2024-08-21 014412](https://github.com/user-attachments/assets/724f3add-f3e2-4352-8d24-f6854551adab)

# Pipelined Logic

## Lab on Calculator and Counter in Pipeline

Block Diagram

![Screenshot 2024-08-21 022513](https://github.com/user-attachments/assets/a174f9c3-9f3f-4254-a571-3e7e9e202f55)

```c

|calc
      @0
         $reset = *reset;
         $clk_sat = *clk;
         
      @1
         $val1 [31:0] = >>1$out [31:0];
         $val2 [31:0] = $rand2[3:0];
   
         $sum [31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $quot[31:0] = $val1 / $val2;
         
         $out [31:0] = ($reset ? 0 : ($op[0] ? $sum : ($op[1] ? $diff : ($op[2] ? $prod : $quot )))) ;
         
         $cnt [31:0] = $reset ? 0 : >>1$cnt + 1 ;
```
### Output

![Screenshot 2024-08-21 022727](https://github.com/user-attachments/assets/161184be-20ba-40af-b397-4eb721a09263)

## Validity

Validity is another feature in TL verilog which is asserted if a particular transactions in a pipeline is valid or true. A new scope, called “when” scope is introduced for this and it is denoted as ?$valid. This new scope has many advantages - easier design, cleaner debug, better error checking and automated clock gating. Validity provides :

-> Easier debug
-> Cleaner design
-> Better error checking
-> Automated Clock gating

## 2 Cycle Calculator with Validity

Code is as follow

```c
|calc
      @0
         $reset = *reset;
         $clk_sat = *clk;
         $count = $reset ? 0 : >>1$count + 1;
         $valid = !($count);         
         $valid_or_reset = $valid || $reset;
         
      ?$valid_or_reset
         @1
            $val1[31:0] = $reset ? 0: >>2$out[31:0];
            $val2[31:0] = $rand2[3:0];

            $sum[31:0]  = $val1[31:0] + $val2[31:0];
            $diff[31:0] = $val1[31:0] - $val2[31:0];
            $prod[31:0] = $val1[31:0] * $val2[31:0];
            $quot[31:0] = $val1[31:0] / $val2[31:0];
            $oldop[2:0] = $op[2:0];

            //$count_num[31:0] = $reset ? 0 : >>1$count[31:0];
            //$count[31:0] = $count_num[31:0] + 1;

         @2
        
            $mem[31:0] = $reset ? 0 :
                          ($oldop[2:0] == 3'b101) ? $out:
                          $mem;
                          
            $out[31:0] = //($reset | !($count) ) ? 0 :
                         ($oldop[2:0] == 3'b000) ? $sum[31:0]:
                         ($oldop[2:0] == 3'b001) ? $diff[31:0]:
                         ($oldop[2:0] == 3'b010) ? $prod[31:0]:
                         ($oldop[2:0] == 3'b011) ? $quot[31:0]:
                         ($oldop[2:0] == 3'b100) ? >>1$mem:
                         $out;
```
Waveform of 2 cycle Calculator with validity

![Screenshot 2024-08-21 023536](https://github.com/user-attachments/assets/e2b2a267-c077-4410-8230-38877f4a9c33)


Diagram

![Screenshot 2024-08-21 023700](https://github.com/user-attachments/assets/80478936-16c9-4bf3-84fd-cfcdb59bd6de)



 </details>

<details>
<summary> Basic RISC-V CPU micro-architecture </summary>

The block diagram of a basic RISC-V microarchitecture is as shown in figure below. Now, using the Makerchip platform the implementation of the RISC-V microarchitecture or core is done. For starting the implementation a starter code present in reference is used. The starter code consist of -

-> A simple RISC-V assembler.
->  An instruction memory containing the sum 1..10 test program.
-> Commented code for register file and memory.
->Visualization.

![Screenshot 2024-08-21 132041](https://github.com/user-attachments/assets/95bf4998-058e-4915-8766-91d91937b475)

## Designing of processor is based on three core steps fetch, decode and execute :

Here we gonna design RiscV Cpu Core for which block diagram is given below :

![Screenshot 2024-08-21 141047](https://github.com/user-attachments/assets/465e1523-45a0-4aee-9ae7-bd31ea856229)

## Fetch
During the fetch stage, processors fetches the instruction from the memory to the address pointed by the program counter. The program counters holds the address of the next stage, in our case it is after 4 cycle and the instruction memory holds the set of instruction to be executed. The code of the fetch stage is shown below.

```c
|cpu
      @0
         $reset = *reset;
         $clk_sat = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 : (>>1$pc + 32'd4);
         
      @1
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0]; 
   ```

Snapshot of Fetch stage in maker chip

![Screenshot 2024-08-21 141551](https://github.com/user-attachments/assets/1f0a95da-935d-4be8-93b1-18e73a44ee3a)

## Lab for RV instruction file Decode Logic
We are decoding the individual instructions using the following code 
The code of the decode stage is shown below.

```c
|cpu
      @0
         $reset = *reset;
         $clk_sat = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 : (>>1$pc + 32'd4);
         
      @1
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0]; 
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001 ||
                       $instr[6:2] ==? 5'b11100;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b0x100 ||
                       $instr[6:2] ==? 5'b01110;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? { {20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? { $instr[31:12], 12'b0} :
                      $is_j_instr ? { {12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} : 32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         
         $rs1_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         ?$rs2_valid
            $funct3[3:0] = $instr[14:12];
   
         $opcode[6:0] = $instr[6:0];
         
         $funct7_valid = $is_r_instr;
         ?$rs2_valid
            $funct7[6:0] = $instr[31:25];
            
         $dec_bits[10:0] = {$funct[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits == 11'b0_000_0110011;
```
Snapshot of Decode stage in makerchip:

![Screenshot 2024-08-21 142202](https://github.com/user-attachments/assets/f4477a3a-0a23-4a2a-8091-4c9e5000c687)

## Lab for register file read

The code of the REGISTER FILE READ operation is included below:



```c
         $rf_rd_en1 = !$rs1_valid;
         $rd_rd_en2 = !$rs2_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_index2[4:0] = $rs2;
         $rf_rd_data1[31:0] = $src1_value[31:0]; 
         $rf_rd_data2[31:0] = $src2_value[31:0];
```

The snapshot of the REGISTER FILE READ operation is included below:

![Screenshot 2024-08-21 144224](https://github.com/user-attachments/assets/ca74fe84-1574-4b87-9616-f47cd6366de1)

## Lab for instruction file Write
The code of the REGISTER FILE Write operation is included below:

```c
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx;
         
         $rf_wr_en = ($rd_valid || $rd !== 5'b0);  
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
```
The snapshot of the REGISTER FILE write operation is included below:

![Screenshot 2024-08-21 144224](https://github.com/user-attachments/assets/7484c7e1-b572-4a24-b4cf-97d3a19d0e80)

## Lab for ALU (Arithmetic aand Logic Unit)
### Block Diagram

![Screenshot 2024-08-21 141047](https://github.com/user-attachments/assets/b6203d7c-fae4-477c-943d-555ce1de1542)


### Code:

```c
//ARITHMETIC AND LOGIC UNIT (ALU)
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
```

### Snapshot from Makerchip

![Screenshot 2024-08-21 150401](https://github.com/user-attachments/assets/345b9bb8-4b35-4ef5-a352-02a444b872bb)

## Lab for implementing Branch Instructions

### Block Diagram

![Screenshot 2024-08-21 151014](https://github.com/user-attachments/assets/9bc62568-504d-4caf-879f-4f75615dbcf6)

### Various Branch Instructions

![Screenshot 2024-08-21 151030](https://github.com/user-attachments/assets/3f0136fd-a28a-4d6e-b9c2-a4a36f6362db)

### Code  

```c
//BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
```
### Snapshot from Makerchip

![Screenshot 2024-08-21 154720](https://github.com/user-attachments/assets/fc5888e4-f1f2-4c7d-b4cc-25d58c4f7429)

### Viz

![Screenshot 2024-08-21 154809](https://github.com/user-attachments/assets/dc9a379b-5bb2-4b7f-a3aa-4aefa53a78c5)

## Lab to Create Simple Testbench:

### Code

```c
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9+10) ;
```


</details>
<details>

<Summary>  Complete Pipelined RiscV CPU Micro-architecture </Summary>

## Pipelining the CPU
Now pipelining of the CPU core is done, which allows easy retiming and reduces functional bug to a great extent . Pipelining allows faster computaion. For pipelining as mentioned earlier we simply need to add @1, @2 and so on. The snapshot of the pipelining is as shown below. In TL verilog, another advantage is defining of pipeline in systematic order is not necessary.

## Lab on 3 Cycle Valid Signal

### Code
 ```c
	
$valid = $reset ? 1'b0 : ($start) ? 1'b1 : (>>3$valid) ;
         $start_int = $reset ? 1'b0 : 1'b1;
         $start = $reset ? 1'b0 : ($start_int && !>>1$start_int);
```

## Lab for generating valid signals for each instruction

### Code
```c
  //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
```


# Completing the RISCV CPU

### Block Diagram

![Screenshot 2024-08-21 160550](https://github.com/user-attachments/assets/4c91e25b-79c3-43e7-bced-b3c11913e2e6)

## Code
```c


</details>






                      



