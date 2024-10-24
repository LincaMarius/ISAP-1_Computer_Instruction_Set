# ISAP-1_Computer_Instruction_Set

This is the process of optimizing the functionality of the SAP-1 computer at the instruction set level, which I went through.

By: Lincă Marius Gheorghe.

Pitești, Argeș, România, Europe.

https://github.com/LincaMarius

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is a continuation of another project made by me:\
https://github.com/LincaMarius/ISAP-Computer

where I optimized the SAP-1 calculator at the block diagram level.

The final structure of the ISAP-1 computer is:

![ Figure 1 ](/Pictures/Figure1.png)

The block diagram of the Central Processing Unit of the ISAP-1 computer is:

![ Figure 2 ](/Pictures/Figure2.png)


## The original format of the SAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|


## The original instruction set of the SAP-1 computer is:

| Mnemonic | Opcode | Operation                                  |
|----------|--------|--------------------------------------------|
| LDA      | 0000   | Load RAM data into Accumulator             |
| ADD      | 0001   | Add RAM data to Accumulator                |
| SUB      | 0010   | Substract RAM data from accumulator        |
| OUT      | 1110   | Load Accumulator data into Output Register |
| HLT      | 1111   | Stop processing                            |

So, we have the instructions coded on the first 4 bits, leaving the next 4 bits to code the address of the operand in the case of the SAP-1 computer.

This means that a maximum of 2 ^ 4 = 16 memory locations can be accessed.

We also notice that the instructions Opcodes: 0011, 0100, 0101, 0110, 0111, 1000, 1001, 1010, 1011, 1100, 1101 are not used. So we can add 11 more new instructions.

## NOP instruction – No operation
Binary form:  **** **** \
Operation:  no operation \
Example: NOP

The NOP instruction has only the Fetch portion present in all instructions, but has nothing in the execution portion of the instruction. \
No action is performed. This instruction can be used in programs to delay the execution of an action while waiting for a response from slow peripherals.

The timing diagram for the NOP instruction is:

![ Figure 3 ](/Pictures/Figure3.png)

