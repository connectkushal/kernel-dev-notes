
#### Introduction and x86 assembly language overview

- **Boot loader**: code that is responsible for loading the main kernel from the disk to the main memory.
- A basic knowledge of the target architecture assembly is required to build a kernel, which in our case x86.
- An assembler is required to to convert assembly language code to machine language.
- We will be using Netwide Assembler (NASM)
---
#### Registers
- In any processor architecture, a **register** is a small memory inside the processor's chip.
- In x86 there are two types of registers: general purpose registers and special purpose registers.
  - The general purpose registers can store any kind of data. 
  - The special purpose registers are provided by the architecture for some specific purposes (to be covered later).
- x86 provides us with **8 general purpose registers** 
  - To read from or write to these we refer to them by their names in assembly code.
  - The names of these registers are: EAX, EBX, ECX, EDX, ESI, EDI, EBP, and ESP.
  - // **(ABCD-X, SD-I, BS-P) (B-XP, D-XI, S-PI)**
  -    The size of each is **32 bits** (**4 bytes**)
  -    E means 'extended'
  -    the register name without the E, for eg AX, can be used to access the first 16 bits of the register (this naming convention supports legacy 16bit architectures)
  - The first 16 bits of these registers can be divided into two parts and each one of them is of size 8 bits (1 bytes) 
    - These parts have their own name that can be referred to in the assembly code: The first 8 bits: **low bits**, the second 8 bits: **high bits**.


- **NOTE**: While the registers **ESI, EDI, EBP and ESP** are considered to be general purpose registers in x86 (According to Intel's manual), they store some important data in some cases and it's better to use them carefully when we need to.

---
#### Instruction Set
- An assembly code is simply a set of instructions which will be **executed sequentially**.
- The processor's architecture provides these instructions.
- Processor is the ultimate library for instructions, just like functions are provided by libraries in languages like C, Go
- An Instruction 
  - has a name,
  - performs a specific job,
  - can take parameters, which are called **operands**.
- Depending on the instruction itself, the operands can be 
    - a static value (e.g. a number),
    - a register name that the instruction is going to fetch the stored value from to be used by it,
    - a memory location.

---
#### Nasm
- It is an open-source assembler for x86 architecture which uses Intel's syntax of assembly language.
- Basic usage of NASM command is 
  - `nasm -f <format> <filename> [-o <output>]`
  - This command generates the corresponding machine code from the x86 assembly code file.
  - The argument `<format>` decides the binary format of the generated machine code (will be discussed later).
  - The argument `<filename`> is the name of the assembly file that we would like to assemble
  - The last option and argument are optional, they are used for naming the output file. The default name is 'filename'.

/* WIP
- Example assembly code:
    - ```
        mov ax, 0Eh
        mov al, 's'
        int 10h
      ```
   - 0Eh hexadecimal
   - Single quotation is used in NASM to represent strings or characters.

*/

---
#### Binary Format
- Each OS has its own binary format.
- Each executable file uses some binary format to organize its content and to make a specific operating system understands its content.
- A binary format describes how a binary file is structured. 
  - In other words, it is basically a specification which gives a blueprint of how a binary file is organized.
- In general there are multiple parts in a binary file and a binary format can be used format them
- There is no difference between the programming languages that compile to an executable, in the matter of the binary format that will be used in the last output of the compiling process
  - For example in Linux, if we create a software either by C, Go, Rust or assembly, the last executable result will be a binary file that is formatted by using **Executable and Linkable Format (ELF)** binary format. 
  - ELF is the default in Linux.
  - the machine code is one part of a binary file parts. 
  - By using the specification of ELF, the Linux kernel will be able to locate the machine code of the software inside the ELF file and load it into memory and prepare it for execution.

---
*Additional Notes*
1) GNU Assembler (GAS) is another alternative
2) Pentium 4 for instance supports 32 bit architecture, Intel 8086 does not, as it is a 16bit architecture.
3) See study material list for x86 architecture tutorials
4) Manual for available instructions on x86: [Intel® 64 and IA-32 architectures software developer's manual](https://software.intel.com/en-us/articles/intel-sdm)
5) Other well-known syntax for assembly language is AT&T syntax
6) The process of transforming an assembly source code to machine code is known as assembling
7) Other binary formats are Mach-O used by Mach-based, Portable Executable (PE) used by Microsoft Windows.
8) Mach is an operating system's kernel which is well-known for using microkernel design. It has been started as a research effort in Carnegie Mellon University in 1985. Current Apple's operating systems macOS and iOS are both based on an older operating system known as NeXTSTEP which used Mach as its kernel
