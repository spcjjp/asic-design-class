Task: To watch C based and RISCV based lab videos and execute the task of compiling the C code using gcc and riscv compiler

1. Compiling C code with gcc

step1 : create a C file in the home directory

           leafpad sum1ton.c
and write the C code

![code](https://github.com/user-attachments/assets/48332797-b4bd-418a-96f1-970ea72bc7f1)

step2 : compile the C code using gcc command as shown below

           gcc sum1ton.c  # for compiling sum1ton.c

           ./a.out # to run the executable file created
![Screenshot 2024-07-17 125706](https://github.com/user-attachments/assets/44984674-30c0-47a1-800b-8aa6f6fd283f)

2. Compiling C code with RISCV gcc

step1 : Compile using RISCV gcc compiler as shown below

            riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c    

step2 : To See the assembly code of the C program, run the following command.

            riscv64-unknown-elf-objdump -d sum1ton.o
            ![ss 3rd](https://github.com/user-attachments/assets/a611af91-3936-4e49-9870-a28f5be1cb4b)

            
            

            
