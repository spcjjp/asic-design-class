<details>
<summary>Lab 8 (15/10/24)</summary>
<br>
  
# Task : RTL design using Verilog with SKY130 Technology
<details>
<summary>Day-1</summary>
<br>
  
**IVerilog based Simulation flow:**

![image](https://github.com/user-attachments/assets/0e2f8052-f0f8-4cfa-bab0-fc83a490afb9)
Simulator continuously checks for changes in the input. If there is any change in input, the output is evaluated else the simulator will never evaluate the output.


 
# LAB-1:
**Aim: Cloning the required files from github repository:**

**Commands:**
```
sudo -i
sudo apt-get install git
ls
cd /home
mkdir VLSI
cd VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
ls
```

**Screenshot of the terminal window:**

![Screenshot from 2024-10-20 15-15-25](https://github.com/user-attachments/assets/a7d6d8b4-291f-4164-8cac-cdd2b4272dc4)


# LAB-2:
**Aim: Introduction to IVerilog gtkwave:**

In this lab we will implement a 2:1 multiplexer.

The .v files of 2:1 multiplexer and its testbench is already present in the 'verilog_file' folder.

We just need to put few commands as stated below in order to see the waveforms.

```c
iverilog good_mux.v tb_good_mux.v
ls
./a.out
gtkwave tb_good_mux.vcd
```
Below is the Snapshot of the above commands:
![Screenshot from 2024-10-20 15-57-13](https://github.com/user-attachments/assets/9e159fa5-2cd5-4f09-b42c-bbab4786d23c)

TO view the Testbench and Verilog file, Use this Command:
```c
apt install vim-gtk3
gvim tb_good_mux.v -o good_mux.v

```
![Screenshot from 2024-10-20 16-00-00](https://github.com/user-attachments/assets/a60b483f-0864-42d7-82e1-8d47de5b0b69)

# LAB 3: AIM : Synthesis of 2:1 Multiplexer using Yosys and Logic Synthesis.

Yosys

Synthesizer is a tool for converting the RTL to Netlist and here we are using the Yosys as the Synthesizer.

A synthesizer plays a key role in digital design by transforming RTL (Register Transfer Level) code into a gate-level netlist. This netlist provides a detailed description of the circuit, outlining the logical gates and their interconnections, and serves as the foundation for later stages like place and route. In this design flow, the synthesizer being used is Yosys, an open-source tool for Verilog HDL synthesis. Yosys applies several optimization techniques to generate an efficient gate-level implementation from the RTL code.

Block Diagram of Yosys setup :

![Screenshot from 2024-10-20 16-28-40](https://github.com/user-attachments/assets/bb3a11a3-6258-4717-8bc4-b97cbd975376)

Block Diagram of synthesis Verification:

The primary inputs and outputs remain identical in both the RTL design and the synthesized netlist. As a result, the same test bench can be applied to both.

![Screenshot from 2024-10-20 16-29-08](https://github.com/user-attachments/assets/42ed2db4-13cf-474a-9139-adba871ff48f)

## Logic Systhesis

RTL Design: The design is described using a behavioral representation in Hardware Description Language (HDL) based on the required specifications.

Synthesis: The RTL (Register Transfer Level) code is translated into a gate-level representation. This process converts the design into gates and interconnections, resulting in a file known as the netlist.

Command steps for Yosys

This will invoke/start the yosys

```
 yosys
       
```
Load the sky130 standard library.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib      
```
Read the design files
```
read_verilog good_mux.v        
```

![Screenshot from 2024-10-20 16-35-19](https://github.com/user-attachments/assets/827c7f0b-59a0-4db8-8a51-8ae48b225adb)


Synthesize the top level module

```

synth -top good_mux     

```
![Screenshot from 2024-10-20 16-39-43](https://github.com/user-attachments/assets/d742ce1d-08dc-4831-943c-29c584e16820)

Map to the standard library

```

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-20 16-41-52](https://github.com/user-attachments/assets/b5e17c7b-f1a3-492e-a9b4-4a065d5721c0)


In order to see graphical version of the logic it has realized just type :

```

show
```
![Screenshot from 2024-10-21 01-42-25](https://github.com/user-attachments/assets/7b6ac318-94a2-4a23-9ab3-a7f3d7d39727)


## To save the netlist, use the write_verilog command. This will generate the netlist file in the current directory:
```c
write_verilog -noattr good_mux_netlist.v
!gvim good_mux_netlist.v

```
![asic](https://github.com/user-attachments/assets/95d68184-b83f-44a4-8332-b1040fdc4773)

</details>


<details>
<summary>Day-2</summary>
<br>

## LAB 4 - AIM : Introduction and Walkthrough to ' dot lib '.

'.lib' is like a collection of standard cells. It contains slow cells, fast cells and many more things. In order to view the '.lib' files, Enter the following command :
```c
sudo -i
cd /home/satya/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib
gvim sky130_fd_sc_hd__tt_025C_1v80.lib

```
Press Shift + : syn off
![image](https://github.com/user-attachments/assets/2ab0f842-bdc0-4cee-b86e-ff62b0243807)


Standard Cell Library Information

Technology Specifications



Process: 130nm CMOS technology
Delay model: Table lookup


Units and Naming Conventions



* ->Time: 1 ns
* ->Voltage: 1 V
* ->Leakage Power: 1 nW
* ->Current: 1 mA
* ->Pulling Resistance: 1 kΩ
* ->Capacitive Load: 1.0 pF
* ->Bus naming style: "%s[%d]"


Cell Characteristics


For each cell in the library, the following information is typically provided:

* ->Leakage power
* ->Power consumption
* ->Area
* ->Input capacitance
* ->Delay for different input combinations

  Considering a two input AND gate:
  ![image](https://github.com/user-attachments/assets/92c784cf-ace9-4751-813a-d3a7776dcdd3)

  ## LAB : 5  Hierarchical vs flat synthesis & Various Flop Coding Styles and optimization:
  # Hierarchical Synthesis:
```c
cd~
cd /home/satya/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v

```
![image](https://github.com/user-attachments/assets/1fa6db1a-f16f-4023-a674-884215a291d4)

### To Synthesize the Design:
```c 
synth -top multiple_modules
```
When we run this Command we get the following:

![image](https://github.com/user-attachments/assets/341bca6f-5e83-4310-9fdc-8779b6202c4a)

### Multiple Modules: - 2 SubModules

Commands to generate the netlist & Create a Graphical Representation of Logic for Multiple Modules:
```c 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
```
![image](https://github.com/user-attachments/assets/acb3060f-6436-429e-a574-ee62a24a895e)

* Writing the netlist and then viewing *
```c 
write_verilog -noattr multiple_modules_hier.v
!vim multiple_modules_hier.v
```
NETLIST file

![image](https://github.com/user-attachments/assets/0dfa5872-f7ae-4a44-809d-527a3ee156e0)

* Use of Flattening: Merges all hierarchical modules in the design into a single module to create a flat netlist. for this just type
```c
flatten

```
Writing the netlist and then viewing
```c

write_verilog -noattr multiple_modules_hier.v
!vim multiple_modules_hier.v
```
![image](https://github.com/user-attachments/assets/c71d8145-c139-4f1f-8e9b-de073b1ab206)


NETLIST file
![Screenshot from 2024-10-21 13-30-36](https://github.com/user-attachments/assets/c64ad262-4cc3-4178-b8c9-9a439843af7c)

Now let's Create a Graphical Representation of Logic for Multiple Modules:

```c
show
```
![Screenshot from 2024-10-21 13-36-52](https://github.com/user-attachments/assets/bc888834-3135-41e4-b6d0-dfb0a052426c)

## Design and Simulation of D Flip-Flops Using Icarus Verilog, GTKWave, and Yosys

This project showcases different coding approaches for D Flip-Flops and includes simulations using Icarus Verilog and GTKWave. Additionally, it explores the synthesis of these designs using Yosys. The simulations cover three varieties of D Flip-Flops:

    * D Flip-Flop with Asynchronous Reset
    * D Flip-Flop with Asynchronous Set
    * D Flip-Flop with Synchronous Reset

## 1. D Flip-Flop with Asynchronous Reset:

Verilog code for the D Flip-Flop with an asynchronous reset:
```c 
module dff_asyncres(input clk, input async_reset, input d, output reg q);
	always@(posedge clk, posedge async_reset)
	begin
		if(async_reset)
			q <= 1'b0;
		else
			q <= d;
	end
endmodule
```
Testbench for Asynchronous Reset D Flip-Flop:
```
module tb_dff_asyncres; 
	reg clk, async_reset, d;
	wire q;
	dff_asyncres uut (.clk(clk), .async_reset(async_reset), .d(d), .q(q));

	initial begin
		$dumpfile("tb_dff_asyncres.vcd");
		$dumpvars(0, tb_dff_asyncres);
		clk = 0;
		async_reset = 1;
		d = 0;
		#3000 $finish;
	end
	
	always #10 clk = ~clk;
	always #23 d = ~d;
	always #547 async_reset = ~async_reset; 
endmodule

```
* Steps to Run the Simulation:

    Navigate to the directory where the Verilog files are located:
```
    cd /home/satya/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
   Run the following commands to compile and simulate the design:
```
iverilog dff_asyncres.v tb_dff_asyncres.v
ls
```
The compiled output will be saved as a.out.

 Execute the compiled output and open the waveform viewer:
```
./a.out
gtkwave tb_dff_asyncres.vcd
```
By following these steps,we can observe the behavior of the D Flip-Flop with an asynchronous reset in the waveform viewer:

![image](https://github.com/user-attachments/assets/eb8036b8-0694-436a-a483-1863b0a3b532)

OBSERVATION : From the waveform, it can be observed that the Q output changes to zero when the asynchronous reset is set high, independent of the positive/negative clock edge.

## 2. Asynchronous Set

The velilog code for the Asynchronous set is given below :

```c

module dff_async_set(input clk, input async_set, input d, output reg q);
	always@(posedge clk, posedge async_set)
	begin
		if(async_set)
			q <= 1'b1;
		else
			q <= d;
	end
endmodule
```
Testbench code is as follows:
```c
module tb_dff_async_set; 
	reg clk, async_set, d;
	wire q;
	dff_async_set uut (.clk(clk),.async_set (async_set),.d(d),.q(q));

	initial begin
		$dumpfile("tb_dff_async_set.vcd");
		$dumpvars(0,tb_dff_async_set);
		// Initialize Inputs
		clk = 0;
		async_set = 1;
		d = 0;
		#3000 $finish;
	end
		
	always #10 clk = ~clk;
	always #23 d = ~d;
	always #547 async_set=~async_set; 
endmodule
```
Command steps :

Go to the required directory
```c
sudo -i
cd ~
cd /home/satya/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
We just need to put few commands as stated below in order to see the waveforms.
```c
iverilog dff_async_set.v tb_dff_async_set.v
ls
```
After giving the above command the IVerilog stores the output as ' a.out '

Now let's execute the ' a.out ' file and observe the waveforms.
```c
./a.out
gtkwave tb_dff_async_set.vcd
```
Below is the Snapshot of the above commands and the resultant Waveforms:

![image](https://github.com/user-attachments/assets/a658af31-7170-4203-8804-1d9c111c93bd)

OBSERVATION : From the waveform, it can be observed that the Q output changes to one when the asynchronous set is set high, independent of the positive/negative clock edge.

## 3. Synchronous Reset

The velilog code for the Synchronous reset is given below :
```c

module dff_syncres(input clk, input sync_reset, input d, output reg q);
	always@(posedge clk)
	begin
		if(sync_reset)
			q <= 1'b0;
		else
			q <= d;
	end
endmodule
```
Testbench code is as follows:
```c
module tb_dff_syncres; 
	reg clk, syncres, d;
	wire q;
	dff_asyncres uut (.clk(clk),.sync_reset (sync_reset),.d(d),.q(q));

	initial begin
		$dumpfile("tb_dff_syncres.vcd");
		$dumpvars(0,tb_dff_syncres);
		// Initialize Inputs
		clk = 0;
		sync_reset = 1;
		d = 0;
		#3000 $finish;
	end
		
	always #10 clk = ~clk;
	always #23 d = ~d;
	always #547 sync_reset=~async_reset; 
endmodule
```
Command steps :

Go to the required directory
```
sudo -i
cd ~
cd /home/satya/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
We just need to put few commands as stated below in order to see the waveforms.
```
iverilog dff_syncres.v tb_dff_syncres.v

```
After giving the above command the IVerilog stores the output as ' a.out '

Now let's execute the ' a.out ' file and observe the waveforms.
```
./a.out
gtkwave tb_dff_syncres.vcd
```
Below is the Snapshot of the above commands and the resultant Waveforms:
![image](https://github.com/user-attachments/assets/5707043e-d8d1-43ff-90d1-53a74f079b73)

## Synthesis of Various D-Flipflop using Yosys
I am performing this using 3 types of D Flip Flops:
* 1. Asynchronous Reset
* 2.   Asynchronous Set
 * 3.   Synchronous Reset.

## 1. Asynchronous Reset

Follow the steps to get Graphical Representation of Asynchronous Reset - D FlipFlop
```c
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys       
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show


```
![image](https://github.com/user-attachments/assets/efc7bc84-b7cd-46c7-940a-258969ca3736)

## 2. Asynchronous Set
Command steps to Create a Graphical Representation of Asynchronous Set - D FlipFlop
```c
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys       
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/user-attachments/assets/acacc62d-8b41-45d1-bb76-b710e7c73fa3)


## 3. Synchronous Reset
Command steps to Create a Graphical Representation of Synchronous Reset - D FlipFlop :
```c
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys       
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/user-attachments/assets/ba7c91a1-e1f0-42a3-852f-a9d8c05786b1)
</details>
<details>
<summary>Day-3</summary>
	
# LAB 6 - AIM : Optimization of various Combinational Designs
## Optimization of various Combinational Designs

  2 input AND gate.
  2 input OR gate.
  3 input AND gate.
  2 input XNOR Gate (3 input Boolean Logic)
  Multiple Module Optimization-1
  Multiple Module Optimization-2

### 1. 2 input AND gate.

The velilog code is given below :
```c
module opt_check(input a, input b, output y);
	assign y = a?b:0;
endmodule
```
Command steps :
```c
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys       
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
```
![image](https://github.com/user-attachments/assets/7e84709f-c224-45b2-816c-ac566eda1a23)

### Now Generate the Netlist
```c
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Removes unused or redundant logic in the design and purges any dangling wires or gates.
```c
opt_clean -purge
```
Now let's Create a Graphical Representation
```c
show
```
![image](https://github.com/user-attachments/assets/0cac41b9-0764-4790-b0e2-9b3f7c54287d)

## 2. 2 input OR gate.

The velilog code is given below :
```c
module opt_check2(input a, input b, output y);
	assign y = a?1:b;
endmodule
```
Command steps :

Go to the required directory
```c
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog opt_check2.v
```
Synthesize the Design
```
synth -top opt_check2
```
![image](https://github.com/user-attachments/assets/38536525-4546-48ef-aa7e-c93d1bf075c5)

Now Generate the Netlist
```c

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Removes unused or redundant logic in the design and purges any dangling wires or gates.
```
opt_clean -purge
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/373c65c6-3d58-4272-bd4d-78e3dad0f4a8)

## 3. 3 input AND gate.

The velilog code is given below :
```c
module opt_check2(input a, input b, input c, output y);
	assign y = a?(b?c:0):0;
endmodule
```
Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog opt_check3.v
```
Synthesize the Design
```
synth -top opt_check3
```
![image](https://github.com/user-attachments/assets/f3b37522-7452-4c2a-9ba7-86dc03529d46)

Now Generate the Netlist
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Removes unused or redundant logic in the design and purges any dangling wires or gates.
```
opt_clean -purge
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/e5d0da6b-e95a-4fc6-ba45-1cd7016265dd)

## 4. 2 input XNOR Gate (3 input Boolean Logic)

The velilog code is given below :
```
module opt_check2(input a, input b, input c, output y);
	assign y = a ? (b ? ~c : c) : ~c;
endmodule
```
Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog opt_check4.v
```
Synthesize the Design
```
synth -top opt_check4
```
![image](https://github.com/user-attachments/assets/c03723f7-3a64-47b9-8de8-53978e6cd6e7)

Now Generate the Netlist
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Removes unused or redundant logic in the design and purges any dangling wires or gates.
```
opt_clean -purge
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/ea59862c-65cb-44d8-a700-3c8ea2b5956b)

## 5. Multiple Module Optimization-1

The velilog code is given below :
```
module sub_module1(input a, input b, output y);
	assign y = a & b;
endmodule

module sub_module2 (input a, input b output y);
	assign y = a^b;
endmodule

module multiple_module_opt(input a, input b input c, input d output y);
	wire n1,n2, n3;

	sub_module1 U1 (.a(a), .b(1'b1), .y(n1));
	sub_module2 U2 (.a(n1), .b(1'b0), .y(n));
	sub_module2 U3 (.a(b), .b(d), .y(n3));

	assign y = c | (b & n1);
endmodule
```
Command steps :

Go to the required directory
```c
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```c
read_verilog multiple_module_opt.v
```
Synthesize the Design
```c
synth -top multiple_module_opt
```
![image](https://github.com/user-attachments/assets/11257847-22c4-47be-8386-6a372cc2b3e8)

Now Generate the Netlist
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Removes unused or redundant logic in the design and purges any dangling wires or gates.
```
opt_clean -purge
```
Use of Flattening: Merges all hierarchical modules in the design into a single module to create a flat netlist for this just type
```
flatten
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/13d628c4-239b-4efe-a2a6-3aad33483f8f)



## 6. Multiple Module Optimization-2

The velilog code is given below :
```c
module sub_module(input a input b output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a, input b input c, input d, output y);
	wire n1,n2, n3;

	sub_module U1 (.a(a), .b(1'b0), y(n));
	sub_module U2 (.a(b), .b(c), .y(n2));
	sub_module U3 (.a(n2), .b(d), .y(n));
	sub_module U4 (.a(n3), .b(n1), .y(y));
endmodule
```
Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog multiple_module_opt2.v
```
Synthesize the Design
```
synth -top multiple_module_opt2
```
![image](https://github.com/user-attachments/assets/866a571e-a478-4585-8135-de9a80f3b515)

Now Generate the Netlist
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Removes unused or redundant logic in the design and purges any dangling wires or gates.
```
opt_clean -purge
```
Use of Flattening: Merges all hierarchical modules in the design into a single module to create a flat netlist for this just type
```
flatten
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/1af0f686-38fc-4823-a2cb-2369aab3a148)

# LAB 6 - AIM : Optimization of various Sequential Designs
## Optimization of various Sequential Designs

1. D-Flipflop Constant 1 with Asynchronous Reset (active low)
2.  D-Flipflop Constant 2 with Asynchronous Reset (active high)
3. D-Flipflop Constant 3 with Synchronous Reset (active low)
 4. D-Flipflop Constant 4 with Synchronous Reset (active high)
5. D-Flipflop Constant 5 with Synchronous Reset
 6. Counter Optimization 1
 7.   Counter Optimization 2

## 1. D-Flipflop Constant 1 with Asynchronous Reset (active low)

The velilog code for the asynchronous reset (active low) is given below :
```c
module dff_const1(input clk, input reset, output reg q); 
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```
Testbench code is as follows:
```
module tb_dff_const1; 
	reg clk, reset;
	wire q;

	dff_const1 uut (.clk(clk),.reset(reset),.q(q));

	initial begin
		$dumpfile("tb_dff_const1.vcd");
		$dumpvars(0,tb_dff_const1);
		// Initialize Inputs
		clk = 0;
		reset = 1;
		#3000 $finish;
	end

	always #10 clk = ~clk;
	always #1547 reset=~reset;
endmodule
```
Command steps :

Go to the required directory
```
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
We just need to put few commands as stated below in order to see the waveforms.
```
iverilog dff_const1.v tb_dff_const1.v
ls
```
After giving the above command the IVerilog stores the output as ' a.out '

Now let's execute the ' a.out ' file and observe the waveforms.
```
./a.out
gtkwave tb_dff_const1.vcd
```
Below is the Snapshot of the above commands and the resultant Waveforms:

![image](https://github.com/user-attachments/assets/392e515c-746f-4e06-9575-4dccbcfbab69)

OBSERVATION : From the waveform, it can be observed that the Q output is always high when reset is zero, and reset doesn't depend on clock edge.

SYNTHESIS :
Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog dff_const1.v
```
Synthesize the Design
```
synth -top dff_const1
```
Now Generate the Netlist
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/d0cf5fc4-63bc-4201-b75a-8054456ad8a5)

OBSERVATION : Since reset doesn't depend on clock edge, therefore the D Flip Flop has not been removed.

## 2. D-Flipflop Constant 2 with Asynchronous Reset (active high)

The velilog code for the asynchronous reset (active high) is given below :
```
module dff_const1(input clk, input reset, output reg q); 
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```
Testbench code is as follows:
```
module tb_dff_const2; 
	reg clk, reset;
	wire q;

	dff_const2 uut (.clk(clk),.reset(reset),.q(q));

	initial begin
		$dumpfile("tb_dff_const1.vcd");
		$dumpvars(0,tb_dff_const1);
		// Initialize Inputs
		clk = 0;
		reset = 1;
		#3000 $finish;
	end

	always #10 clk = ~clk;
	always #1547 reset=~reset;
endmodule
```
Command steps :

Go to the required directory
```
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
We just need to put few commands as stated below in order to see the waveforms.
```
iverilog dff_const2.v tb_dff_const2.v
ls
```
After giving the above command the IVerilog stores the output as ' a.out '

Now let's execute the ' a.out ' file and observe the waveforms.
```
./a.out
gtkwave tb_dff_const2.vcd
```
Below is the Snapshot of the above commands and the resultant Waveforms:
![image](https://github.com/user-attachments/assets/b403ef14-96c5-4536-b1b0-8261ff0c1d69)

OBSERVATION : From the waveform, it can be observed that the Q output is always high irrespective of reset.

### SYNTHESIS :

Go to the required directory
```c
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog dff_const2.v
```
Synthesize the Design
```
synth -top dff_const2
```
![image](https://github.com/user-attachments/assets/47e5fb31-8bb3-4c50-94da-701fa1970586)


### OBSERVATION : Now D Flip Flop has been synthesised.

Now Generate the Netlist
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/940bc44c-6573-42b4-ae90-fb7fc5802a4b)
OBSERVATION : Since output q doesn't depend on reset edgeand is always 1, therefore the D Flip Flop has been removed.

## 3. D-Flipflop Constant 3 with Synchronous Reset (active low)

The velilog code for the synchronous reset (active low) is given below :
```
module dff_const3(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b1;
			q1 <= 1'b0;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```
Testbench code is as follows:
```
module dff_const3(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b1;
			q1 <= 1'b0;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```
### SYNTHESIS :
Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys

yosys       

Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog dff_const3.v
```
Synthesize the Design
```
synth -top dff_const3
```

![image](https://github.com/user-attachments/assets/d47b12f7-fb16-40ee-8e65-623d327fc8ba)

Now Generate the Netlist
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/76291f62-aefa-405c-bf8e-6aa9a4ceaad7)
OBSERVATION : This module defines a D flip-flop, for a positive edge of reset, q is set to 1 and q1 is set to 0. On each clock cycle, q1 is set to 1, and q is updated with the value of q1.

When synthesized, the design will result in a flip-flop where q becomes 1 after the first clock cycle post-reset and stays 1 afterward.

## 4. D-Flipflop Constant 4 with Synchronous Reset (active high)

The velilog code for the synchronous reset (active high) is given below :
```
module dff_const4(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b1;
			q1 <= 1'b1;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```
### SYNTHESIS :

Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog dff_const4.v
```
Synthesize the Design
```
synth -top dff_const4
```
![image](https://github.com/user-attachments/assets/d49ac6e9-a2d7-4c14-bda3-77b4e42db309)


Now Generate the Netlist
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/10d2184e-cb8b-4671-aaf1-2b07ed0ebc7f)
OBSERVATION : This module defines a D flip-flop that sets both q and q1 to 1 on a positive edge of reset. On each clock cycle, q1 remains 1, and q is updated with the value of q1 (which is always 1).

When synthesized, the design will result in a flip-flop where q is always 1, regardless of the reset or clock state.

## 5. D-Flipflop Constant 5 with Synchronous Reset

The velilog code for the synchronous reset is given below :
```
module dff_const5(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b0;
			q1 <= 1'b0;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```
### SYNTHESIS :
Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog dff_const5.v
```
Synthesize the Design
```
synth -top dff_const5

```
![image](https://github.com/user-attachments/assets/37d5de10-11fa-4d8a-b487-3251fee86e08)

Now Generate the Netlist
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/70b9aca6-35a1-41bc-a13b-acf76e24e4d8)

OBSERVATION : This module defines a D flip-flop that resets both q and q1 to 0 on a positive edge of reset. On each clock cycle, it sets q1 to 1 and then updates q with the value of q1 (which will always be 1 after the first cycle).

When synthesized, the design will result in a flip-flop where q is always 1 after the first clock cycle post-reset.


## 6. Counter Optimization 1

The verilog code for the Counter Optimization 1 is given below :
```c
module counter_opt (input clk, input reset, output q);
	reg [2:0] count;
	assign q = count[0];
	
	always @(posedge clk,posedge reset)
	begin
		if(reset)
			count <= 3'b000;
		else
			count <= count + 1;
	end
endmodule
```
## SYNTHESIS :
Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog counter_opt.v
```
Synthesize the Design
```
synth -top counter_opt
```
![image](https://github.com/user-attachments/assets/54dfcaab-f544-4295-b8b8-a075d1f92f6c)


Now Generate the Netlist
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now let's Create a Graphical Representation
```
show
```
![image](https://github.com/user-attachments/assets/0bd80314-a5fe-4569-b6df-311cd63f87ba)

## 7. Counter Optimization 2

The velilog code for the synchronous reset (active high) is given below :
```
module counter_opt2 (input clk, input reset, output q);
	reg [2:0] count;
	assign q = (count[2:0] == 3'b100);
	
	always @(posedge clk,posedge reset)
	begin
		if(reset)
			count <= 3'b000;
		else
			count <= count + 1;
	end
endmodule
```
## SYNTHESIS :

Command steps :

Go to the required directory
```
cd ~
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
This will invoke/start the yosys
```
yosys       
```
Read the library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Read the design verilog files
```
read_verilog counter_opt2.v
```
Synthesize the Design
```
synth -top counter_opt2
```
Now Generate the Netlist
```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Now let's Create a Graphical Representation
```
show
```
</details>

<details>
	<summary>Day-4</summary>
<br>
	
 # Lab 8: AIM : GLS, Synthesis-Simulation mismatch, non - blocking and blocking statements.
 
## Gate Level Simulation (GLS) in Digital Circuit Verification

Gate Level Simulation (GLS) is a critical step in the verification process of digital circuits. This phase involves simulating the synthesized netlist—a lower-level representation of the design—using a testbench to verify its logical correctness and timing behavior. By comparing the simulated outputs with the expected outputs, GLS ensures that the synthesis process has not introduced any errors and that the design meets its performance requirements.

Importance of Sensitivity Lists

Sensitivity lists are essential for achieving accurate circuit behavior. An incomplete sensitivity list can lead to unexpected latches, which may complicate the design's functionality.
Blocking vs. Non-Blocking Assignments

Within always blocks, the use of blocking and non-blocking assignments plays a significant role:

* Blocking Assignments (=): These execute sequentially, which can inadvertently create latches if not managed properly.
* Non-Blocking Assignments (<=): These allow for parallel execution and help avoid issues related to timing and latches.

Best Practices

To prevent synthesis and simulation mismatches, it’s vital to:

* 1. Carefully Analyze Circuit Behavior: Understand the flow of signals and potential edge cases.
* 2. Ensure Proper Sensitivity Lists: Make sure that all relevant signals are included to avoid unintended behavior.
* 3. Choose the Right Assignment Type: Use blocking and non-blocking assignments appropriately based on the intended functionality.

By adhering to these practices, we can enhance the reliability and robustness of digital circuits, leading to successful implementations in real-world applications.

![image](https://github.com/user-attachments/assets/940a2e31-f235-415f-8255-a27caf84109a)

## Example 1 : There is no mismatch in this example as the netlist simulation and rtl simulation waveform are similar only
```c
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
endmodule
```
### Command steps for Simulation:
```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![image](https://github.com/user-attachments/assets/7e4f2ed2-ae81-4b4b-a98a-63d5acbd472e)

Synthesis:

```c
yosys       
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
```
![image](https://github.com/user-attachments/assets/25f3dade-f4eb-406e-8e2c-1754e99d6bd8)
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
![image](https://github.com/user-attachments/assets/808d63e5-89f1-4b19-8b3b-661c28b15e34)

To see the Netlist
```c
write_verilog -noattr ternary_operator_mux_net.v
!gvim ternary_operator_mux_net.v
```
![Screenshot from 2024-10-21 18-12-52](https://github.com/user-attachments/assets/ad59e97c-b690-4ca6-8db1-14a44c16fde3)

### Gate Level Synthesis (GLS)
Command steps :

Go to the required directory
```
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
We just need to put few commands as stated below in order to see the waveforms.
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
ls
```
After giving the above command the IVerilog stores the output as ' a.out '

Now let's execute the ' a.out ' file and observe the waveforms.
```
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
Below is the Snapshot of the above commands and the resultant Waveforms:

![image](https://github.com/user-attachments/assets/34d3f84a-9e2d-4f4d-b358-b977d8c10f0c)

These waveforms correspond to the GATE LEVEL SYNTHESIS for the Ternary Operator MUX.

  ## * Example 2: Design of a 2:1 Bad MUX

Verilog code:
```
module bad_mux(input i0, input i1, input sel, output reg y);
	always@(sel)
	begin
		if(sel)
			y <= i1;
		else
			y <= i0;
	end
endmodule
```
Command steps for Simulation:
```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
![image](https://github.com/user-attachments/assets/f5356b19-8b8b-4ea0-9e77-72555d5c2026)

![image](https://github.com/user-attachments/assets/5dc9c503-7d15-401f-b3da-f534a81703e3)

To see the Netlist
```
write_verilog -noattr bad_mux_net.v
!gvim bad_mux_net.v
```
![image](https://github.com/user-attachments/assets/8ac851b0-dbb9-4255-80c5-d3728fd82df4)
From the waveform it can be observed that the output y changes only when there is a change in select line, completely ignoring the change in i0 and i1, which should also change the output y. Thus, this design is that of a bad MUX.

### Gate Level Synthesis (GLS)
Command steps :

Go to the required directory
```
sudo -i
cd ~
cd /home/vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
We just need to put few commands as stated below in order to see the waveforms.
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux.v tb_bad_mux.v
ls
```
After giving the above command the IVerilog stores the output as ' a.out '

Now let's execute the ' a.out ' file and observe the waveforms.
```
./a.out
gtkwave tb_bad_mux.vcd
```
Below is the Snapshot of the above commands and the resultant Waveforms:
![image](https://github.com/user-attachments/assets/2c5661e7-373f-498e-8bcb-758dffc92026)
These waveforms correspond to the GATE LEVEL SYNTHESIS for the Bad MUX.


##  Example 3: Blocking Caveat

Verilog code:

```c

module blocking_caveat(input a, input b, input c, output reg d);
	reg x;

	always@(*)
	begin
		d = x & c;
		x = a | b;
	end
endmodule
```
Command steps for Simulation:
```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
![image](https://github.com/user-attachments/assets/881e0f93-e331-4785-b5f0-206a4536efd8)

As depicted, when A and B go zero, the OR gate output should be zero (X equal to zero), and the AND gate output should also be zero (same as D output). But, the AND gate input of X takes the previous value of A|B equal to one, based on the design created by the blocking statement, hence the discrepancy in the output.

### Synthesis

![image](https://github.com/user-attachments/assets/dacc284d-658e-4dc9-b78a-f4803735036a)

To see the Netlist
```
write_verilog -noattr blocking_caveat_net.v
!gvim blocking_caveat_net.v
```
![image](https://github.com/user-attachments/assets/11748ef0-abf6-4f83-8faa-eeb81fa18a7c)

## Gate Level Synthesis (GLS) Workflow

Welcome to the Gate Level Synthesis (GLS) section of the VLSI Sky130 RTL Design and Synthesis Workshop! Below are the command steps you'll need to follow in order to simulate your design and visualize the waveforms.

Step-by-Step Guide

1. Navigate to the Project Directory
```c
sudo -i
cd ~
cd /home/chandra-shekhar-jha/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
2. Compile Your Verilog Files

Next, we need to compile the necessary Verilog files using IVerilog. Run the following command:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
```

3. Execute the Output File

Now it’s time to run the simulation and generate the waveform data:
```
./a.out
```
5. Visualize Waveforms with GTKWave

Finally, to visualize the results, you can open the waveform viewer:
```
gtkwave tb_blocking_caveat.vcd
```
Result

![image](https://github.com/user-attachments/assets/f85e199c-545f-4dad-8703-a190249970f7)



</details>
