# RISC-V INSTRUCTION SET ARCHITECTURE (ISA)
ISA stands for "Instruction Set Architecture." It is a crucial concept in computer architecture that defines the set of instructions that a computer's hardware can execute. An ISA essentially serves as an interface between the hardware and the software, allowing software programs to communicate with and control the underlying hardware components of a computer.

The ISA defines various aspects of a computer's operation, including:

- Instruction Set: The specific instructions that a computer's processor can understand and execute. These instructions can include arithmetic operations, memory operations, control flow   instructions, and more.

- Data Types: The types of data that the processor can handle, such as integers, floating-point numbers, and characters.

- Registers: Special storage locations within the processor that are used to hold data temporarily during instruction execution. Different ISAs may have different numbers and types of registers.

- Memory Addressing: The way in which the ISA specifies how memory locations are addressed and accessed. This includes addressing modes, which determine how operands are retrieved from memory.

- Control Flow: How the sequence of instructions is controlled, including branching (conditional and unconditional jumps) and subroutine calls.

- I/O Operations: The instructions and mechanisms for interacting with input and output devices.

  There are two main types of ISAs:

1. Complex Instruction Set Computer (CISC): In CISC architectures, instructions are relatively complex and can perform multiple tasks in a single instruction. This reduces the number of instructions needed to accomplish a task but can make the hardware more complex.

2. Reduced Instruction Set Computer (RISC): RISC architectures focus on simpler instructions that are executed in a single clock cycle. RISC architectures often have a smaller set of instructions, but more instructions might be required to complete a complex task. This design simplifies the hardware and can lead to better performance and efficiency in certain scenarios.

RISC-V (pronounced "risk-five") is an open-source instruction set architecture (ISA) for computer processors. An instruction set architecture is a set of instructions that a processor can execute, defining the interface between software and hardware. RISC-V is designed to be simple, modular, and extensible.


