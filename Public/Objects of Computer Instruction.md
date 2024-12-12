# Objects of Computer Instruction
Computer instructions consist of:
1. Operations: what to do (e.g. add, multiply, etc.); described by the [ISA](Instruction%20Set%20Architecture%20(ISA).md)
2. Operands: what objects to manipulate (data stored in registers, memory locations, or constant values, etc.)

### Operations
1. Data Transfers: move data between registers and memory, or between integer and floating point or special registers
2. Arithmetic/Logical: Operations on integer or logical data in General Purpose Registers (GPRs). For example: add multiply, and, xor, etc.
3. Control: ==Conditional branches and jumps; PC-relative or through register==
4. Floating Point
### Operands
Operands can be:
- Registers: small, high-speed storage locations within the processor itself.
- Memory locations: data stored in the computer's main memory
- Constants: fixed values embedded directly within the instruction

The number of operands in an instruction is called the operand count

Instructions can have zero, one, two, or three operands. Most instructions have two operands.

Operands are specified using different addressing modes:
- Immediate addressing: the operand is a constant within the instruction
- Register addressing: the operand is contained in a processor register
- Base or displacement addressing: the operand is at the memory location whose address is the sum of a register and a constant in the instruction
- PC-relative addressing: the branch address is the sum of the PC and a constant in the instruction
Choosing the right addressing modes affects code size and the number of instructions needed for a computation

#
###### Backlinks
- [Computer Architecture MoC](Computer%20Architecture%20MoC.md)