``# RISC-V INSTRUCTION SET ARCHITECTURE (ISA)
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
- Day-0: 
- Day-1:



## DAY-0: Installation of the Tools

<details>
  <summary>Tool Installation</summary>

---
- Install the dependencies using the following command :

```
sudo apt-get install libboost-regex-dev
```
- Steps to install the toolchain

```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh
./run.sh
```

- Running this command will result in a make error. Ignore the error and follow the steps given below:

```
cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install 
```

- Once the toolchain is installed it is necessary to create a PATH variable in bashrc file. To create the path variable follow the steps given below :

```
gedit .bashrc


#Type at last line
export PATH="/home/akul-sinha/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH" 

# close the bashrc and type in terminal
source .bashrc
```
</details>


## DAY-1: Introduction to RISC-V ISA and GNU compiler toolchain

<details> 
  <summary>INTRODUCTION</summary>
  
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

</details>

 ## DAY-2: Introduction to ABI and Basic Verification Flow


<details> 
<summary> Application Binary Interface </summary>

---
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

- add: This is the opcode mnemonic for the "Add" instruction. It's used to perform addition between two operands and store the result in a destination register.

- x8: This is the destination register where the result of the addition will be stored. In this case, the result will be stored in register x8.

- x14: This is the first source register. It contains one of the operands for the addition.

- ,: This comma separates the source registers from the destination register in the instruction.

- x8: This is the second source register. It contains the other operand for the addition.

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

32 Registers & their ABI names
=============================
The instructions we have been operating on signed and unsigned integers are called as Base Integer Instructions (RV64I). So, any CPU core which wants to implement these instructions are called as RV64I CPU core and will need to implement atleast the 47 base integer instructions out of which 3 we have already observed.

In the RISC-V instruction set architecture (ISA), instructions are categorized into several types based on their functionality and operation. The following are the common types of instructions in RISC-V:

1. R-Type Instructions (Register-Type):
These instructions operate on registers and typically perform arithmetic, logical, or bitwise operations. They take three source registers as operands and store the result in a destination register. Example: add, sub, and, or, xor.

2. I-Type Instructions (Immediate-Type):
These instructions operate on an immediate value (a constant) and a register. They perform operations like adding an immediate value to a register, loading immediate values into registers, and branching. Example: addi, lw, sw, beq.

3. S-Type Instructions (Store-Type):
   These instructions store a value from a register into memory at a specified address. They take an immediate offset and two registers (one source and one base) as operands. Example: sb, sh, sw.

