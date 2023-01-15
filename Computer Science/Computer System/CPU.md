# Central Processing Unit(CPU)

## Table of Contents
- [What is a Central Processing Unit(CPU)?](#what-is-a-central-processing-unitcpu)
- [Parts of a CPU](#parts-of-a-cpu)
  - [Control Unit](#control-unit)
  - [Registers](#registers)
  - [Cache](#cache)
  - [Memory Controller](#memory-controller)
  - [Arithmetic-Logic Unit](#arithmetic-logic-unit)
  - [Wires/Buses](#wiresbuses)
- [CPU Architecture](#cpu-architecture)
  - [CPU/Processor](#cpuprocessor)
  - [Core](#core)
  - [Thread](#thread)
- [Instructions](#instructions)
  - [Machine Instruction Cycle](#machine-instruction-cycle)
  - [Instruction Pipelining](#instruction-pipelining)
- [CPU Performance](#cpu-performance)
  - [Performance Metrics](#performance-metrics)
    - [Clock](#clock)
    - [Million Instructions per Second(MIPS)](#million-instructions-per-secondmips)
    - [Response/Elapsed Time](#responseelapsed-time)
  - [Considerations for CPU Performance](#considerations-for-cpu-performance)

## What is a Central Processing Unit(CPU)?
> A **central processing unit (CPU)**, also called a **central processor**, **main processor** or just **processor**, is the electronic circuitry that executes instructions comprising a computer program. The CPU performs basic arithmetic, logic, controlling, and input/output (I/O) operations specified by the instructions in the program. | Wikipedia

- A CPU is a chip (piece of metal and silicon) that contains a lot of **transistors** (on/off switches that represent 1s and 0s).
  - To perform mathematical calculations, a CPU combines multiple transistors together to form **logic gates** (OR, NOR, AND, NAND, XOR, XNOR, Buffer, NOT). These logic gates are used to solve the computations.
    - For example, the logic gate `AND` takes two binary inputs and validate that both are true to produce true output.
- A CPU calculates the necessary computations for running applications on the computer.
  - Ex: a software written in JS, which are essentially a set of instructions, will be compiled into machine code for the CPU to execute.
- The **clock speed/rate** of a CPU is measured in GHz (billions of cycles per second).
- **Instructions Per Cycle(IPC)** refers to the number of instructions that the CPU can complete per cycle.
  - Ex: CPU A with 2.4 GHz and 2 IPC is better than CPU B with 2.4 GHz and 1 IPC.

## Parts of a CPU
- A CPU is comprised of a **Control Unit**, **Registers**, **Cache**, **Arithmetic-Logic Unit**, and **Wires/Buses** connecting them.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/CPU.png" alt="CPU" width="60%" />
</p>

### Control Unit
> The control unit (CU) is a component of a computer's central processing unit (CPU) that directs the operation of the processor. | Wikipedia

- The control unit is responsible for controlling the order of execution of commands and uses **control signals** to direct how the memory, ALU, and I/O devices should respond to the instructions that the processor received.
  - i.e. it is the one operating the **machine instruction cycle**.
### Registers
- **Registers** are quickly and directly accessible temporary memory in the CPU (fastest memory in a computer) where data is stored in bits/byte code.
- Some registers include: accumulator register, index register, stack pointer, program counter, memory address register, memory buffer register, command register, flag register, general-purpose registers.
#### Program Counter(PC)
- A register storing the address of the instruction being or to be executed.
- It automatically increments as instructions are executed.
#### Memory Address Register(MAR)
- A register temporarily storing the address of the instruction stored in the Program Counter _before being outputted to the memory bus_.
#### Memory Buffer Register(MBR)
- a.k.a. Memory Data Register(MDR)
- A register storing data being transferred to and from main memory.
- It serves as a buffer between the CPU and main memory, _which allows the two to operate concurrently_.
  - The processor accesses the data in the MBR, which allows the memory unit to start bringing next data from main memory to the MBR.
#### Instruction Register(IR)
- a.k.a. Current Instruction Register(CIR)
- A register storing the instruction that was fetched from main memory to be executed now.
- It holds the instruction code (opcode) and the operands (data or addresses) needed for the instruction in binary code.
- Instructions are taken from the IR one at a time by the control unit for the processor to execute.
#### Accumulator Register(ACC)
- A general-purpose register storing the result of ALU operations.
- It is used to hold results and pass arguments to functions.
#### Stack Pointer
- A register storing the address for the value that is at the top of the call stack.
### Cache
- **Cache** (L1, L2 Cache) is memory intended to be used to reduce the average time that the Register needs to access data from main memory.
  - Copies of frequently used data from main memory are stored.
  - The closer the cache is to the CPU, the faster that it can be accessed.
### Memory Controller
- The memory controller is responsible for managing communication (reading, writing, memory addressing, memory refresh) between the CPU and main memory.
- It is usually integrated inside the CPU chip and connected to other components via the system bus.
### Arithmetic-Logic Unit
- The CPU's ALU is responsible for performing arithmetic and logic operations.
- It performs: addition, subtraction, multiplication, division, AND, OR, NOT.
- It performs the required operation upon receiving the operation code and operands from the registers.
### Wires/Buses
- The different parts of a computer (including components of the CPU) are connected via physical wires called **buses**.
#### System Bus
- A group of wires or connections between the CPU, memory controller, main memory, and I/O devices.
- It is the combination of: the **control bus**, **address bus**, and **data bus**.
#### Control Bus
- A group of wires for transmitting **control signals** to control the CPU's operation and other components of the computer.
  - i.e. pathway used by the control unit to coordinate actions.
#### Address Bus
- A group of wires for specifying the address of the target data in main memory.
  - i.e. serves as an indicator for where to store/retrieve the data to/from in main memory.
- The address bus' width indicates the maximum number of memory locations that can be accessed (i.e. that it's aware of).
  - Ex: 32-bit address bus simply means that the computer can access any one of 2^32 (4,294,967,296) number of different memory locations (about 4GB worth of memory addresses).
- *Note*
  - *The width of the address bus does not directly relate to the size of each memory location.*
#### Data Bus
- A group of wires for transferring data between the CPU, main memory, cache, I/O device, etc.
  - i.e. data is transmitted via the data bus to/from the location specified by the address bus.
- The data bus' width indicates the maximum possible size (in bits) of a single memory access, which also indicates the maximum size of a single item in each memory location.
  - i.e. how many bits maximum can be transferred at a time.
  - Ex: a data bus that is 8 bits wide means that a single memory access can be maximum 8 bits, and therefore, each memory location can hold an 8-bit value maximum (a single character or pixel).
- Usually between 32 and 512 bits.

## CPU Architecture
### CPU/Processor
- A computer may have one or more CPUs (**multiprocessing**).
  - **Symmetric Multiprocessing**
    - All processors are involved in executing the same tasks.
  - **Asymmetric Multiprocessing**
    - One processor acts as a "master" and the remaining processors serve as "slaves".
    - The "master" supervises the "slaves", and the "slaves" take care of individual tasks.
  - **Clustered Systems**
    - Use multiple CPUs to accomplish computational work.
    - Two or more individual systems are coupled together.
    - Can be structured symmetrically or asymmetrically.
- **Word**
  - A **Word** is the fixed unit of data that can be addressed and moved between the processor and memory.
    - Ex: a 32-bit processor means the word is 32 bits. Therefore, it is capable of processing 4 bytes at a time from memory.
    - Ex: a 64-bit processor means the word is 64 bits. Therefore, it is capable of processing 8 bytes at a time from memory.
  - All incoming data to the processor that is smaller than the **word** is **padded** to match the word (to make it more feasible for the processor to handle).
    - Ex: in a 32-bit processor, if an incoming data is less than 4 bytes, the data needs to be padded to 4 bytes first.
  - *Note*
    - A 64-bit processor means that it has 64-bit registers that hold memory addresses, and a 64-bit data bus to transfer data between the registers and main memory.
      - If the data is larger than the data bus, it will have to be retrieved in more than one trip.
    - Word is not necessarily the same as the register size.
      - Ex: a register that has a size of 8 bytes could have a word of 4 bytes or 8 bytes.
### Core
- A processing unit of the CPU.
- A CPU may have one (**singlecore**) or more cores (**multicore**) that work in ***parallel***, each doing their own thing on their own thread(s).
- **Physical Core** vs **Logical Core**
  - Physical Core: the physical hardware on the CPU itself.
  - Logical Core: the number of pathways that the computer has to process information (i.e. threads).
  - Ex: 4 Physical Cores and 8 Threads mean that there are 8 Logical Cores.
### Thread
- A sequence of commands given to the core to execute.
- A core may use one (**singlethread**) or more threads (**multithreading**).
  - cf. **hyperthreading/simultaneous multithreading** is when a single physical core (hardware) is split into two virtual cores, and the OS recognizes them as two separate cores.
- A core cannot work on multiple threads at a time.
  - Rather, a core can switch between processing different threads ***concurrently***. Therefore, it can efficiently switch over to another thread when the current thread is on a downtime (Ex: waiting for resources, cache, etc).
- Threads share resources (Ex: computing units, caches) of the core that they belong to.
- **Context Switching** refers to storing the state of a thread so that it can resume execution later from that state. This allows for multitasking.

## Instructions
- An instruction is a binary number that contains the operation code and operands required to complete the operation.
  - Ex: `opCode operand1 operand2 operand3`
- Some operations are: load, store, move, add, subtract, and, or.
- Some operations like branch(B), and branch with link (BL) can jump to another command in another memory adderss (instead of continuing to the next in line in memory).
  - This leads to **pipeline bubbling** where some CPU clocks have to discarded since the computations done in the bubble cannot be used.
  - Therefore, it is ideal to reduce as much statements such as `if...else`.
### Machine Instruction Cycle
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/machine-instruction-cycle.png" alt="Machine Instruction Cycle" width="80%" /><br />
  <em>This cycle is repeated billions of times per second.</em>
</p>

1. **Fetch**
    - The **control unit** looks at the **program counter** for the address to retrieve the next instruction/command from in the main memory. The retrieved instruction is stored in the **instruction register**.
    - Explanation
      - A program is essentially a set of instructions stored in the **RAM**.
      - The CPU has **registers** to temporarily store the address in memory that it wants to interact with.
      - The **Program Counter** starts at 0 and the address that it is pointing to is copied over to the **Instruction Register**, effectively fetching the instruction.
2. **Decode**
    - The **control unit** uses the **instruction decoder** to decode the command (from byte code to assembly) that is currently in the **instruction register** to figure out what to do.
    - After decoding, the **control unit** sends out the decoded information through electrical signals to the relevant parts of the CPU.
      - Ex: retrieve the operands (data or addresses) needed for executing the instruction from the corresponding registers.
    - Explanation
      - The **control unit** parses the actual instruction expressed in bits to figure out the **operation code** and any **operands**.
      - This decoded information is then passed as electrical signals to the relevant parts of the CPU.
3. **Execute**
    - Depending on the instruction, the **control unit** sends signals to the **ALU** to perform an arithmetic operation OR sends signals to the **memory unit** to transfer data from/to main memory to/from a register.
4. Store
    - If needed, store the result in main memory (state of the program).
### Instruction Pipelining
> Instruction pipelining is a technique of organising the instructions for execution in such a way that the execution of the current instruction is overlapped by the execution of its subsequent instruction. Instruction pipelining improves the performance of the processor by increasing its throughput i.e. number of instructions per unit time. | Binary Terms

- Allows operations to run concurrently.
- There can be multiple pipelining in each core in a CPU.

<p align="center">
  <img src="https://binaryterms.com/wp-content/uploads/2021/03/Instruction-Pipelining.jpg" alt="Instruction Pipelining" width="80%" /><br/>
</p>

- IF - Instruction Fetch
- ID - Instruction Decode
- OF - Operand Fetch
- IE - Instruction Execute
- OS - Operand Store


## CPU Performance
### Performance Metrics
#### Clock
> The clock sends out a regular electrical pulse which synchronises (keeps in time) all the components. | BBC

- A small cylinder-like component next to a mercury battery on the mainboard.
- **Clock Speed**
  - The number of times that the clock pulses (clock cycle) per second is the clock's speed in Hz.
    - Ex: 3.6GHz means that the clock pulses 3,600,000,000 times per second.
  - One **clock cycle** is roughly equivalent to one unit of work that the processor performs.
    - Executing one command requires about 4 to 5 blocks.
  - Therefore, the clock speed of a CPU is an indicator of how fast the CPU is able to process operations required for instructions.
    - A 3.6GHz processor is able to compute approximately 13 billion more operations than a 2.3GHz processor.
  - The memory and CPU have different clocks.
    - Ex: a CPU may be 3.6GHz while the memory be 2.1GHz.
#### Million Instructions per Second(MIPS)
- The number of instructions that the processor executes per second.
- Since different instructions take different amounts of time, MIPS is not absolute and is not a constant metric. It is merely a vague reference to the CPU's performance.
- Different CPUs cannot be compared to each other based on MIPS.
- *Note*
  - 3.6GHz means the processor executes 36 billion operations (not instructions) per second.
#### Response/Elapsed Time
- The time it took from start to finish is never constant.
- There are many variables that can affect the time (Ex: OS, I/O delay, other programs using the CPU).
- CPU resources are used by not only the programs but also the OS at the same time.
### Considerations for CPU Performance
- Reduce program size.
- Improve Cycles Per Instruction(CPI) by minimizing bubbles.
  - Avoid using lots of function calls, `if...else` statements.
  - Write code so that the CPU can easily predict what operations it needs to do (Ex: keep `true` and only `false` at the end).
- Lots of memory access means higher space complexity, which also means the CPU needs more time to go back and forth to the memory.
- Maximizing speed results to less throughput.
- Maximizing throughput results to slow speed.
- Therefore, we need to consider what is more efficient for the demand.
- **Amdahl's Law**
  - Simply improving one component does not necessarily mean the whole thing can

## Reference
[Central processing unit - Wikipedia](https://en.wikipedia.org/wiki/Central_processing_unit)  
[Computer architecture - Wikipedia](https://en.wikipedia.org/wiki/Computer_architecture)  
[Control unit - Wikipedia](https://en.wikipedia.org/wiki/Control_unit)  
[Introduction of Control Unit and its Design - GeeksforGeeks](https://www.geeksforgeeks.org/introduction-of-control-unit-and-its-design/)  
[Cache Memory in Computer Organization - GeeksforGeeks](https://www.geeksforgeeks.org/cache-memory-in-computer-organization/)  
[What is Instruction Pipelining? Introduction, Hazards and Advantages - Binary Terms](https://binaryterms.com/instruction-pipelining.html)  

[Thread (computing) - Wikipedia](https://en.wikipedia.org/wiki/Thread_(computing))  
[Multithreading and Multiprocessing in 10 Minutes | by Kay Jan Wong | Towards Data Science](https://towardsdatascience.com/multithreading-and-multiprocessing-in-10-minutes-20d9b3c6a867)  
[How a CPU Works in 100 Seconds // Apple Silicon M1 vs Intel i9 - YouTube](https://www.youtube.com/watch?v=vqs_0W-MSB0&ab_channel=Fireship)  
[Single Core vs Multi Core - Which is more important? A CPU primer. - YouTube](https://www.youtube.com/watch?v=D8ooMQG0T4k&ab_channel=ConstantGeekery)  
[Common CPU components - Architecture - Eduqas - GCSE Computer Science Revision - Eduqas - BBC Bitesize](https://www.bbc.co.uk/bitesize/guides/zhppfcw/revision/2)  
