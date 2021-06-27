# RISC-V CPU CORE

Follow this repo to build your RISC-V pipelined core, housing a RV64I instruction set. 

# Table of Contents
- [What is RISC-V ISA?](#what-is-risc-v-isa)
- [GNU compiler toolchain](#risc-v-gnu-compiler-toolchain)
- [Application Binary Interface](#application-binary-interface)
- [TL-Verilog](#tl-verilog)
- [Makerchip](#makerchip)
  - [Combinational logic](#combinational-logic)
  - [Sequential logic](#sequential-logic)
  - [Pipelined logic](#pipelined-logic)
  - [Validity](#validity)
- [RISC-V CPU Architecture](#risc-v-cpu-architecture)
  - [Fetch](#fetch)
  - [Decode](#decode)
  - [Execute](#execute)
  - [Control Logic](#control-logic)
  - [Base CPU](#base-cpu)
- [Pipelined RISC-V CPU](#pipelined-risc-v-cpu)
  - [Jump](#jump)
  - [Completed Pipelined RISC-V CPU](#completed-pipelined-risc-v-cpu)
- [Acknowledgements](#acknowledgements)

# What is RISC-V ISA?

- RISC-V is an open standard instruction set architecture (ISA) based on established reduced instruction set computer (RISC) principles. Unlike most other ISA designs, the RISC-V ISA is provided under open source licenses that do not require fees to use.
- Because the ISA is open, it is the equivalent of everyone having a micro architecture license. One can optimize designs for lower power, performance, security, etc.
- RISC-V is much more than an open ISA, it is also a frozen ISA. The base instructions are frozen and optional extensions which have been approved are also frozen.

# RISC-V GNU Compiler Toolchain

- This is the RISC-V C and C++ cross-compiler. It supports two build modes:
a generic ELF/Newlib toolchain and a more sophisticated Linux-ELF/glibc
toolchain.
- **Installation (UBUNTU) :** 
    
    * `$ sudo apt-get install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev`
    * `$ git clone https://github.com/riscv/riscv-gnu-toolchain`
- **Configuration** 

     Pick an install path.  If you choose, say, `/opt/riscv`, then add `/opt/riscv/bin` to your `PATH` now. Then, simply run the following command:
  #### OPTIONS: 
    1. **(Newlib)**

        `./configure --prefix=/opt/riscv`
        
         `make`

       You should now be able to use riscv64-unknown-elf-gcc and its cousins.
    2. **(LINUX)**
    
       *RV64GC (64-bit):*
       `./configure --prefix=/opt/riscv`
       `make linux`
       
       *32-bit RV32GC :* 
       `./configure --prefix=/opt/riscv --with-arch=rv32gc --with-abi=ilp32d`
       `make linux`
    3. **(MULTILIB)**
    
      `./configure --prefix=/opt/riscv --enable-multilib`
      `make`   (FOR NEWLIB)
      `make linux` (FOR LINUX)
      
      The multilib compiler will have the prefix riscv64-unknown-elf- or riscv64-unknown-linux-gnu-, but will be able to target both 32-bit and 64-bit systems. It will support the most common `-march`/`-mabi` options, which can be seen by using the `--print-multi-lib` flag on either cross-compiler.
       
#### COMMANDS

    Under the risc-v toolchain, 
  * **Compile:**

    `riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o <filename.o> <filename.c>`

   **Compile with options:**

    `riscv64-unknown-elf-gcc <compiler option -O1 ; Ofast> <ABI specifier -lp64; -lp32; -ilp32> <architecture specifier -RV64 ; RV32> -o <filename.o> <filename.c>`

    [All Options](https://www.sifive.com/blog/all-aboard-part-1-compiler-args)

  * **See Assembly**
    
    `riscv64-unknown-elf-objdump -d <object filename>`
    
  * **Run code**
  
    `spike pk <filename.o>`
    
  * **Debug code**
    
    `spike -d pk <filename.o>` with degub command as `until pc 0 <pc number>`

### Running the 1to9_custom.c and load.S on the riscv GNU toolchain

* First, we ran the program using riscv64-unknown-elf-gcc command.
* Then, debugged the Program using spike -d pk 1to9_custom.o to check for values of registers at specific addresses as asked
* Finally we took a look into the assembly code to check the address locations of the load and theloop blocks.

### 1) Program Run and Debug

---
![gnu](https://github.com/ninja3011/riscv-cpu-core/blob/master/Day2/prog_run_debug.PNG)
### 2) Debug

---
![gnu](https://github.com/ninja3011/riscv-cpu-core/blob/master/Day2/debug.PNG)
### 3) Assembly code

---
![gnu](https://github.com/ninja3011/riscv-cpu-core/blob/master/Day2/assembly.PNG)

# Application Binary Interface

- Application binary interface (ABI) is an interface between two binary program modules. Often, one of these modules is a library or operating system facility, and the other is a program that is being run by a user.
- ABI defines how your code is stored inside the library file, so that any program using your library can locate the desired function and execute it. ABIs are important when it comes to applications that use external libraries.


### Running the 1to9_custom.c and load.S on the picorv32 riscv cpu core

* Ran the program using the picorv32 core using ./rv32im.sh command in ~/labs
* Peeked inside the rv32im.sh file using vim rv32im.sh to check the -mabi and -march values

### 1) Program Run 

---
![gnu](https://github.com/ninja3011/riscv-cpu-core/blob/master/Day2/prog_run.PNG)

### 2) Read rv32im.sh

---
![gnu](https://github.com/ninja3011/riscv-cpu-core/blob/master/Day2/read_rv32im.PNG)

# TL-Verilog

- Transaction-Level Verilog (TL-Verilog) is an emerging extension to SystemVerilog that supports transaction-level design methodology. In transaction-level design, a transaction is an entity that moves through a microarchitecture.
- It is operated upon and steered through the machinery by flow components such as pipelines, arbiters, and queues.
- A transaction might be a machine instruction, a flit of a packet, or a memory read/write.
- The flow of a transaction can be established independently from the logic that operates on the transaction. 

# Makerchip

- Prime choice of online Editor for coding in TL-Verilog.
- We can code, compile, simulate, and debug Verilog designs, all from our browser. Our code, block diagrams, waveforms, and novel visualization capabilities are tightly integrated for a seamless design experience.
- Advantages:
  * Develop Verilog in your Browser
  * Easy Pipelining
  * Organized Waveforms
  * Organized Diagrams
  * Linked Design and Debug
### Testing the Validity Tutorial in MakerChip

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/validity_tut.PNG)

# Combinational Logic

-Combinational Logic can be thought of as logic that works in a procedural manner. One after the other. Here we are taking the example of AND(ing) 2 signals and of 2 signals.

### 2-Input Logic

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/2-input.PNG)

### Vectors (signals)
- Easy way to visualize is to imagine a different wire for each index of the vector. This is not how it always happens however, there are protocols which help transmit vectors through a single wire as well.

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/vectors.PNG)

### Mux Using ternary
- Ternary can be thought of as (cond) ? (execute if true) : (execute if false) ; very similar to an if-else block.

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/mux.PNG)

### Combinational Calculator
- A basic Combinational calculator made with a ternary operator.

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/combinational_calc.PNG)

### Counter
- Counter shows us the power of retiming which is made super simple in TLV. Think of adding a Flip-Flop ahead of the signal so the previous value of the signal can be accessed. 

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/counter.PNG)

# Sequential Logic

### Sequential Tutorial

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/seq_tut.PNG)

### Completed Calculator 
- After adding all the blocks, we have coded a complete working Calculator!

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/calc_final_1.PNG)

---
![calculator](https://github.com/ninja3011/riscv-cpu-core/blob/master/calc_final_2.PNG)

# Pipelined Logic
- This is a place where TLV Shines bright. All us Verilog users have faced the endless pains of pipelining and fails. In both the calculator and the cpu core we have used pipelines
- Defined with a '|' symbol
- Within the Pipeline, multiple stages can be defined using '@'
- This concept is called Time Abstraction

# Validity
- Validity can be understood by a single English Word, when?
- NOTE: This functionality is not a part of other circuit design languages and is exclusive to TLV
- This will make no difference on the nature of the code to be honest. In usual processes, these would all be not care
- It basically decides when a signal has significance, so it is only executed then.
- This keeps the Waveform clean and easy to debug
- You will see this operator being used '?' throughout the CPU Code.


# RISC-V CPU Architecture
- Any Microprocessor has a main job of executing programs
- It achieves this is 3 steps
  * Fetch
  * Decode 
  * Execute

# Fetch
- The processor fetches the instruction from the Instr Mem pointed by address given by PC

### Program Counter
- PC keeps the track of where the execution is currently at
- It is also instrumental in branching and Jump instructions
- By Modifying the PC we can get anywhere in our program

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/prog_counter.PNG)

### Register File Read
- A register is a collection of flip flops holding memory
- Here we see how to read its memory

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/reg_file_rd.PNG)

# Decode
- In decode the CPU identifies which instruction has been read in by the processor
- There are 6 types of instructions we have implemented:
  * R-type - Register 
  * I-type - Immediate
  * S-type - Store
  * B-type - Branch 
  * U-type - Upper Immediate
  * J-type - Jump 
- We Code this by piecing the different parts of the instruction and comparing with the RISC-V ISA 

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/decode.PNG)

# Execute
- Here we have completed the ALU
- And we can witness the CPU Performing its Intended Actions giving us the proper results.

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/alu_complete.PNG)

### Register File Write
- A register file has the capability of being written to or read from. 

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/reg_file_wr.PNG)

# Control Logic
- Control Logic is the order of execution. 
- We control the logic or rather embed logic in our programs using control logic instructions.
- i.e. Branch Instructions

# Base CPU
- We have completed all the base instructions for running our assembly program!

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/testbench.PNG)
- An important point to note is that the CPU is currently all in one pipeline.
- In a physical setting this may fail due to hazards and delays
- But on simulation we get the intended behaviour.

# Pipelined RISC-V CPU
- Next Order of Business to to seperate the physical processes into different stages. 
- Imagine each pipeline as a flow and each stage as a unit
- each stage seperation is marked by an understood flip flop
- This means all signals will not be in the same cycle, we can manage when which signals reach where.

# Jump 
- Added Jumps and completed Instruction Decode and ALU for all instruction present in the ISA 

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/jump.PNG)
### Completed Pipelined RISC-V CPU
- We have completed out CPU! Happy Coding and Extending :)
- Running 1to9sum.S in the snapshot below 

---
![cpu](https://github.com/ninja3011/riscv-cpu-core/blob/master/riscv_cpu_viz.PNG)

---
<p align ="center">
<img src="https://github.com/ninja3011/riscv-cpu-core/blob/master/riscv_cpu_diagram.PNG" width="25%" height="25%" />
</p>

# Acknowledgements
- [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
- [Steve Hoover](https://github.com/stevehoover), Founder, Redwood EDA
- [Shivani Shah](https://github.com/shivanishah269), IIITB , @fossi-foundation