---
![riscvABIname](https://github.com/akul-star/RISC-V/assets/75561390/56f550e0-4dac-4805-9f36-aaed77e054ce)

ABI names refer to the Application Binary Interface names used in the context of the RISC-V architecture. The RISC-V ABI defines conventions and rules that govern how functions are called and how data is passed between different parts of a software system, such as between different parts of a program or between a program and its libraries.


</details>

<details> 
<summary> Lab work using ABI function calls </summary>

---
In past LAB sessions, we wrote a C-program to add number from 1 to n. We will modify the C-program to make some function call's to the assembly language in the RISC-V ISA  and then do some computation and finally send the final results back to the main program.

---
![ABIcprogramalgo](https://github.com/akul-star/RISC-V/assets/75561390/24d5dc63-2c19-4eec-8520-9dd1fbc4681a)

- Modified C-program for the summation of 1 to n.

```
  #include <stdio.h>
extern int load(int x,int y);
int main()
 {
 	int result = 0;
	int count =9;
 	result = load(0x0,count+1);
 	printf("Sum of numbers from 1 to %d is %d\n",count,result);
 } 
   
```

- Code for the load file. It is saved as load.S with an extension of **.S**. A **.s** file is a text file that contains assembly language source code. Assembly language is a low-level programming language that is closely related to the machine code instructions executed by a computer's CPU. Each line of an assembly language program typically corresponds to a single machine code instruction.

```
.section .text
.global load
.type load, @function

load: 
     add   a4,a0,zero    //initialize sum register a4 with 0x0
     add   a2,a0,a1      //store count of 10 in reg a. reg a1 is loaded with 0xa(decimal 10) from main
     add   a3,a0,zero    //initialize intermediate sum reg a3 by 0x0

loop:
 add   a4,a3,a4     // Incremental addition
     addi  a3,a3,1      // Increment intermediate register by 1
     blt   a3,a2,loop   // If a3 is less than a2,branch to label <loop> 
     add   a0,a4,zero   // store final result to reg a0 so that it can be read by main pgm
     ret
```
Use the following command to compile the C-Program, make the object file and to observe the output.
```
cd ~/RISCV-ISA/riscv_isa_labs/day_2/lab1/
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o custom1_to9.o custom1_to_9.c load.S
riscv64-unknown-elf-objdump -d custom1_to9.o | less
spike pk custom1_to9.o
```
- Output will be as shown below.

---
![1to9_custom_output](https://github.com/akul-star/RISC-V/assets/75561390/c76ed767-7772-48b6-b045-1bd498e99cbe)  

- The disassembled code will look like as shown below.

--- 
![1to9_custom_assemblyinstructions](https://github.com/akul-star/RISC-V/assets/75561390/66a38bcd-54f4-46e9-877f-700e98fde4d0) 

</details>

<details>
<summary>Basic Verification Flow using iverilog</summary>

---
Now we will learn how to run the same C-Program on the RISC-V CPU. The idea is to have this this RISC-V CPU to do labs written in verilog. We will write Testbench and load the Hex file of the C-program and load it in the memory. The CPU core will process the contents of the memory and display the output. For this LAB, we will use a RISC-V CPU core known as PicoRV32. PicoRV32 is an open-source RISC-V CPU core that is designed to be small, simple, and easily synthesizable for FPGA (Field-Programmable Gate Array) implementations. It is an implementation of the RISC-V instruction set architecture (ISA) and is often used as a building block for creating custom RISC-V-based processors in FPGA projects.

---
![LAB-Risc_VCPUcore](https://github.com/akul-star/RISC-V/assets/75561390/7025f67c-35fd-4854-8739-663d3d0347b1)

Let's download the PicoRV32 CPU core on which we will do our experiments through. 

```
git clone https://github.com/kunalg123/riscv-workshop-collaterals.git
```
In this lab, we are basically going to generate a hex file and a bitstream of the same code done above. We run the below code to generate the same. For the demo, go to the lab directory using the command givem below.
```
cd ~/riscv_workshop_collaterals/labs/
chmod 777 rv32im.sh
./rv32im.sh  # Contains necessary commands to convert C to hex
```

---
![contents](https://github.com/akul-star/RISC-V/assets/75561390/f873dfd4-1b0f-4748-808a-b6f8b104497a)
The file firmware.hex is the hex file and firmware32.hex is the bitstream generated.
The below file is firmware.hex.

---

![firmwareghex](https://github.com/akul-star/RISC-V/assets/75561390/08e20701-a1d9-4506-86ab-dfe16d30e57f)

</details>


## Day-3: Digital Logic with TL Logic & Makerchip

<details>
  <summary> Introduction </summary>

---
In this part of the workshop, we are going to look at:

- Logic gates
- MakerChip platform(IDE)
- Combinational Logic
- Sequential Logic
- Piplining logic
- Slate

Logic Gates
===========
Logic gates are fundamental building blocks of digital circuits that perform logical operations on one or more binary inputs and produce a single binary output. These gates form the basis of digital electronics and are used to design complex digital circuits, such as CPUs, memory units, and more. There are several basic types of logic gates, each with its own truth table describing its behavior.

--- 
![logicgates](https://github.com/akul-star/RISC-V/assets/75561390/78176b8d-7499-4673-80fd-4229ee92568c)

These basic logic gates can be combined in various ways to create more complex logic functions. In digital circuit design, logic gates are used to perform operations and implement functions that process binary signals, which form the basis of digital computing and modern electronics.

Boolean Operators
==================

Boolean operators, also known as logical operators, are symbols used in Boolean algebra and programming to combine and manipulate logical values (true or false). These operators allow you to create more complex logical expressions by combining simpler ones. There are three primary Boolean operators: AND, OR, and NOT. 

---
![boolean ](https://github.com/akul-star/RISC-V/assets/75561390/df8add98-fb6e-4aca-9755-04096abaad22)
These Boolean operators are fundamental tools for creating logical expressions and controlling the flow of programs in programming languages. They are used extensively in conditional statements (if, else, switch), loop conditions (while, for), and more. By combining these operators, you can create complex logical expressions that allow your code to make decisions and perform actions based on different conditions.

</details>

<details>
	<summary> Basic Mux Implementation And Introduction To Makerchip </summary>

---
[Makerchip](https://makerchip.com/): Makerchip is an online platform that provides an integrated development environment (IDE) for digital design and verification using SystemVerilog and TL Verilog. It allows engineers, students, and enthusiasts to design and simulate digital circuits, develop RTL (Register Transfer Level) code, and explore hardware design concepts without requiring the local installation of tools. TL-Verilog was used as the HDL of choice for this project. Projects on Makerchip can be completely designed using TL-Verilog. Transaction Level - Verilog standard is an extension of Verilog which has various advantages like simpler syntax, shorter codes and easy pipelining.

TL-Verilog: Transaction-Level Verilog (TL Verilog) is an extension of traditional Verilog, which is a hardware description language used for designing digital systems and circuits. TL Verilog was developed to make hardware design more approachable and efficient, especially for complex and high-level designs. We will be using TL-verilog to design circuits on the Makerchip IDE.


Implementing MUX using [Makerchip](https://makerchip.com/)
==========================================================
- Different ways to represent Mux is given below.

---
![mux0](https://github.com/akul-star/RISC-V/assets/75561390/4b2527a3-443a-43cf-9665-cda35d7e781e)

- The verilog syntax can simply be given as,
```
assign f= s ? X1 : X2;
```
- A 4x1 MUX given below.

  ![MUX1](https://github.com/akul-star/RISC-V/assets/75561390/af6e81f8-68d5-4631-ba5c-f69ca84f438a)

- The verilog syntax can be given as,
  ```
  assign f=
     sel[0]
        ?a:
     sel[1]
        ?b:
     sel[2]
        ?c:
  //default 
         d:
   )
  );

  ```

LAB: Makerchip Platform
=======================

---
**1. Loading Pythagorean Example on Makerchip IDE:**

![Pythagorean_0](https://github.com/akul-star/RISC-V/assets/75561390/b2b26e89-aa7d-4edf-9c52-9775f80e94c2)
![Pythagorean](https://github.com/akul-star/RISC-V/assets/75561390/7fad9307-1eae-4213-87fb-9a4fd84fab21)
    

</details>


<details>

<summary>Combinational Circuits</summary>

---
In this section we will try to implement basic devices like inverter, GATE's, etc, on the Makerchip IDE.

**2. INVERTER**
The TL-verilog code for Inverter is shown below:
```
$in = !$out
```
---
![TLV-inverter](https://github.com/akul-star/RISC-V/assets/75561390/ffe2e7be-8c48-4caf-81c7-f604f2e800ff)

**3. Vectors Addition**

Note that the bit ranhes can generally be assumed on the left-hand side, but with no assignments to these signals, they must be explicit. 
---
![vector](https://github.com/akul-star/RISC-V/assets/75561390/e31cde5d-c028-47d4-86fa-50dd94877cd3)

**4. Multipltxer**

TL-verilog code for Mux is given below,
```
$out = $sel ? $in1 : $in2
```
![Makerchip_mux](https://github.com/akul-star/RISC-V/assets/75561390/7581d4ae-bc88-499a-bbd3-ede8dc8bd463)

**5. Combinational Calculator**
This circuit implements a calculator that can perform addition, substraction, multiplication and divion on two inputs (32bits each). Encoded select will select the operation we want as an output between two numbers.

---
![calculatorckt](https://github.com/akul-star/RISC-V/assets/75561390/caf501de-2cef-4ab8-a5a3-c45f57961031)
 
 The code for the combinational calculator using multiplexer is given below.
```
 $reset = *reset;
   $op[1:0] = $random[1:0];
   
   $val1[31:0] = $rand1[3:0];
   $val2[31:0] = $rand2[3:0];
   $sum[31:0] = $val1+$val2;
   $diff[31:0] = $val1-$val2;
   $prod[31:0] = $val1*$val2;
   $div[31:0] = $val1/$val2;
```
![combcal_makerchip](https://github.com/akul-star/RISC-V/assets/75561390/7ac262bd-5aa5-4c84-b38b-dbf638d02fe9)


</details>

<details> 
<summary>Sequential Circuits</summary>
---
D-Flipflop: A D flip-flop, also known as a Data or Delay flip-flop, is a fundamental digital electronic circuit component used in sequential logic circuits. It's a type of bistable multivibrator, which means it has two stable states and can store a single bit of data. The primary purpose of a D flip-flop is to store and synchronize data in digital systems.

 Fibonacci Series
 ================
 The Fibonacci series is a sequence of numbers in which each number is the sum of the two preceding ones. The series starts with 0 and 1, and each subsequent number is the sum of the previous two. Mathematically, the Fibonacci series can be defined as:

F(0) = 0
F(1) = 1
F(n) = F(n-1) + F(n-2) for n > 1

So, the first few terms of the Fibonacci series are: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, and so on.

Here's how the series is built:

- The first term is 0.
- The second term is 1.
- The third term is the sum of the first and second terms (0 + 1 = 1).
- The fourth term is the sum of the second and third terms (1 + 1 = 2).
- The fifth term is the sum of the third and fourth terms (1 + 2 = 3).
    And so on...

Implementing a Fibonacci series using D flip-flops involves using two flip-flops to store the current and previous terms of the series. Here's a brief outline of the process:

- Initialize two D flip-flops, one for the "Current" term and one for the "Previous" term.
- Set the initial values: Current = 0, Previous = 1.
- Connect the D input of the "Current" flip-flop to the Q output of the "Previous" flip-flop.
- Use logic gates (e.g., XOR gate) to add the "Current" and "Previous" terms and store the result in another flip-flop temporarily.
- Connect the output of the temporary flip-flop to the D input of the "Current" flip-flop to update it.
- Update the "Previous" flip-flop with the previous value of the "Current" flip-flop.
- Repeat steps 4 to 6 for each term in the series.

This arrangement utilizes flip-flops to store and update the terms of the Fibonacci series, and logic gates to perform the addition operation. The clock input of the flip-flops ensures synchronous updating.


</details>


## References

- https://github.com/Shant1R/RISC-V.git
- https://makerchip.com/
- https://riscv.org/
- https://github.com/mrdunker/RISC-V_based_MYTH_IIITB.git
- https://inst.eecs.berkeley.edu/
- https://github.com/riscv/riscv-gnu-toolchain
- https://github.com/alwinshaju08/RISCV
