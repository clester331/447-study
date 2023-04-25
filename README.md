# Chapter 13 - CPU Intro

## ISA Hardware and design
* *ISA - The software interface the programmers use to control the CPU*
  * The ISA specifies the software interface to the CPU

* *Microarchitecture - The hardware design of a physical CPU*
  * Microarchitecture is an implementation of an ISA

## CPU Organization
* Major parts of the CPU
  * *Control - Tells everything else what to do and when*
  * *Registers - Hold the values being computed*
  * *ALU - computes new value*

* Registers are just a temporary stopping point for your program's data
* *The registers that the ISA specifies are the architectural registers*
  * *The general-purpose registers are the ones that you can use for any of your program*
  
* *The register file holds the general-purpose registers*
  * It's like an array of registers, or small word-addressed memory
  
* *The ALU takes old values and produces new ones from them

* *The PC register is part of the control. It says what step to do next*
  * The PC is a memory address, so we sent that to memory, the memory then sends back an instruction which gets decoded by the control and tells everything else what to do
 
* All parts of the CPU work together to do the CPU's job: **To read and execute instructions**
* For every instruction, it:
  * *fetches* the next instruction to execute from memory
  * Then the control *decodes* (reads) the instruction
  * The control *executes* the instrucion by telling the other parts what to do
**Fetch, decode, execute. Once per isntruction**

* For a load or store, memory is accessed twice:
  * Once to fetch the instruction, and a second time to access the value
  
# Chapter 14 - Gates and Wires

## Schematics
* *Schematic - a graphical way to represent systems* (Like a flowchart)
* *Circuit Schematics - show how data flows from one component to another*
  * Components can both produce and consume values
  
* There is no implied order when you make a circuit schematic
* **Everything in the circuit happens simultaneously**

## Boolean Functions and Gates
* *boolean function - Takes one or more Boolean (true/false) inputs and produces a boolean output*
* *truth table -  summarizes a Boolean function by listing the output for every possible combination of inputs*

