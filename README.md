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
![spikedebug](https://github.com/akul-star/RISC-V/assets/75561390/7837e4bc-927e-4105-b404-e2e24f5678b1)
![Assemblylan](https://github.com/akul-star/RISC-V/assets/75561390/3a60c2c0-0024-4c11-ad06-60b3a8be3c0b)


</details>

## References
