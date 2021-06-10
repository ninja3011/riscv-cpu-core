# Running the 1to9_custom.c and load.S on the riscv GNU toolchain

* First, we ran the program using riscv64-unknown-elf-gcc command.
* Then, debugged the Program using spike -d pk 1to9_custom.o to check for values of registers at specific addresses as asked
* Finally we took a look into the assembly code to check the address locations of the load and theloop blocks.

### 1) Program Run and Debug

---
![lab2](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_jun21-ninja3011/blob/master/Day2/myth_day2_lab2_1.PNG)
### 2) Debug

---
![lab2](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_jun21-ninja3011/blob/master/Day2/myth_day2_lab2_2.PNG)
### 3) Assembly code

---
![lab2](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_jun21-ninja3011/blob/master/Day2/myth_day2_lab2_3.PNG)

# Running the 1to9_custom.c and load.S on the picorv32 riscv cpu core

* Ran the program using the picorv32 core using ./rv32im.sh command in ~/labs
* Peeked inside the rv32im.sh file using vim rv32im.sh to check the -mabi and -march values

### 1) Program Run 

---
![lab3](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_jun21-ninja3011/blob/master/Day2/myth_day2_lab3_1.PNG)

### 2) Read rv32im.sh

---
![lab3](https://user-images.githubusercontent.com/51434707/121593950-14d06080-ca5a-11eb-85de-8cf8dcf1a2dc.png)