* *Gate* - implements one of the basic Boolea logic functions
![image](https://user-images.githubusercontent.com/122314614/233881016-6804a508-dfe7-4510-89a8-ef97527cae66.png)

* If you stick a NOT after and AND gate, you get a NAND gate
  * Any boolean function can be made with NAND gates

# Chapter 15 - Arithmetic and Decisions

## Binary addition, in logic
* All componenets can be built from the basic boolean logic operations (AND, OR, NOT)
* A full adder contains 3 input bits (Carry in, A, B), and 2 output bits (Carry out, sum)           
![image](https://user-images.githubusercontent.com/122314614/233897804-14ba87b5-4f82-4f38-8d87-3227ee8f1a37.png)
![image](https://user-images.githubusercontent.com/122314614/233897819-6a2e194b-27bb-45f5-ad39-9cc42ea0a072.png)
* S is 1 if we have an odd number of 1s between C<sub>i</sub>, A, and B
* Co is 1 if we have 2 or 3 1s                       
![image](https://user-images.githubusercontent.com/122314614/233897967-da08a0ef-4026-4b28-81ef-ce097e762933.png)

* Three steps to building a truth table
  * Make a truth table that expresses the computation that we want
  * Turn the truth table into a Boolean expression,
  * Turn that into logic gates

## Adding and Subtracting Multi-bit Numbers
* If we want to add two three-bit numbers, we need three one bit adders
* We chan the carries from each place to the next higher place                                   
![image](https://user-images.githubusercontent.com/122314614/233898499-fef53fcc-a106-4c44-88a2-b5fcd73a8ea0.png)

* Propogation delay is the amount of time needed for a signal to fully travel through a wire
* In ripple carry, sincec each place must wait for the next smaller piece to compute it's final answer **Ripple Carry is linear in the number of bits**
  * If you double the number of bits, then you double the amount of time needed
* For subtraction, we can use a full adder for the LSB, and when we subtract, we set the "carry in" to 1
  * now becomes x + ~y + 1                           
  ![image](https://user-images.githubusercontent.com/122314614/233899013-dc457839-0203-4a7c-87b0-92420435ec84.png)

* To detect unsigned verflow:
  * When adding, if the MSB carry out is 1, it's an overflow
  * When subtracting, if the MSB carry out is 0, it's an overflow                               
![image](https://user-images.githubusercontent.com/122314614/233899180-db5449a5-f92e-442f-bb0d-b3c8feed2b4d.png)
* When A&B are the same and SO is opposite, then O is 1

## Hardware That Makes Decisions
**When you make a decision in *software*, only one code path is run; the others never happen**
**WHen you make a decision in *hardware*, you do all possible thinkgs, and then pick the only thing you need. Ignore the rest**
* *Multiplexer(MUX) - Outputs one of its inputs based on a select*
  * **The select signal is basically the condition**                                 
![image](https://user-images.githubusercontent.com/122314614/233900062-5579f046-5c67-4e8a-86bc-70406da2bbd3.png)

# Chapter 16 - Sequential Logic, Registers, and Clocking

## Combinational Logic
* Combinational logic takes a combination of inputs and returns an output
* **For combinational logic, the outputs only depends on the current inputs**

## Sequential Logic
*This is what and RS latch looks like                                
![image](https://user-images.githubusercontent.com/122314614/233900591-a1692da9-2e39-4824-8ab3-207940e93300.png)

* When you poke R or S, it makes it settle into one state or another.
* You can also switch between states as well, making this a circuit that remembers
* **This circut is a sequential circuit - a circuit that remembers data**
* *Flip flop - A component made up of multiple RS latches and some other gates. It stores a single bit*

## Time
* Sequential circuits can preform sequences of operations, things that happen over time
* *Clock Signal - Tells us when to move on to the next step*
  * It keeps all the sequential circuits in sync (Switches between 0 & 1 in steady rhythm)
* *Clock edges - when it changes between 0 and 1 (rising and falling)*
* *Clock Cycle - time from one rising edge to the next*

*When inserting a clock into a rising edge-triggered flip-flop, the rising edge of the clock makes it take a snapshot of what the input was and remembers it.
  * The input can change but the flip-flop remembers what it saw at the last rising clock edge, until the next clock edge comes

## Registers
* A flip-flop is a 1-bit register
* *Splitters - How we connect between bundles and individual wires*
* An n-bit register is n-flip flops next to each other
![image](https://user-images.githubusercontent.com/122314614/234093555-292a0cd5-5265-4fe1-b1b9-82eb187cae07.png)

## Write Enables
* **We have 32 registers, but on each step, at most one register changes**
* *Write enable - when the write enable is 0, it ignores the clock value and never changes.* **The only way to cahnge its value is when the write enable is 1 and the clock has a rising edge** 
* A write enable chooses if we change a register's value on each cycle
* **A register with a write enable can remember a value accross multiple clock cycles, instea of just until the next cycle**
* **Registers do not accept any new values until the next rising clock edge**

# Chapter 17 - FSMs and Multiplication

## States and Transitions

* *State - all the things that are remembered by a system*           
![image](https://user-images.githubusercontent.com/122314614/234095454-6da90a42-2e2b-4311-8d66-2cd18d1636fb.png)
* **The above image is a state transition diagram - it shows all the states and where each step goes next**
* *State transition table - sequential version of a truth table*
  * **It encodes the same information as a state transition diagram**
![image](https://user-images.githubusercontent.com/122314614/234096055-c42442ce-78b2-457d-a8d2-971909c9d54f.png)

## Finite State Machines (FSMs)
* *FSM - a way of thinging about a sequential process where*
  * *There is a state (memory)*
  * *The state changes based on the current state, transitional logic, and inputs*
  * There are outputs based on the state
* **the transition logic determines the next state based on the current state and inputs**

## Multiplication
* In A x B, the product (answer) is "A copies of B, added together"                 
![image](https://user-images.githubusercontent.com/122314614/234099907-07cc6766-3d37-4770-96e4-4a467183b34b.png)
* **Because multiplication is repeated addition, it is always slower than addition**
* **When multiplying two n-digit/bit numbers, the product will be at most, 2n digits/bits **
  * In MIPS, there are two 32-bit registers, HI and LO. HI contains the upper 32 bits and LO contains the lower 32 bits
* **Addition is commutative and associative**
* Fast multiplication algorithm:
![image](https://user-images.githubusercontent.com/122314614/234100872-d3a4a377-83db-4f53-bf51-87bd727ddfb5.png)
* This algorithm takes a logarithmic number of steps based on the bits n in the input
  * That means if you doble n, you only add one more step
* For n bits, it requires (n-1) n bit adders
  * **This means the entire process uses n<sup>2</sup> 1-bit adders. If you double n, you quadruple the number of 1 bit adders**
* This is a time space tradeoff: it completes quickly at the cost of having to use a lot of space to do it

# Chapter 18 - Multipilication and Division

## Multiplication with an FSM
* New method for solving multiplication
    1. We iterate over the bits of the multiplier right to left
    2. We either add 0 to the product, or add the multiplicand, shifted left by an increasing distance
    3. Takes a linear number of steps in the number of bits (double the bits = double the steps)
    4. Add the partial products onto the final product as soon as we compute them
    ![image](https://user-images.githubusercontent.com/122314614/234102529-2bc48654-f36d-4130-93b5-a9a1a2aea4ff.png)

## Division
* Division is repeated subtraction
* We find the partial products that add up to a total project
* **Division goes left-to-right because we can't know the smaller partial products until we know the larger ones**
  * **Multiplication is multiplying polynomials and division is factoring polynomials**
* *Divisor - the number that divides the dividend & Dividend - the number being divided*
![image](https://user-images.githubusercontent.com/122314614/234104826-935d9e96-15c4-4d3d-b74b-e8f358a0bdd1.png)
* **Subtraction is not commutative or associative**
* Division is fundamentally slower than multiplication
  * **Each step depends on the previous one; cannot be split into sub-problems like multiplication**
  * **Division is still a linear time algorithm**

## Signed Division
* Dividend = (Divisior x Quotient) + remainder

# Chapter 19 - The Register File, ALU, and Memory
  
## The Register File
* The register file contains the general purpose registers. They are all the registers we can name in instructions
* The register file must be able to
  * Read from two different registers at the same time
  * Write to a third, different register at the same time                  
![image](https://user-images.githubusercontent.com/122314614/234131071-822f7dff-ff13-4cd3-86df-e77d9aea178e.png)
* rd, rs, and rt are 5 bit numbers ued to index the registers in the register file
* **rd and REG[rd] are not the same thing. rd is the index used to access the item in REG**
  
* A demux forwards its input to one of its outputs. The rest will be zero. For the register file's case, we will use to activate write enable for the register we are editing
* The register file component:                                           
![image](https://user-images.githubusercontent.com/122314614/234131760-9f3ef1a0-b206-423f-8947-3316d53874fe.png)

## The ALU
* is the Arithmetic and Logic Unit - performs arithmetic and logic (bitwise) operations - add, subtract, AND, OR, NOT, shifts…
  * Combines two values to get a new one (entirely combinational without multiplication or division)
* The ALU consists of a MUX and arithmetic/logic components (+, -, AND, OR, XOR NOT, SHL, SHR)

## Memory
* memory is an array of bytes. Each byte has an addres that you can read and write from                         
![image](https://user-images.githubusercontent.com/122314614/234132274-36b25eb8-cc0a-470c-844c-bd78f95be3ba.png)
* **You can only access one address in memory per clock cycle**
  * Must wait until the next clock cycle to access another value at another address
* **Every instruction must be fetched from memory before it can be executed**
  * For a load, we do one access to fetch the instruction
  * And a second access to actually load the value
  * **lw is impossible to implement in one cycle**
   
## Von Neumann vs Harvard
![image](https://user-images.githubusercontent.com/122314614/234132692-db9a6e80-01ac-4829-9e53-63c2cc5dc53c.png)
* *Harvard Architecture: Has two memories. One for instructions and one for data*
* *Von Neumann Architecture: Has one memory for both instructions and data*
  * Despite having one memory, it uses multiple clock cycles to execute each function
* Harvard (two memories) is easier for hardware designers
* Von Neaumann (one memory) is easier for programmers

# Chapter 20 - The PC and Interconnect

## Machine Code and Control
* The instruction in an ISA, tells you how an assembler translates your code into binary, and what a CPU will do when it sees that binary instruction
* The assmelber's job: turns assembly language code (written by humans) into machine code (for the CPU to read) based on the ISA                    
![image](https://user-images.githubusercontent.com/122314614/234133453-21312386-524a-4fba-bf97-eba518c773ca.png)
* We can encode instructions as bitfields to save on size
  * The opcode (and funct) field identifies which instruction is being sent
![image](https://user-images.githubusercontent.com/122314614/234133603-4e8b7579-f681-4462-83ed-0206c51a3e10.png)
* **The control's job is to decode the instruction and set all the control signals to make that instruction happen

## Jumps and Branches
* Most instructions change the PC (program counter) to the next addres
  * control flow instructions can change the PC to a constant, or the value from a register, etc.
* Jumps make execution go to one specific place
  * *Are absolute - it sets the PC to a new value*
* Branches make execution go to one of two places
  * *Are relative - adds an offset to the current PC value*

## PC FSM
* **The PC FSM controls the PC and lets it advance to the next instruction, do absolute jumps, or relative branches**
  * **Branches do not have a return address
* Jumps put a value (jump target) into the PC
* Relative branches are like a number line, we want to move from one part to another
  * **Offset = destination - here (calculated by the assembler, not the CPU)**
* We use subtraction to compare values, so in order to know whether to branch or not, we have to use the ALU

## The Interconnect
* The instructions in the ISA, tell us what has to connect to what
* Is where all the wires and MUXes connect to eachother
* The interconnect lets the CPU parts combine in many ways (like a circulatory system)
  * **It moves data around**
  
# Chapter 21 - The Control

## The Control
* This is a full image of the datapath
![image](https://user-images.githubusercontent.com/122314614/234144533-2fb934ea-0acf-4c3d-bbc0-2fb4f9acabdc.png)
* This can do the work of the instructions but it can't read them
* The control is what automatically sets the control signals to the right values to make each instruction happen
  * Like the nervous system/brain of the CPU 

## Instruction Execution
* The five phases of insturction execution
  * *Fetch (IF or F) - Use PC too get the next instruction from memory (PC FSM/INSTRUCTION MEMORY)*
  * *Decode (ID or D) - look at the fetched instruction and set control signals (CONTROL)*
  * *Execute (EX or X) - Wait for data to flow through the ALU (ALU)*
  * *Memory Access (MEM or M) - If it's a load or store, do that (DATA MEMORY)*
  * *Write-back (WB or W) - If there's a destination register, write the result to it (REGISTER FILE)*
![image](https://user-images.githubusercontent.com/122314614/234146766-33012155-963a-4401-9693-d1b524bd6f41.png)
* *Single cycle machine - Each instruction takes one clock cycle to execute*
  * **Each instruciton ends on the rising edge of the clock** 
  * The control must remember which state it is in as well as what the fetched instruction was
  * A multi-cycle control unit is an FSM

## Single-cycle Control
* In a single cycle machine, there are three steps
    1. Split the encoded bitfield into its constituent values
    2. From the opcode, determine which instruction we're looking at
    3. map that instruction to a unique combination of control signals
* **All of these signals happen inside the control unit**
* **Do everything at once, but only what you need**
* The control is a boolean function that takes the instruction opcode as its input and outputs the control signals (like a truth table)
* *Decoder - takes a number and turns on one output. The rest are 0. Used for the opcode*
* *Priority Encoder - given several 1-bit inputs, it tells you which one is 1
  * **You must put a constant 1 as the first input** 
* Overall shape of the control:                                                                                             
![image](https://user-images.githubusercontent.com/122314614/234148261-2042cdb5-a90d-446d-99f7-fbbd74fa21b1.png)
 
# Chapter 22 - Performance

## Scientific Noation and SI Prefixes
* Small numbers ( 0 < x < 1) have negative exponents and large numbers have positive exponents
* **When you move the decimal point to the left, you add to the exponent**
* **When you move the decimal point to the right, you subtract from the exponent**     
![image](https://user-images.githubusercontent.com/122314614/234181689-45375a47-d76c-4f76-9bf9-524bdffc15a4.png)
* **Do not take things out of scientific notation until the end**
  * For example, when multiplying, you group the significands together, and add the exponenets of the bases
  ![image](https://user-images.githubusercontent.com/122314614/234181857-78da8e22-cb0e-4500-bc6e-465173966a2c.png)

* List of SI prefixes:                                                       
![image](https://user-images.githubusercontent.com/122314614/234181930-4ff45710-1ce3-475d-8554-05320b8fcffd.png)
* *Engineering notation - A variety of scientific notation where you keep the exponent to a multiple of 3, so it always corresponds to an SI prefix*                                                                       
![image](https://user-images.githubusercontent.com/122314614/234182090-d4bc13b9-4029-4d6e-b82b-498a1d5621b8.png)
* *Freuency - Measures how often a periodic, repeating event occurs (Hz)
  * **Frequency and time are inverses (1 Hz = 1/s)**

## Performance
* *Response Time - the length of time from start to finish*
  * Make Comparisons using the same task 
* *Throughput - the amount of work you can do in a span of time*
  * Make comparisons using the same time 
* The CPU's job is to run instructions, so we are able to finish instructions faster by:
  * Doing each instruction faster (reduce latency)
  * Executing more instructions at once (increase throughput) 

## Real-World Clocking Issues
* Propogation delay is the time it takes for a signal to pass from the inputs to the inputs
  * During the delay, the outputs are invalid, but after it, they are valid 
  * Propogation is never a fixed amount of time, can fluctuate within a range
* *Race Condition - the data and clock have a race and the outcome will depend on who wins* 

## Determining Maximum Clock Speed
* *Critical Path - the path through a circuit that requires the longest series of sequential operations*
  * They depend on each other and cannot be done in parallel 
* **The fastest we can clock a sequential circuit is the reciprocal of the critical path's propogation delay** 

## Maximum Clock Speed of a Single-Cycle CPU
* Each component has different propogation delays
  * **As for memory, it is very slow** 
* Processor Memory Performance Gap:                                                          
![image](https://user-images.githubusercontent.com/122314614/234183736-54b1353e-4be6-4858-9e6e-56a8c9839d6d.png)
* **The absolute fastest a single-cycle machine can run is 66-83 MHz)**
* **For a single cycle CPU, the clock cannot tick any faster than the instruction with the longest critical path**

# Chapter 23 - Multicycle and Microcode

## Multicycle CPUs
* Problems with single cycle CPUs:
  * The clock period cannot be any shorter than the instruction that takes the longest time (Critical Path)
    * Since memory is so slow, this means that loads and stores are the slowest instructions
  * We can't run the clock faster than memory latency  
* If the clock period is only 1 ns, what do we do about memory that takes 11 ns to respond?
  * Just wait and do nothing for multiple clock cycles  
![image](https://user-images.githubusercontent.com/122314614/234327111-8aa13ebd-b208-437c-82d1-18b65aea4eac.png)
* We can split the memory phase accross multiple clock cycles
  * Send the address to memory in M<sub>1</sub>, do nothing in M<sub>2</sub> through M<sub>11</sub>
  * The at the end of M<sub>11</sub> the loaded value is finally ready to be used in the W phase 
* All instructions will not take different numbers of cycles and thus will take different amounts of time
  * **Now the clock will only tick as slow as the longest phase of any instruction**
  ![image](https://user-images.githubusercontent.com/122314614/234328892-6ff25983-1b2b-4a0d-9633-0332f59911fb.png)
* *Pipeline Registers - Hold onto the data for the next phase*

## CPI and Performance Equation
* *CPI - How many cycles it takes to complete one instruction*
  * **Lower CPI = faster. Higher CPI = slower**
  * Best we can do is 1 CPI
* CPI is just: (number of cycles / number of instructions)
* Example:
![image](https://user-images.githubusercontent.com/122314614/234329790-8a9c8c86-af1b-4f6b-850f-d1377ca33c1f.png)
* The result is the average CPI for this program. Different mixes give different CPIs
* *Performance equation -  Measures how long it takes to finish running a program*
![image](https://user-images.githubusercontent.com/122314614/234330021-572415e4-924c-4e21-9e56-35002782578d.png)OR![image](https://user-images.githubusercontent.com/122314614/234330121-a91e15f6-4f64-4721-814b-7692725a6aa8.png)

## Multi-Cycle Control
* Now with Multi-Cyce, instruction fetch and lw/sw now happen on different cycles, transforming into a Von Neumann architecture
* In a multi-cycle implementation, control is an FSM
* Ways to implement the FSM
  * Hardwire the FSM design - write the transition table, turn it into logic, etc.
  * Put the transition table in a special kind of memory
  * Or change the contents of that meomry 

## Micorprogramming
* The control is an FSM whose transition table is in a special memory
* *Microcode - the samell programs that encode the sequence of steps for each instruction*
  * Microcode is able to reprogram the microcode rom
* *Firmware - software which serves a very important function and is hard to change*
* **Microcoded control is flexible but comes at the cost of speed**
* **Hardwired control is much faster**
  * Hardwired is faster because it is much smaller and simpler, microprogrammed contorl is bigger and more complex, which makes it slower

# Chater 24 - Pipelining, Caching, Superscalar

## Pipelining
* Hazards
  * Structural - two instructions need to use the same hardware at the same time, must both access memory at the same time
  * Data - An instruction depends on the result of the previous one
    * For instance, a sub instruction may not be able to use the value of t0 until the beginning of the next cycle as an add instruction writes its result at that step
    * We can solve this by forwarding the result of add to sub before the add completes
  * Control - You don't know the outcome of a conditional branch

## Caching
* A cache is a temporary holding area for recently used data
  * Memory access references the cache,which is a copy of the data from memory, filled in as needed
    * Load a variable the first time and a copy of it gets put into cache then intro the register. The value in the register is then modified and only stored into cahce, that way if we access it again, it will load much quicker  
* Caching exploits temporal and spacial locality
  * Temporal - variables are used over and over very quickly. Makes sense to access them somewhere fast
  * Spatial - Items in an array are all next to each other in memory. Cache will pull in several items at a tim

## Superscalar CPUs
* *scalar CPU  - can finish at most 1 instruction per cycle*
* *Superscalar CPUs - finishes more than 1 instruction per cycle*

