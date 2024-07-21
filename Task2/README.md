# Task2: Get the same output from both gcc and RISCV gcc, and also debug the output got from riscv gcc.












## step1: compile the code using gcc and get output using ./a.out

run the following code
 
     gcc sum1ton.code

    ./a.out

    

## step2: compile the code using riscv gcc and get output using spike

run the following code

     riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c

     spike pk sum1ton.o

if we get the same output, it is verified that simulations are working as per expected

![1](https://github.com/user-attachments/assets/f41f06b6-0790-428d-88ea-7ce2b6ba6937)
    
## step3: now debug the output 
for that open the objdump of the output using the below command

     riscv64-unknown-elf-objdump -d sum1ton.o | less

![2](https://github.com/user-attachments/assets/e664d32a-a2ad-45a8-af38-5fab3fe6b2ab)

now inorder to debug, we need to open a debuger, to do that we are going to use spike

     spike -d pk sum1ton.o

now if we want run the program counter to run till 100b0 and after this run the commands mannually, run the following command

     until pc 0 100b0

after running this command, you must see "bbl loader", this ensures assembly code has run till 100b0 address

now see the objdump to see what is the next instruction, in my case 

1. first command is lui a0, 0x21 which changes a0 register value

- so first check the value of reg a0 using the below command
     
      reg 0 a0

- inorder to run the instruction press Enter

- check the value of regiester again

      reg 0 a0

now we can observe the value of the a0 register has been modified

2. second command is addi sp,sp,-16 which changes sp value

- so first check the value of stack pointer using the below command
     
      reg 0 sp

- inorder to run the instruction press Enter

- check the value of sp regiester again

      reg 0 sp

now we can observe the value of the sp register has been modified

3. third command is li a2,55 which changes a2 register value

- so first check the value of reg a2 using the below command
     
      reg 0 a2

- inorder to run the next instruction press Enter

- check the value of regiester again

      reg 0 a2

now we can observe the value of the a2 register has been modified

![3](https://github.com/user-attachments/assets/1f27a2a9-a906-4c03-8781-bcdeb4f3e827)

you can see the calculation for the stack pointer from the below image

- initial value of stack pointer: 3ffffffb50

- subtracted by: 16 in decimal which is 10 in hexadecimal

![6](https://github.com/user-attachments/assets/6d7c732c-95be-4f3c-a26f-05e29e206a17)
