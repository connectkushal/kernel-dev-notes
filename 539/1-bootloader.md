
#### Introduction and x86 assembly language overview

- **Boot loader**: code that is responsible for loading the main kernel from the disk to the main memory.
- A basic knowledge of the target architecture assembly is required to build a kernel, which in our case x86.
- An assembler is required to to convert assembly language code to machine language.
- We will be using Netwide Assembler (NASM) ^[GNU Assembler (GAS) is another alternative]
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
- The processor's architecture provides these instructions. ^[ Manual for available instructions on x86: [Intel® 64 and IA-32 architectures software developer's manual](https://software.intel.com/en-us/articles/intel-sdm)]
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
- It is an open-source assembler for x86 architecture which uses Intel's syntax of assembly language. ^[ Other well-known syntax for assembly language is AT&T syntax]
- Basic usage of NASM command is 
   `nasm -f <format> <filename> [-o <output>]`
  - This command generates the corresponding machine code from the x86 assembly code file. ^[The process of transforming an assembly source code to machine code is known as assembling]
  - The argument `<format>` decides the binary format of the generated machine code (will be discussed later).
  - The argument `<filename`> is the name of the assembly file that we would like to assemble
  - The last option and argument are optional, they are used for naming the output file. The default name is 'filename'.

/* WIP
- Example assembly code:
  ```
    mov ax, 0Eh
    mov al, 's'
    int 10h
  ```
   - 0Eh hexadecimal
   - Single quotation is used in NASM to represent strings or characters.

*/

---
#### Binary Format

- Each executable file uses some binary format to organize its content.
- A binary format describes the structure of the binary file, i.e how multiple parts in it are organised.
- Each OS has its own binary format. 
  - eg ELF is the default in Linux.
  - Mach-O is used by Mach-based. ^[Mach is an operating system's kernel which is well-known for using microkernel design. It has been started as a research effort in Carnegie Mellon University in 1985. Current Apple's operating systems macOS and iOS are both based on an older operating system known as NeXTSTEP which used Mach as its kernel]
  - Portable Executable (PE) used by Microsoft Windows.
- All programming languages compile to produce an executable formatted with the same binary format.
  - For example in Linux, if we create a software either by C, Go, Rust or assembly, the last executable result will be a binary file that is formatted by using **Executable and Linkable Format (ELF)** binary format. 
  - By using the specification of ELF, the Linux kernel will be able to locate the machine code of the software inside the ELF file and load it into memory and prepare it for execution.

---
#### GNU Make
- GNU Make is a build automation tool. 
- It automates the building process of the binary file.
- The steps include compiling and assembling the source code, then linking the generated object files to produce the binary file.
  - An object file is a machine code of a source file and it is generated by the compiler.
- All the required commands are gathered in a text file known as **Makefile**
  - This file is used with command `make`
- GNU Make is going to run commands in the file sequentially.
- it also checks whether a code file is modified since the last building process.
  - If the file is not modified then the generated object file from the last building process is used.
- There is a specific syntax followed when writing makefile.
- It has a list of _rules_ that define how to create the executable file.
- Each rule has the following format:
  ```
  target: prerequisites
  recipe
  ```
  - The name of a target can be a general name or filename.
  - The prerequisites part of a rule is sort of a list of dependencies of this rule.
    -  these dependencies can be either filenames (the C files of the source code for instance) or other rules in the same makefile.
  -  The recipe part contains the commands that are going to run when the rule is being executed.
      - Each line here should start with a tab.
      - Any arbitrary linux commands can be used in the recipe.
  - makefile can have variables which can defined as: `foo = bar` and can be used in the rules as: `$(foo)`.
- `make <target_name>` executes the target rule.
  - if the rule has a dependency, which is another rule, that rule should be executed successfully first.
  - if there is a filename in the dependencies list and there is no rule that has the same filename as a target name, then this file will be checked and used in the recipe of the rule.
- When running `make` without arguments, the first rule in the makefile which does NOT start with dot (eg .xyz) is run.
- One of well-known convention is to define a rule with target name _clean_  which deletes all object files and binaries that have been created in the last building process.
- Consider the following C source files:
  - file1.c
    ```
    #include "file2.h"
    int main()
    {
      func();
    }
    ```
  - file2.h
     ```
    void func();
    ```
  - file2.c
    ```
    #include <stdio.h>
    void func()
    {
      printf( "Hello World!" );
    }
    ```
- By using these three files, let's take an example of a makefile with filenames that have no rules with same target's name.
  ```
  build: file1.c file2.c
    gcc -o ex_file file1.c file2.c
  ```
  - The target name of this rule is build.
  - To execute this rule we can use either `make build` or simply `make` (as it is the first rule and doesnt start with dot).
  - The rule *build* depends on two C files, file1.c and file2.c, they should be available on the same directory. 
  - The *recipe* uses GNU GCC to compile and link these two files and generate an executable file named *ex_file*.
- The following is an example of a makefile that has multiple rules.
  ```
  build: file2.o file1.o
    gcc -o ex_file file1.o file2.o
  file1.o: file1.c
    gcc -c file1.c
  file2.o: file2.c file2.h
    gcc -c file2.c file2.h
  ```
    - The first rule *build* depends on two object files file1.o and file2.o.
      - Before running the building process for the first time, these two files will not be available in the source code directory because they are a result of the compiling step that has not been performed yet. So, we have defined a rule for each one of them. 
    - The rule file1.o is going to generate the object file file1.o and it depends on file1.c, the object file will be simple generated by compiling file1.c.
    - The same happens with file2.o but this rule depends on two files instead of only one.

- We can redefine the second makefile by using the variables.
  ```
  c_compiler = gcc
  buid_dependencies = file1.o file2.o
  file1_dependencies = file1.c
  file2_dependencies = file2.c file2.h
  bin_filename = ex_file
  build: $(buid_dependencies)
    $(c_compiler) -o $(bin_filename) $(buid_dependencies)
  file1.o: $(file1_dependencies)
    gcc -c $(file1_dependencies)
  file2.o: $(file2_dependencies)
    gcc -c $(file2_dependencies)
  ```

---
*Additional Notes*
- Pentium 4 for instance supports 32 bit architecture, Intel 8086 does not, as it is a 16bit architecture.
- See study material list for x86 architecture tutorials

