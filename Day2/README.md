# Running the 1to9_custom.c and load.S on the riscv GNU toolchain

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

# Running the 1to9_custom.c and load.S on the picorv32 riscv cpu core

* Ran the program using the picorv32 core using ./rv32im.sh command in ~/labs
* Peeked inside the rv32im.sh file using vim rv32im.sh to check the -mabi and -march values

### 1) Program Run 

---
![gnu](https://github.com/ninja3011/riscv-cpu-core/blob/master/Day2/prog_run.PNG)

### 2) Read rv32im.sh

---
![gnu](https://github.com/ninja3011/riscv-cpu-core/blob/master/Day2/read_rv32im.PNG)
