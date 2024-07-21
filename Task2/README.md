# Task2: Get the same output from both gcc and RISCV gcc












## step1: compile using gcc and output using ./a.out
 
     gcc sum1ton.code

    ./a.out

   ![Screenshot 2024-07-20 172340](https://github.com/user-attachments/assets/ced655e3-a8a5-409b-852c-334225f9c482)



    

## step2: To check if we are getting same output with riscv compiler

run the following code

     riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c

     spike pk sum1ton.o

After this if we are getting same output, then it is verified

![Screenshot 2024-07-20 175448](https://github.com/user-attachments/assets/95fc87cb-95fb-44d0-923b-4a821c8c72b5)

    
## step3: When we are to debug, we open debugger using spike

     spike -d pk sum1ton.o

now if we want run our program counter to run till 100b0 
   
     until pc 0 100b0
 
 and after this run the commands mannually

after this , you must see "bbl loader", this ensures assembly code has run till 100b0 address

now see the objdump to see what is the next instruction, in my case 

1. first command is lui a0, 0x21 which changes a0 register value


![Screenshot 2024-07-20 180140](https://github.com/user-attachments/assets/c5a40beb-1995-44ff-a2b7-1b936fbab489)
    

- so first check the value of reg a0 using the below command
     
       :reg 0 a0

-> Press Enter

-> To find the contents of a2

      reg 0 a0

 it get modified

2. second command is addi sp,sp,-16 which changes sp value

-> check the value of stack pointer using the below command
     
      reg 0 sp

-> press Enter

-> check the value of sp regiester again

      reg 0 sp

now you can go further like this 

![3](https://github.com/user-attachments/assets/1f27a2a9-a906-4c03-8781-bcdeb4f3e827)

![6](https://github.com/user-attachments/assets/6d7c732c-95be-4f3c-a26f-05e29e206a17)
