# AIM : To write a simple application in 'C' that can be compiled using GCC and RISC-V GCC.
## APPLICATION : 1's and 2's compliment of a Decimal Number
INTRODUCTION :The task is to perform the 1's and 2's compliment of a decimal number
Step 1 : Open the text editor and write the program in 'C' that gives the 1's and 2's complimnet of a decimal number.
         First my code will convert the decimal number into binary then it wil calculate its 1's and 2's compliment of the binary and print the compliment of 
         the binary and conert it to decimal again.

         
```
 #include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_BITS 32 // Maximum bits for binary representation

// Function to convert a decimal number to binary string
void decimalToBinary(int decimal, char *binary) {
    int index = 0;
    if (decimal == 0) {
        binary[index++] = '0';
    } else {
        while (decimal > 0) {
            binary[index++] = (decimal % 2) ? '1' : '0';
            decimal /= 2;
        }
    }
    binary[index] = '\0';

  // Reverse the binary string
    for (int i = 0; i < index / 2; i++) {
        char temp = binary[i];
        binary[i] = binary[index - i - 1];
        binary[index - i - 1] = temp;
    }
}

// Function to calculate 1's complement of a binary string
void onesComplement(const char *binary, char *complement) {
    int length = strlen(binary);
    for (int i = 0; i < length; i++) {
        complement[i] = (binary[i] == '0') ? '1' : '0';
    }
    complement[length] = '\0';
}

// Function to calculate 2's complement from 1's complement binary string
void twosComplement(const char *onesComplement, char *twosComplement) {
    int length = strlen(onesComplement);
    int carry = 1;

  // Calculate 2's complement by adding 1 to 1's complement
    for (int i = length - 1; i >= 0; i--) {
        int sum = (onesComplement[i] - '0') + carry;
        twosComplement[i] = (sum % 2) + '0';
        carry = sum / 2;
    }
    twosComplement[length] = '\0';
}

// Function to convert a binary string to decimal number
int binaryToDecimal(const char *binary) {
    int decimal = 0;
    int length = strlen(binary);
    for (int i = 0; i < length; i++) {
        if (binary[length - i - 1] == '1') {
            decimal += (1 << i);
        }
    }
    return decimal;
}

int main() {
    int decimal;
    char binary[MAX_BITS + 1]; // Binary representation (32 bits + null terminator)
    char onesComp[MAX_BITS + 1]; // 1's complement
    char twosComp[MAX_BITS + 1]; // 2's complement
    printf("Enter a decimal number: ");
    scanf("%d", &decimal);

  // Convert decimal to binary
    decimalToBinary(decimal, binary);
    printf("Binary representation: %s\n", binary);

  // Calculate 1's complement
  onesComplement(binary, onesComp);
    printf("1's complement in binary: %s\n", onesComp);

 // Calculate 2's complement
    twosComplement(onesComp, twosComp);
    printf("2's complement in binary: %s\n", twosComp);

  // Convert 1's complement binary to decimal
    int onesCompDecimal = binaryToDecimal(onesComp);
    printf("1's complement in decimal: %d\n", onesCompDecimal);

  // Convert 2's complement binary to decimal
    int twosCompDecimal = binaryToDecimal(twosComp);
    printf("2's complement in decimal: %d\n", twosCompDecimal);   return 0;
}
```
Created  a folder named 
   ```
   complimnet.c
```

![1](https://github.com/user-attachments/assets/eb851b1f-3f7a-49fd-b5a2-86bfa743fe9b)

## Step 2 : Compile it using GCC by giving the following series of command.
```
gcc compliment.c
```
press ` Enter `

```
./a.out
```
press `  Enter `

Output after compiling my c code using gcc


![2](https://github.com/user-attachments/assets/8fbde3b4-eed6-44fa-ae91-712afead5bfb)

## Step 3 : Compile it using RISC-V GCC and verify the output using SPIKE by giving the following series of command.

```
riscv64-unknown-elf-gcc -O1 -o compliment compliment.c -lm

```
Press ` Enter `

```
spike pk compliment
```
Press `  Enter         `

Output after compiling using risc v gcc

![3](https://github.com/user-attachments/assets/c7540658-4cb7-40d8-a8a8-bfd8916ccf8f)

### Result 
 I am getting same output after compiling my c code through gcc and risc v gcc

