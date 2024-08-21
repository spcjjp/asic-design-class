<details>
## Digital logic with TL Verilog in Makerchip

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








                      