## Table of Content
- [Day-0: 
- Day-1:



## DAY-0: Installation of the Tools
## DAY-1: Introduction to RISC-V ISA and GNU compiler toolchain
<details> 
 
 <summary> INTRODUCTION  </summary>
  
  ---
  In this section we will learn what exactly is the Instruction Set Architecture (ISA) role in a device and why it is required.  
![Screenshot from 2023-08-21 10-46-39](https://github.com/akul-star/RISC-V/assets/75561390/ae4ea0da-5b23-4771-90d3-4ef404471e51)

Let's explore how applications communicate with hardware components through various layers, including the operating system (OS), compiler, assembler, and a Register Transfer Language (RTL) snippet.

1. Operating System (OS):
    The operating system provides an abstraction layer between applications and hardware. It manages the hardware resources, such as memory, processors, and I/O devices, and provides services that applications can use. 

2. Compiler:
    The compiler translates high-level programming code written in languages like C, C++, or Java into machine code that the hardware can execute. During compilation, the compiler maps high-level code constructs to appropriate machine instructions. For instance, if an application contains a loop, the compiler generates machine instructions that correspond to looping constructs supported by the ISA (RISC-V in our case).

3. Assembler:
    An assembler converts assembly language code (a human-readable representation of machine code) into actual machine code. Assembly language is a low-level representation of the ISA, and each assembly instruction typically corresponds to a single machine instruction. Assemblers take care of translating assembly mnemonics into binary machine code that the hardware understands. The ISA acts as a abstract interface between the high level language like C, C++ and JAVA & the hardware.

4. RTL Snippet (Register Transfer Language):
RTL is a description of digital circuits using registers, data paths, and control logic. It's used in hardware design to describe the behavior of digital systems at a low level. 

</details>


<details>
<summary> Description of Course Content </summary>

---
In this curriculum, we will undertake an exploration of the operational mechanics of the RISC-V architecture and delve into the categorization of its assembly language constructs.

**1. 64 bit representation of signed & unsigned integer.**

**2. Application Binary Interface (ABI).**

**3. Memory allocation and Stack Pointer.**

**4. Single and double precision floating point extensions (RV64F & RV64D).**

**5. Multiply Extensions (RV64M)**

**6. Base Integer Instructions (RV64I)**

**7. Psuedo Instructions**

</details>

<details>
<summary> Labwork for RISC-V software toolchain  </summary>

---  
  
GCC Compile & Toolchain
========================
 
  **GNU Compiler toolchain:** The GNU Compiler Toolchain is a collection of essential software development tools, including the GCC compiler for languages like C and C++, Binutils for working with binary files, GDB debugger, and libraries like Glibc. It enables the creation, compilation, and debugging of programs, supporting diverse platforms and architectures. 

  Let us use GCC compiler for a C-program which sum's numbers from 1 to n.

```
#include <stdio.h>

int main () {
	int i,sum = 0, n = 6;
	for (i = 1; i <=n; ++i) {
		sum += i;
	}
	printf("The sum of the number from 1 to %d is %d\n", n,sum);
	return 0;
	}
```
- The above command can be compiled using the below command. 

```
gcc <filename>
./a.out
```
 ![sum1tonout](https://github.com/akul-star/RISC-V/assets/75561390/13092e94-ae8a-4853-b227-2d284efd5e51)

- To open the C-program inside the terminal, write the below command.
  ```
  cat <filename>
  ```
- Now we will run the code using RISC-V simulator to convert the C-program into RISC-V assembly language. The below command will create a compiled code named as <filename.o> which is the object file (An object file is an intermediate representation of your source code after it has been compiled by a compiler but before it's linked into an executable program or a library. It contains machine code instructions, data, and metadata that represent the compiled version of your source code.)
    ```
   riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton_O1.o sum1ton.c
    ```
- Open another terminal and write the below command to see the disassembled machine code instructions corresponding to the binary content in the object file.

  ```
  riscv64-unknown-elf-objdump -d sum1ton_O1.o | less
  ```
  ![Assemblylan](https://github.com/akul-star/RISC-V/assets/75561390/9e0da9bb-c884-4c57-b7ee-f91223c77e17)
    
  To view the address of the line main() or printf()) type **/main** or **/printf**. To quit type **:q**.

If we change the "O1" to "Ofast" in the context of GCC (GNU Compiler Collection), -O1 and -Ofast are both optimization flags that control how aggressively the compiler optimizes your code. However, they have different levels of optimization and might result in different behavior and performance characteristics.

  - O1: This flag turns on the first level of optimization. It enables a basic set of optimizations that aim to improve code size and execution speed without spending too much time on compilation. -O1 optimizations typically include inlining of small functions, constant propagation, and some basic loop optimizations.

  - Ofast: This flag enables aggressive optimizations that go beyond -O1. It includes all the optimizations enabled by -O2 (the second optimization level) and further applies transformations that might not strictly follow the C/C++ standards. For example, it might enable optimizations that assume strict IEEE compliance of floating-point operations, which could potentially lead to non-conforming behavior. This can result in significant performance improvements but might also introduce subtle issues if your code relies on strict adherence to language standards.


SPIKE Simulation & Debug
========================

Till now we have compiled the C-program using RISC-V simulator and has observed the assembly instructions of the C-program. To observe the ouput of the c-program using the riscv compiler, we givr the below mentioned command.
```
riscv64-unknown-elf-objdump -d sum1ton_O1.o | less
spike pk <filename>.o
```
**SPIKE ISA Simulator:**"Spike" refers to the RISC-V ISA Simulator, which is a functional simulator for the RISC-V instruction set architecture (ISA). It allows developers to run RISC-V assembly code on a simulated RISC-V processor, enabling them to test and experiment with RISC-V programs without needing access to physical RISC-V hardware. Now we will use the SPIKE simulator to debug the assembly instructions in the "main" content of the assembly instructions. Give the below command to open the debugger.

```
spike -d pk <file1ton>.o
```
When the debugger is open, give the below instruction for the program counter to run till the memory address location of first instruction of the "main" whcih is 1000b0.
```
until pc 0 1000b0
```
Now by pressing ENTER each assembly instruction will run one at a time.

---
![spikedebug](https://github.com/akul-star/RISC-V/assets/75561390/cfd3c9bd-b414-4a36-98dc-848e1d4721ba)

---
![Assemblylan](https://github.com/akul-star/RISC-V/assets/75561390/840108f8-5c64-4c95-a71d-59d61b7cc073)

</details>

<details> 
 
<summary> Integer Number Representation </summary>

Unsigned Numbers
================
Let's look into how does the RISC-V represents 64-but unsigned numbers.

---
![unsigned](https://github.com/akul-star/RISC-V/assets/75561390/b0c14489-ab2a-40e8-8d52-ae2dbc42c5b8)

An assembler converts human-readable instructions written in assembly language into a sequence of 0s and 1s that a specific computer chip designed with the RISC-V architecture can understand and execute, essentially translating our instructions into the language the chip "speaks". This is why it becomes very important to understand from human readable format to a binary format, and hoe binary is arranged and respresented by RISC-V implementation. Let's understand for a 64-bit RISC-V architecture,

---
![64bits](https://github.com/akul-star/RISC-V/assets/75561390/cd649cba-2672-47e8-ad7a-998fa715a840)

Here's a breakdown of the common terminology for data sizes in a 64-bit RISC-V architecture:

1. Byte: In a 64-bit RISC-V architecture, a "byte" refers to the smallest addressable unit of data, just like in any computer architecture. Regardless of the bit width of the processor's architecture, a byte remains a fundamental unit of data storage.

2. Word: A halfword is 16 bits or 2 bytes in size. It can store larger integer values than a byte.

3. Doubleword: In a 64-bit architecture, a word is typically 64 bits or 8 bytes in size. It can hold even larger integer values and is often used as the natural data size for many   operations and data storage.

4. Quadword: A quadword, also known as a "long long" or "octaword," is 128 bits or 16 bytes in size. It can store very large integer values or double-precision floating-point numbers.

Signed Numbers
=============

Signed numbers represent both positive and negative values. In a signed number representation, the leftmost bit (most significant bit) is reserved to indicate the sign of the number. For example, in a 4-bit signed representation, the leftmost bit would be the sign bit, and the remaining 3 bits would represent the magnitude of the number. In the two's complement representation, which is commonly used for signed integers in computers, the sign bit is 0 for positive numbers and 1 for negative numbers. The remaining bits represent the magnitude of the number using binary notation.

Two's complement is a common method used in computing to represent negative numbers in binary form. It simplifies arithmetic operations like addition and subtraction, as well as hardware implementation. Here's how to use two's complement to represent a negative number:

Determine the Positive Binary Representation:   

Start by representing the positive magnitude of the number in binary form. For example, let's use -5 as the negative number. The positive binary representation of 5 is 0101.

- Invert the Bits:To find the two's complement of a negative number, first invert all the bits of its positive binary representation. Change all 0s to 1s and all 1s to 0s. Inverting 0101 gives you 1010.

- Add 1: Finally, add 1 to the inverted binary number obtained in the previous step. In this case, 1010 + 0001 equals 1011.

So, the two's complement representation of -5 in an 8-bit system would be 11111011.

To verify, if you add -5 and 5 using binary addition: markdown

```
  11111011  (Two's complement of -5)
+ 00000101  (Positive binary of 5)
-----------
  00000000
```

You get zero, which indicates that the method is working correctly. Remember that the number of bits in the representation affects the range of values you can represent using two's complement. An 8-bit representation can represent values from -128 to 127, for example.

- RISC-V doubleword can represent 0 to (2^64 - 1) unsigned numbers or positive numbers.
- For a 64bit binary number, the biggest positive number possible to represent is (2^63 - 1) and the smallest value is -2^63.
- If MSB is 0 then the binary number is unsigned and if the MSB is 1 then the binary number is signed number as we can already tell from the above example.   


LAB: Signed & Unsigned Integer's
=================================
We will write a C-program whcih finds the highest possible unsigned and signed number in 64 bit format.

```
#include <stdio.h>
#include <math.h>
int main () {
    unsigned long long int max = (unsigned long long int) (pow(2,64)-1);
    printf ("highest number represented by unsigned long long int is %llu\n",max);
    return 0;
 }   
```
Use the below command to compile the C program and to make the object file.
 ```
 riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o <filename>.o <filename>.c
 spike pk <filename>.o
```

---
![unsignedlab](https://github.com/akul-star/RISC-V/assets/75561390/2c8d8334-688e-4caf-9e1d-d2460149442e)

Note: **long long int** data type can store maximum of 64 bits.

Now we will modify our C-program to give highest and lowest possible number for a signed number.

```
#include <stdio.h>
#include <math.h>
int main () {
    long long int max = (long long int) (pow(2,63)-1);
    long long int min = (long long int) (pow(2,63)*-1);
    printf ("highest number represented by unsigned long long int is %lld\n",max);
    printf ("lowest number represented by unsigned long long int is %lld\n",min);
    return 0;
 }   

</details>
```
The program calculates and displays the highest and lowest values representable by the long long int data type. It uses bitwise left-shifting for accuracy in computing these values. The calculated maximum is obtained by shifting the bit 1 to the left by 63 positions and subtracting 1, while the minimum is the negative of the shifted bit pattern. The program then prints these values using formatted output.

---
![Signedmaxmin](https://github.com/akul-star/RISC-V/assets/75561390/08f170b4-3dad-4fbb-82b3-2b523667655a)


 ## DAY-2: Introduction to ABI and Basic Verification Flow
 
 Application Binary Interface
 =============================

When an application wants to perform a task that requires interaction with the operating system, it makes a system call to request the corresponding service. The operating system's kernel then handles the request and performs the requested operation on behalf of the application. Examples of common system calls include opening or closing files, reading or writing data, creating new processes, allocating and freeing memory, and managing input/output devices.

- User ISA (Instruction Set Architecture): This is the set of instructions visible to application programmers and software developers. It defines the operations and data manipulation capabilities that application-level programs can use. User ISA provides a higher-level abstraction, allowing programmers to write software without needing to understand the underlying hardware details. Common examples of user-level instructions include arithmetic operations, memory access, branching, and more. Different processors or CPUs from various manufacturers might have different user ISAs, which can affect the compatibility of software across different systems.

- System ISA (Instruction Set Architecture): Also known as the "privileged ISA" or "machine ISA," this is the set of instructions used by the operating system and system-level software to control and manage the hardware resources of the computer. System ISA instructions are generally more powerful and privileged than user-level instructions. They enable actions such as controlling memory protection, managing interrupts, handling I/O operations, and other low-level system management tasks. Access to system ISA instructions is typically restricted to the operating system kernel or other trusted system components.

ABI (Application Binary Interface) with respect to system calls defines the standardized rules and conventions for how user-level applications interact with the operating system's kernel through binary-level communication. It encompasses details like how arguments are passed, system call numbers are identified, registers are used for communication, return values are retrieved, and errors are managed when making system calls. The ABI ensures consistent and reliable communication between user code and the kernel, abstracting the underlying hardware complexities and promoting compatibility across different software components and versions.

In the end, we can say that if the application programmer wants to access the hardware resources of your processor, then it has to do it via registers. We need to understand the architecture of the registers provided by the RISC-V specifications.

Memory Allocation for Doublewords
=================================
The RISC-V architecture has only 32 registers with a width of either 32 bit or 64 bit. 

---
![RISCVreg](https://github.com/akul-star/RISC-V/assets/75561390/073291f8-2a22-4eae-b2c6-6c73c865d83a)

- **XLEN:** The XLEN value specifies the bit width of the integer registers, which in turn determines the maximum size of integer data that the processor can handle natively.
- In a RISC-V processor with XLEN set to 32, the integer registers would be 32 bits wide, and the processor could perform arithmetic and logical operations on 32-bit integers in a single instruction.
- In a RISC-V processor with XLEN set to 64, the integer registers would be 64 bits wide, allowing the processor to handle 64-bit integers in a single instruction.

Now let us assume a XLEN 64 bit register. Their are two ways to load a doubleword data into the registers. 

1. Directly loading the 64 bit data to the regsiter of RISC-V.
2. Using memory registers of 8bit length, we can load 64 bit data using 8 memory registers. Each memory register is assigned a byte address m[0], m[1], m[2], etc.
---
![doublewrdallocation](https://github.com/akul-star/RISC-V/assets/75561390/70497c4a-948c-49d9-bc79-2c9cd56ddd29)

From the figure, we can observe that the 64 bit data has been uploaded using the little-endian method. In a little-endian architecture, the least significant byte (LSB) of a multi-byte data item is stored at the lowest memory address, while the most significant byte (MSB) is stored at the highest memory address. RISC-V is a little-endian architecture, which means that when loading a 64-bit data value into registers, you need to consider the byte order in memory.

- Address of 1st doubleword = m[0]
- Address of 2nd doubleword = m[7]
- Address of 1st doubleword = m[15]
- Address of 1st doubleword = m[23]
And so on.....

Load Double word Instruction
===========================
  
Let's say we want to load m[16] to m[23] double word into the RISC-V register x8. If you want to access this data from the memory you need the first address of that particular memory. Because if you want to reach the memory m[16], you first need the addres of the particular memory. We will store the base address of array M into the rehister x23. (Assume base address (0)dec)
```
ld x8, 16(x23)
```
The assembly instruction ld x8, 16(x23) in RISC-V represents a load operation. Let's break down what each part of the instruction means:

- ld: This is the opcode mnemonic for the "Load Doubleword" instruction. It's used to load a 64-bit (8-byte) data value from memory into a register.

- x8: This is the destination register where the loaded 64-bit data value will be stored. In this case, the destination register is x8.

- 16(x23): This is the memory address where the data will be loaded from. It consists of two parts:
        - 16: This is the offset value. It specifies the distance, in bytes, from the address stored in register x23 to the memory location you want to load from.
        - (x23): This refers to the base address register. In this case, x23 holds the base memory address.

So, the instruction ld x8, 16(x23) means:

"Load a 64-bit data value from memory. Add an offset of 16 bytes to the memory address stored in register x23, and store the loaded value in register x8."

This instruction is used to load a 64-bit data value from memory into a register, and the effective memory address used for the load operation is calculated by adding the offset to the contents of register x23. The loaded data will be stored in register x8.

---
![loaddoubleword](https://github.com/akul-star/RISC-V/assets/75561390/4c02f38e-ff4c-453b-9dbf-e2127acd88e7)

Add Instruction
==============

```
add x8, x24,x8
```

The instruction add x8, x14, x8 in RISC-V assembly language represents an addition operation. Let's break down what each part of the instruction means:

-  add: This is the opcode mnemonic for the "Add" instruction. It's used to perform addition between two operands and store the result in a destination register.

- x8: This is the destination register where the result of the addition will be stored. In this case, the result will be stored in register x8.

- x14: This is the first source register. It contains one of the operands for the addition.

-  ,: This comma separates the source registers from the destination register in the instruction.

-  x8: This is the second source register. It contains the other operand for the addition.

So, the instruction add x8, x14, x8 means:

"Add the values in registers x14 and x8. Store the result in register x8."

In other words, the value currently stored in register x8 (the second source register) is added to the value in register x14 (the first source register), and the sum is stored back in register x8 (the destination register).

For example, if x14 contains the value 5 and x8 contains the value 10 before the instruction is executed, after executing the add instruction, the value in x8 will be updated to 15, which is the result of adding 5 and 10.

---
![add](https://github.com/akul-star/RISC-V/assets/75561390/bee538bf-e9b5-4d31-be50-2ccdfd8eb2ac)


Store Double-word
=================

```
sd x8, 8(x23)
```
The instruction sd x8, 8(x23) in RISC-V assembly language represents a store operation. Let's break down what each part of the instruction means:

-   sd: This is the opcode mnemonic for the "Store Doubleword" instruction. It's used to store a 64-bit (8-byte) data value from a register into memory.

-   x8: This is the source register containing the data value that you want to store. In this case, the data value to be stored is the contents of register x8.

-   8(x23): This is the memory address where you want to store the data. It consists of two parts:
        8: This is the offset value. It specifies the distance, in bytes, from the address stored in register x23 to the memory location where you want to store the data.
        (x23): This refers to the base address register. In this case, x23 holds the base memory address.

So, the instruction sd x8, 8(x23) means:

"Store the 64-bit data value from register x8 into memory. Add an offset of 8 bytes to the memory address stored in register x23, and write the contents of x8 to that memory location."

This instruction is used to store a 64-bit data value from a register into memory. The effective memory address used for the store operation is calculated by adding the offset to the contents of register x23, and the data value in x8 is written to that memory location.

---
![storedoubleword](https://github.com/akul-star/RISC-V/assets/75561390/67405938-a97f-4a71-aacd-e7f446a34bcc)

## References
