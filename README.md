# RISC-V CPU CORE

Follow this repo to build your RISC-V pipelined core, housing a RV64I instruction set. 

# Table of Contents
- [What is RISC-V ISA?](#what-is-risc-v-isa)
- [GNU compiler toolchain](#risc-v-gnu-compiler-toolchain)
- [Introduction to ABI](#introduction-to-abi)
- [Digital Logic with TL-Verilog and Makerchip](#digital-logic-with-tl-verilog-and-makerchip)
  - [Combinational logic](#combinational-logic)
  - [Sequential logic](#sequential-logic)
  - [Pipelined logic](#pipelined-logic)
  - [Validity](#validity)
- [Basic RISC-V CPU micro-architecture](#basic-risc-v-cpu-micro-architecture)
  - [Fetch](#fetch)
  - [Decode](#decode)
  - [Register File Read and Write](#register-file-read-and-write)
  - [Execute](#execute)
  - [Control Logic](#control-logic)
- [Pipelined RISC-V CPU](#pipelined-risc-v-cpu)
  - [Pipelinig the CPU](#pipelining-the-cpu)
  - [Load and store instructions and memory](#load-and-store-instructions-and-memory)
  - [Completing the RISC-V CPU](#completing-the-risc-v-cpu)
- [Acknowledgements](#acknowledgements)

# What is RISC-V ISA?

- RISC-V is an open standard instruction set architecture (ISA) based on established reduced instruction set computer (RISC) principles. Unlike most other ISA designs, the RISC-V ISA is provided under open source licenses that do not require fees to use.
- Because the ISA is open, it is the equivalent of everyone having a micro architecture license. One can optimize designs for lower power, performance, security, etc.
- RISC-V is much more than an open ISA, it is also a frozen ISA. The base instructions are frozen and optional extensions which have been approved are also frozen.

# RISC-V GNU Compiler Toolchain

- This is the RISC-V C and C++ cross-compiler. It supports two build modes:
a generic ELF/Newlib toolchain and a more sophisticated Linux-ELF/glibc
toolchain.
- Installation (UBUNTU) : 
    
    * `$ sudo apt-get install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev`
    * `$ git clone https://github.com/riscv/riscv-gnu-toolchain`
- Configuration 

     Pick an install path.  If you choose, say, `/opt/riscv`, then add `/opt/riscv/bin` to your `PATH` now. Then, simply run the following command:
### OPTIONS: 
    1. (Newlib)

        `./configure --prefix=/opt/riscv`
        
         `make`

       You should now be able to use riscv64-unknown-elf-gcc and its cousins.
    2. (LINUX)
    
       RV64GC (64-bit):
       `./configure --prefix=/opt/riscv`
       `make linux`
       
       32-bit RV32GC : 
       `./configure --prefix=/opt/riscv --with-arch=rv32gc --with-abi=ilp32d`
       `make linux`
    3. (MULTILIB)
    
      `./configure --prefix=/opt/riscv --enable-multilib`
      `make`   (FOR NEWLIB)
      `make linux` (FOR LINUX)
      
      The multilib compiler will have the prefix riscv64-unknown-elf- or riscv64-unknown-linux-gnu-, but will be able to target both 32-bit and 64-bit systems. It will support the most common `-march`/`-mabi` options, which can be seen by using the `--print-multi-lib` flag on either cross-compiler.
       
### COMMANDS

    Under the risc-v toolchain, 
  * Compile:

    `riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o <object filename> <C filename>`

    Compile with options:

    `riscv64-unknown-elf-gcc <compiler option -O1 ; Ofast> <ABI specifier -lp64; -lp32; -ilp32> <architecture specifier -RV64 ; RV32> -o <object filename> <C      filename>`

    [All Options](https://www.sifive.com/blog/all-aboard-part-1-compiler-args)

  * See Assembly
    
    `riscv64-unknown-elf-objdump -d <object filename>`
    
  * Run code
  
    `spike pk <object filename>`
    
    Debug code
    
    `spike -d pk <object Filename>` with degub command as `until pc 0 <pc of your choice>`

