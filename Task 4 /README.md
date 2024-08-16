# AIM : To write a simple application in 'C' that can be compiled using GCC and RISC-V GCC.
## APPLICATION : Check if the given number is armstrong
INTRODUCTION :An Armstrong number is a number that is equal to the sum of its digits, each raised to the power of the number of digits. This means if you take each digit in the number, raise it to the power of the total number of digits, and add them up, you get the original number.
Step 1 : Open the text editor and write the program in 'C' to check the number is armstrong number or not.
         

         
```
#include <stdio.h>
#include <math.h>

int main() {
    int n, original, remainder, result = 0, digits = 0;

    // Input the number
    printf("Enter an integer: ");
    scanf("%d", &n);

    original = n;

    // Calculate the number of digits
    while (original != 0) {
        original /= 10;
        digits++;
    }

    original = n;

    // Calculate the sum of the powers of its digits
    while (original != 0) {
        remainder = original % 10;
        result += pow(remainder, digits);
        original /= 10;
    }

    // Check if the number is an Armstrong number
    if (result == n)
        printf("%d is an Armstrong number.\n", n);
    else
        printf("%d is not an Armstrong number.\n", n);

    return 0;
}


           

     


```
Created  a folder named 
   ```
   armstrong.c
```

![1 1](https://github.com/user-attachments/assets/ec594fdc-4895-46ca-8093-5559c2dd61d6)
## Step 2 : Compile it using GCC by giving the following series of command.
```
gcc armstrong.c
```
press ` Enter `

```
./a.out
```
press `  Enter `

Output after compiling my c code using gcc


![2 2](https://github.com/user-attachments/assets/affc2ead-7850-4761-991e-792e9493c129)
## Step 3 : Compile it using RISC-V GCC and verify the output using SPIKE by giving the following series of command.

```
riscv64-unknown-elf-gcc -O1 -o armstrong armstrong.c -lm

```
Press ` Enter `

```
spike pk compliment
```
Press `  Enter         `

Output after compiling using risc v gcc

![3 3](https://github.com/user-attachments/assets/202e77d0-f10c-49ae-8dfc-c7b93d0da14c)

### Result 
 I am getting same output after compiling my c code through gcc and risc v gcc

