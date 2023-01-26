
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


- **NOTE**: While the registers **ESI, EDI, EBP and ESP** are considered as general purpose registers in x86(According to Intel's manual), they store some important data in some cases and it's better to use them carefully if we are forced to.

---
*Additional Notes*
1) GNU Assembler(GAS) is another alternative
2) Pentium 4 for instance supports 32 bit architecture, Intel 8086 does not, as it is a 16bit architecture.
3) See study material list for x86 architecture tutorials