ALL UNIMPLEMENTED INSTRUCTIONS WILL BE TREATED BY THE ISP-1 CPU AS A NOP INSTRUCTION

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 1 ](/Pictures/Table1.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the NOP instruction is executed are:
-	EP = NOP * T1
-	LAR = NOP * T1
-	PM = NOP * T2
-	LI = NOP * T2
-	CP = NOP * T2

Since steps T1 and T2 are present and identical in any instruction we can say that they are independent of the executed instruction so we can rewrite the instructions as follows:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2

*If we implement the Control Block using Combinational Logic we will use these equations.*

## LDA instruction – Load the Accumulator
Binary form:    0000 nnnn \
Operation:      A ← [n] \
Example: LDA 9h 

Loads the numeric value from Address n into the Accumulator.

The timing diagram for the LDA instruction is as follows:

![ Figure 4 ](/Pictures/Figure4.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 2 ](/Pictures/Table2.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS. 

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the LDA instruction is executed are:
-	EP = T1
-	LAR = T1 + LDA * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3
-	DM = LDA * T4
-	LAH = LDA * T4
-	LAL = LDA * T4

The last two equations are equivalent to: LA = LDA * T4

*If we implement the Control Block using Combinational Logic we will use these equations.*

## ADD instruction – Add to accumulator
Binary form:  0001 nnnn \
Operation:    A ← A + [n] \
Example: ADD 8h 

Adds the numeric value at address n with the numeric value stored in the Accumulator and stores the result in the Accumulator.

The timing diagram for the ADD instruction is as follows:

![ Figure 5 ](/Pictures/Figure5.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 3 ](/Pictures/Table3.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the ADD instruction is executed are:
-	EP = T1
-	LAR = T1 + ADD * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = ADD * T3
-	DM = ADD * T4
-	LB = ADD * T4
-	EU = ADD * T5
-	LAH = ADD * T5
-	LAL = ADD * T5

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 = T1 + (LDA + ADD) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 = (LDA + ADD) * T3
-	DM = LDA * T4 + ADD * T4 = (LDA + ADD) * T4
-	LB = ADD * T4
-	EU = ADD * T5
-	LAH = LDA * T4 + ADD * T5
-	LAL = LDA * T4 + ADD * T5

*If we implement the Control Block using Combinational Logic we will use these equations.*

## SUB Instruction – Subtract from accumulator
Binary form:  0010 nnnn \
Operation:  A ← A – [n] \
Example: SUB 5h

Subtracts the numeric value at address n from the numeric value stored in the Acumulator and stores the result in the Accumulator.

The timing diagram for the SUB instruction is as follows:

![ Figure 6 ](/Pictures/Figure6.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 4 ](/Pictures/Table4.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the SUB instruction is executed are:
-	EP = T1
-	LAR = T1 + SUB * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = SUB * T3
-	DM = SUB * T4
-	LB = SUB * T4
-	EU = SUB * T5
-	LAH = SUB * T5
-	LAL = SUB * T5
-	SU = SUB * T5

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 = T1 + (LDA + ADD + SUB) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 = (LDA + ADD + SUB) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 = (LDA + ADD + SUB) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LDA * T4 + ADD * T5 + SUB * T5 = LDA * T4 + (ADD + SUB) * T5
-	LAL = LDA * T4 + ADD * T5 + SUB * T5 = LDA * T4 + (ADD + SUB) * T5
-	SU = SUB * T5

*If we implement the Control Block using Combinational Logic we will use these equations.*

## OUT instruction – Output data from the accumulator
Binary form:  1110 pppp \
Operation:    PORT p ← A \
Example: OUT 1h

Transfers the numeric value stored in the Accumulator to Output Port p.

The timing diagram for the OUT instruction is as follows:

![ Figure 7 ](/Pictures/Figure7.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 5 ](/Pictures/Table5.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the OUT instruction is executed are:
-	EP = T1
-	LAR = T1 + OUT * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = OUT * T3
-	EA = OUT * T4
-	I/O = OUT * T4
-	R/W = OUT * T4

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 = T1 + (LDA + ADD + SUB + OUT) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 = (LDA + ADD + SUB + OUT) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 = (LDA + ADD + SUB) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LDA * T4 + ADD * T5 + SUB * T5 = LDA * T4 + (ADD + SUB) * T5
-	LAL = LDA * T4 + ADD * T5 + SUB * T5 = LDA * T4 + (ADD + SUB) * T5
-	SU = SUB * T5
-	EA = OUT * T4
-	I/O = OUT * T4
-	R/W = OUT * T4

*If we implement the Control Block using Combinational Logic we will use these equations.*

## The HLT instruction – Halt computer
Binary form:  1111 1111 \
Operation:  Halt computer \
Stops further execution of computer instructions. It is an instruction with prefix 1111 and has no parameters. \
Example: HLT

The timing diagram for the HLT instruction is as follows:

![ Figure 8 ](/Pictures/Figure8.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 6 ](/Pictures/Table6.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the HLT instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	HLT = HLT * T3

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 = T1 + (LDA + ADD + SUB + OUT) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 = (LDA + ADD + SUB + OUT) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 = (LDA + ADD + SUB) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LDA * T4 + ADD * T5 + SUB * T5 = LDA * T4 + (ADD + SUB) * T5
-	LAL = LDA * T4 + ADD * T5 + SUB * T5 = LDA * T4 + (ADD + SUB) * T5
-	SU = SUB * T5
-	EA = OUT * T4
-	I/O = OUT * T4
-	R/W = OUT * T4
-	HLT = HLT * T3


*If we implement the Control Block using Combinational Logic we will use these equations.*

## LIL instruction – Load immediate value into lower nibble of Accumulator
Binary form: 0011 nnnn \
Operation: A[3-0] ← Imm \
Example: LIL 2h

This instruction is added by me and is useful for loading an immediate numeric value into the Accumulator in its lower half, leaving the upper half unchanged.

The timing diagram for the LIL instruction is as follows:

![ Figure 9 ](/Pictures/Figure9.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 7 ](/Pictures/Table7.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the LIL instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LIL * T3
-	LAL = LIL * T3

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 = T1 + (LDA + ADD + SUB + OUT) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + LIL * T3 = (LDA + ADD + SUB + OUT + LIL) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 = (LDA + ADD + SUB) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LDA * T4 + ADD * T5 + SUB * T5 = LDA * T4 + (ADD + SUB) * T5
-	LAL = LIL * T3 + LDA * T4 + ADD * T5 + SUB * T5 = LIL * T3 + LDA * T4 + (ADD + SUB) * T5
-	SU = SUB * T5
-	EA = OUT * T4
-	I/O = OUT * T4
-	R/W = OUT * T4
-	HLT = HLT * T3

*If we implement the Control Block using Combinational Logic we will use these equations.*

## LIH instruction – Load immediate value into upper nibble of Accumulator
Binary form: 0100 nnnn \
Operation: A[7-4] ← Imm \
Example: LIH 5h

This statement is added by me and is useful for loading an immediate numeric value into the Accumulator in its upper half, leaving the lower half unchanged.

The timing diagram for the LIH instruction is as follows:

![ Figure 10 ](/Pictures/Figure10.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 8 ](/Pictures/Table8.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the LIH instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LIH * T3
-	LAH = LIH * T3

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 = T1 + (LDA + ADD + SUB + OUT) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + LIL * T3 + LIH * T3 = (LDA + ADD + SUB + OUT + LIL + LIH) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 = (LDA + ADD + SUB) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LIH * T3 + LDA * T4 + ADD * T5 + SUB * T5 = LIH * T3 + LDA * T4 + (ADD + SUB) * T5
-	LAL = LIL * T3 + LDA * T4 + ADD * T5 + SUB * T5 = LIL * T3 + LDA * T4 + (ADD + SUB) * T5
-	SU = SUB * T5
-	EA = OUT * T4
-	I/O = OUT * T4
-	R/W = OUT * T4
-	HLT = HLT * T3

*If we implement the Control Block using Combinational Logic we will use these equations.*

## IN Instruction – Input data from an input port into the Accumulator
Binary form:  1101 pppp \
Operation:  A ← PORT p \
Example: IN 1h

This instruction is added by me and transfers the numeric value from Input Port p to Accumulator.

The timing diagram for the IN instruction is as follows:

![ Figure 11 ](/Pictures/Figure11.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 9 ](/Pictures/Table9.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the IN instruction is executed are:
-	EP = T1
-	LAR = T1 + IN * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = IN * T3
-	I/O = IN * T4
-	LAH = IN * T4
-	LAL = IN * T4

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + IN * T3 = T1 + (LDA + ADD + SUB + OUT + IN) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + LIL * T3 + LIH * T3 + IN * T3 = (LDA + ADD + SUB + OUT + LIL + LIH + IN) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 = (LDA + ADD + SUB) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LIH * T3 + LDA * T4 + IN * T4 + ADD * T5 + SUB * T5 = LIH * T3 + (LDA + IN) * T4 + (ADD + SUB) * T5
-	LAL = LIL * T3 + LDA * T4 + IN * T4 + ADD * T5 + SUB * T5 = LIL * T3 + (LDA + IN) * T4 + (ADD + SUB) * T5
-	SU = SUB * T5
-	EA = OUT * T4
-	I/O = OUT * T4 + IN * T4 = (OUT + IN) * T4
-	R/W = OUT * T4
-	HLT = HLT * T3

*If we implement the Control Block using Combinational Logic we will use these equations.*

## JMP instruction – Unconditional jump to address n
Binary form:  1100 nnnn \
Operation:  PC ← Imm \
Example: JMP 7h

This statement is added by me and performs an unconditional jump to Address n.

The timing diagram for the JMP instruction is as follows:

![ Figure 12 ](/Pictures/Figure12.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 10 ](/Pictures/Table10.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the JMP instruction is executed are:
-	EP = T1
-	LAR = T1
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = JMP * T3
-	LP = JMP * T3

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + IN * T3 = T1 + (LDA + ADD + SUB + OUT + IN) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + LIL * T3 + LIH * T3 + IN * T3 + JMP * T3 = (LDA + ADD + SUB + OUT + LIL + LIH + IN + LMP) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 = (LDA + ADD + SUB) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LIH * T3 + LDA * T4 + IN * T4 + ADD * T5 + SUB * T5 = LIH * T3 + (LDA + IN) * T4 + (ADD + SUB) * T5
-	LAL = LIL * T3 + LDA * T4 + IN * T4 + ADD * T5 + SUB * T5 = LIL * T3 + (LDA + IN) * T4 + (ADD + SUB) * T5
-	SU = SUB * T5
-	EA = OUT * T4
-	I/O = OUT * T4 + IN * T4 = (OUT + IN) * T4
-	R/W = OUT * T4
-	HLT = HLT * T3
-	LP = JMP * T3

*If we implement the Control Block using Combinational Logic we will use these equations.*

## STA Instruction – Store Accumulator
Binary form:  0101 nnnn \
Operation:  [n] ← A \
Example: STA 9h

This instruction is added by me and has the effect of storing the numeric value present in the Accumulator at address n in Data Memory.

The timing diagram for the STA instruction is as follows:

![ Figure 13 ](/Pictures/Figure13.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 11 ](/Pictures/Table11.png)

Signals represented in Red: are active when data is written to the Data BUS. \
Signals represented in Green: are active when reading data from the Data BUS. \
Signals shown in Black: their activation has no influence on the Data BUS.

*If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.*

The Boolean equations for the signals that are active when the STA instruction is executed are:
-	EP = T1
-	LAR = T1 + STA * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = STA * T3
-	EA = STA * T4
-	DM = STA * T4
-	R/W = STA * T4

All the Boolean equations for the control signals that are active for the instructions implemented so far are:
-	EP = T1
-	LAR = T1 + LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + IN * T3 + STA * T3 = T1 + (LDA + ADD + SUB + OUT + IN + STA) * T3
-	PM = T2
-	LI = T2
-	CP = T2
-	EI = LDA * T3 + ADD * T3 + SUB * T3 + OUT * T3 + LIL * T3 + LIH * T3 + IN * T3 + JMP * T3 + STA * T3 = (LDA + ADD + SUB + OUT + LIL + LIH + IN + LMP + STA) * T3
-	DM = LDA * T4 + ADD * T4 + SUB * T4 + STA * T4 = (LDA + ADD + SUB + STA) * T4
-	LB = ADD * T4 + SUB * T4 = (ADD + SUB) * T4
-	EU = ADD * T5 + SUB * T5 = (ADD + SUB) * T5
-	LAH = LIH * T3 + LDA * T4 + IN * T4 + ADD * T5 + SUB * T5 = LIH * T3 + (LDA + IN) * T4 + (ADD + SUB) * T5
-	LAL = LIL * T3 + LDA * T4 + IN * T4 + ADD * T5 + SUB * T5 = LIL * T3 + (LDA + IN) * T4 + (ADD + SUB) * T5
-	SU = SUB * T5
-	EA = OUT * T4 + STA * T4 = (OUT + STA) * T4
-	I/O = OUT * T4 + IN * T4 = (OUT + IN) * T4
-	R/W = OUT * T4 + STA * T4 = (OUT + STA) * T4
-	HLT = HLT * T3
-	LP = JMP * T3

*If we implement the Control Block using Combinational Logic we will use these equations.*

