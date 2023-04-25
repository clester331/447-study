# Sequential vs Combinational Logic
* Sequential logic - any memory, or any circuit that contains memory
  * Relies on the clock signal to tell the memory components when to update their contents 
  * Combines with combinational logic to make FSMs
* Combinational logic - Anything that doesn't have memory
  * Includes gates (AND, OR, NOT), plexers, arithmetic computations

# FSMs 
* Inputs, state, transition logic, outputs
* Transition logic determines the next state based on the current state and inputs
* The output logic determines the outputs from the current state

# Parts of the CPU

## PC FSM (Finite State Machine)
* Controls the PC (Program Coutner) and lets it advance to the next instruction, do absolute jumps, or do relative branches
  * **Absolute jumps** set the PC to some value
  * **Relative branches** add a number to the current PC to move forwards or backwards by a certain amount
 
## Instruction Memory (ROM)
* Contains the instructions, and is addressed by the PC - corresponds to the .text segment of your program

## Control
* Decodes the instruction and produces all the control signals for the rest of the CPU
 
### Control Signals
* Things such as write enables MUX/DEMUX selects - **They control what the other components do**

## Register File
* An array of general purpose registers; typically we can read and write multiple registers simultaneously

## ALU
* The Arithmetic and Logic Unit - performs arithmetic and logic (bitwise) operations such as add, sub, AND, OR, NOT, shifts, etc.

## Data Memory (RAM)
* Contains the variables that you can load or store - corresponds to the .data segment of your pgoram

## Interconnect
* All the wires and MUXES that connect all of the above components together, so that data can be flexibly routed to different components depending on the instruction

# Phases of instruction
* **Fetch** - Use PC to get instruction from memory
* **Decode** - control decodes instruction and sets control signals
* **Execute** - Wait for the ALU to produce it's output
* **Memory** - If it's a load or store, do that
* **Write Back** - put the result into the register file (Only for instructions that have a destination register) 

# Harvard vs Von Neumann and Single vs Multi Cycle
* In a single cycle machine, **Every instruction takes one clock cycle**
* In a multi-cycle machine, **Instructions take 2 or more clock cycles**
* Harvard = 2 memories (instruction and data)
* Von Neumann = 1 memory
  * VN is prefered as it is easier to deal with a sinlge address space 
  * **You cannot access two different adresses in one piece of memory at the same time**
* Single cycle = Harvard | Multi-cycle = Von Neumann 

# Kinds of Control
* Hardwired SIngle-cycle
  * Entirely combinational: Instruction goes in; control signal comes out
  * Bad performance
* Hardwired multi-cycle (FMS)
  * Multiple setps/phases for each instruction
  * Have to keep track of what pahse we are on
* Microcoded multi-cycle (Fancy FSM)
  * States and transition tables can be reprogrammed
  * Firmware is the "program" that implements the control FSM
* Hybrid miccrocode and hardwired
  * User hardwire control for really common and simple instructions

# Microcode
* Way of designing multi-cycle control so that each ISA instruction is implemented as a sequence of micro isntructions that perform the various phases of execution
* Flexible but slow
  * Hardcoded FSMs are harder to build but run much faster

# Other Definitions
* Caching - keeping copies of recently-used data in a smaller bust faster memory so it can be accessed more quickly in the near future
* Pipelining - partially overlappoing instruction execution  to improve throughput and complete instructions faster
* Superscalar CPUs - complete > 1 instruction per cycle by fetching and executing multiple instructions simultaneously
* Out of order CPUs - analyze several instructions in advance, then dynamically reorder them so they can be executed more quickly than they would as written

