- von neumann architecture ([wiki](https://en.wikipedia.org/wiki/Von_Neumann_architecture))
- [alu wiki](https://en.wikipedia.org/wiki/Arithmetic_logic_unit) 
- ALU computes a function on the two inputs and outputs the result
- It can perform both arithmetic(addition, multiplication, division, ...) and logical(And, Or, Xor, ...)


- fundamental building block of the CPU and GPU
- inputs are the data to be operated on, **operands** and code indicating the operation to be performed.
## Data
- basic ALU has 3 parallel data buses, two input operands (A and B) and a result output (Y).
## Opcode
- specifie the desired arithmetic or logical operation to be performed by the ALU.
- opcode size determines the max number of distinct operations the ALU can perform (for ex,4-bit opcode can specify upto 16 (2^4) different ALU operations)
## Status bits
- ### Outputs
	- **Carry-Out** -> carry resulting from an addition operation, borrow from subtraction or overflow from binary shift
	- **Zero** -> indicates all bits of Y are zero
	- **Negative** -> indicates the output is negative
	- **Overflow** -> indicates the result has exceeded the numeric range of Y
	- **Parity** -> indicates whether an even or odd number of bits in Y are logic one.
- ### Inputs
	- single "carry-in" bit that is the carry out from previous ALU