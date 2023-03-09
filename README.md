
# RISC-V based MYTH  - Building a RISC-V Core using TL-Verilog
RISC-V based CPU Core Design MYTH , offered by for **VLSI System Design (VSD) and Redwood EDA.**
The basic RISC-V ISA was studied & a simple RISC-V core with base instruction set was implemented. Under the software section, the programming languages that have been used are C, Assembly language and some Pseudo codes. The RISC-V CPU Core has been designed with the help of **Transaction Level Verilog(TL-Verilog) in addition with the Makerchip IDE Platform.**

## 1. Labwork for RISC-V software toolchain

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
  
  ![inv-dir](Day1/u5.png)

  ## 2. ABI and Basic verification Flow
  
 ### New Algorithm for sum 1 to N using ASM language
 
 Write these two codes:
 
 ![inv-dir](Day2/l1.png)
 
 ![inv-dir](Day2/l2.png)
 
 Review ASM function calls
 
 Now for compilation and simulation run these.
 
 ```
 riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o 1to9_custom.o 1to9_custom.c load.S
 spike pk 1to9_custom.o
 riscv64-unknown-elf-objdump -d 1to9_custom.o | less 
 ```
  ![inv-dir](Day2/l3.png)
  
  ![inv-dir](Day2/l4.png)
  
  ![inv-dir](Day2/l5.png)
  
  ![inv-dir](Day2/l6.png)

## 3. Digital logic with TL-verilog and makerchip

![inv-dir](Day3/m1.png)

1. Inverter

![inv-dir](Day3/m3.png)

2. Vector

![inv-dir](Day3/m4.png)

3. MUX

![inv-dir](Day3/m5.png)
![inv-dir](Day3/m6.png)

### Combinational Calculator

This circuit implements a calculator that can perform +,-,/,* on two input values.

![inv-dir](Day3/m7.png)


Free running counter:

![inv-dir](Day3/c1.png)

## Sequential logic :

![inv-dir](Day3/c2.png)

## Pipelined logic :

![inv-dir](Day3/c3.png)

## 4.Basic RISC-V CPU micro-architecture:
Designing the basic processor of 3 stages fetch, decode and execute based on RISC-V ISA.
# RISC-V Core Implementation

Designing the basic processor of 3 stages fetch, decode and execute based on RISC-V ISA.

## [Program Counter]()
   The program counter (PC), commonly called the instruction pointer (IP) is a counter in a processor that indicates where a computer is in its program. PC jumps 4bytes at a time as each instruction is 32bits in RV32.

![RISCV_CPU_PC_Implmentation](Images/Program_counter_imp.png)


## [Fetch]()

*The instruction fetch unit (IFU) in a central processing unit (CPU) is responsible for organising program instructions to be fetched from memory, and executed, in an appropriate order. This makes the control logic of the core.*

![CPU_Instruction_cycle_diagram](Images/Instruction_fetch.png)


## [Decode]()

*The decoding stage allows the CPU to determine what instruction is to be performed so that the CPU can tell how many operands it needs to fetch in order to perform the instruction. The opcode fetched from the memory is decoded for the next steps and moved to the appropriate registers. Below image shows hoe decode is determining the TYPE OF RISC V instructions set (Various types of Instructions in RV32 are I, R, S, J, U)*

![Screenshot from 2020-08-30 03-52-41](Images/Instruction_decode.png)

Waveform showcasing BLT signal (Branch if less than) Toggle on Branch Instruction decode. 

![Instruction_Decode_Waveform](https://user-images.githubusercontent.com/14968674/92879155-69db3980-f42a-11ea-9457-c2254b092e05.png)


## [Execute]()

*An arithmetic-logic unit (ALU) is the part of the CPU that carries out arithmetic and logic operations. Below image shows an ADDI (ADD Immediate) instruction computation.*

*A unique feature of makerchip yet to be released to public is VIZ (Visualization), it helps analyze the implementation visually thereby developers can understand how instructions are executed and which registers is at play during transactions and the final register output.*

![ADD_register_write](https://github.com/RISCV-MYTH-WORKSHOP/RISC-V-Core/blob/master/Images/ALU.png)




## [RISC-V Pipelined Core]()

*Converting non-piepleined CPU to pipelined CPU using timing abstract feature of TL-Verilog. This allows easy retiming wihtout any risk of funcational bugs.*

*The Core was enhanced to be staged across multi-stages in a pipeline, Final output where the core is computing Sum of 9 numbers and the code for the same is available [here](https://github.com/RISCV-MYTH-WORKSHOP/RISC-V-Core/blob/master/Day3_5/risc-v_solutions.tlv).*

![RISCV_CPU_CORE](https://github.com/RISCV-MYTH-WORKSHOP/RISC-V-Core/blob/master/Images/RISC%20V%203%20Stage%20Pipelined%20Core.png)

# Conclusion

This project was done as a part of the [RISC-V based MYTH (Microprocessor for You in Thirty Hours)][3] workshop conducted by Kunal Ghosh and Steve Hoover. The current project implements almost the entire RV32I base instruction set. We capable of executing all RISC-V instructions in four cycles with easy pipelining using Transaction-Level Verilog. TL-Verilog not only reduces your code size significantly but allows us to freely declare signals without explicitly declaring them (just like Python does compare to C). In addition, we can generate Verilog/SystemVerilog code from TL-Verilog in Makerchip IDE which using Sandpiper complier. Future work involves modifying the current design to implement support for the remaining operations and also implementation of other standard extensions like M, F and D.
