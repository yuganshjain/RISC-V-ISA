
# RISC-V based MYTH  - Building a RISC-V Core using TL-Verilog
RISC-V based CPU Core Design MYTH , offered by for **VLSI System Design (VSD) and Redwood EDA.**
The basic RISC-V ISA was studied & a simple RISC-V core with base instruction set was implemented. Under the software section, the programming languages that have been used are C, Assembly language and some Pseudo codes. The RISC-V CPU Core has been designed with the help of **Transaction Level Verilog(TL-Verilog) in addition with the Makerchip IDE Platform.**

## Labwork for RISC-V software toolchain

### Program to compute sum of 1 to N
```
cd
leafpad sum1ton.c
gcc sum1ton.c
./a.out
```
Code :

![inv-dir](Day1/c1.png)

Output :

![inv-dir](Day1/c2.png)

![inv-dir](Day1/c3.png)

### RISC-V gcc compile and Disassemble
We will run the same sum1ton.c program using RISC-V simulator
First we will compile it with RISC-V gcc compiler

```
riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
![inv-dir](Day1/o1.png)
It will generate file sum1ton.o

Now we will see the assembly code for the sum1ton.c file
new terminal
![inv-dir](Day1/o2.png)
```
riscv64-unknown-elf-objdump -d sum1ton.o
riscv64-unknown-elf-objdump -d sum1ton.o | less
/main
```
![inv-dir](Day1/o3.png)

check number of instructions

![inv-dir](Day1/o4.png)

### Spike simulation and Debug

We are doing ./a.out for the gcc compiler to check the output.
Same thing is done in RISK-V using spike
```
spike pk sum1ton.o
```
![inv-dir](Day1/s1.png)

Open the objectdump if we want to debug using *riscv64-unknown-elf-objdump -d sum1ton.o | less*

To open debugger
```
spike -d pk sum1ton.o
```
To run instructions from 0 to specific like 100b0 pc

![inv-dir](Day1/s2.png)

To view the contents of register
![inv-dir](Day1/s3.png)

Press enter to run next instruction

![inv-dir](Day1/s4.png)

![inv-dir](Day1/s5.png)

![inv-dir](Day1/s6.png)

![inv-dir](Day1/s7.png)

### Signed and Unsigned numbers:
Write this code in vim
 ![inv-dir](Day1/u1.png)
 
 Then run
 ```
 riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o unsigned.o unsigned.c
 spike pk unsigned.o
 ```
  ![inv-dir](Day1/u2.png)
  
  ![inv-dir](Day1/u3.png)
    
  ![inv-dir](Day1/u4.png)
  
  ![inv-dir](Day1/u5.png)'

  
  
 
 
