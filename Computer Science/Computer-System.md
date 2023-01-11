# Computer System

## Table of Contents
- [Big Picture](#big-picture)
- [Computer Hardware](#computer-hardware)
  - [I/O Devices](#io-devices)
  - [Central Processing Unit(CPU)](#central-processing-unitcpu)
    - [Parts of a CPU](#parts-of-a-cpu)
      - [Control Unit](#control-unit)
      - [Registers](#registers)
      - [Cache](#cache)
      - [Memory Controller](#memory-controller)
      - [Arithmetic-Logic Unit](#arithmetic-logic-unit)
      - [Wires/Buses](#wiresbuses)
    - [CPU Architecture](#cpu-architecture)
    - [Machine Instruction Cycle](#machine-instruction-cycle)
    - [Instruction Pipelining](#instruction-pipelining)
  - [Memory](#memory)
    - [Memory Hierarchy](#memory-hierarchy)
    - [Memory Segments and Addresses](#memory-segments-and-addresses)
  - [Motherboard](#motherboard)
- [System Programs and Application Programs](#system-programs-and-application-programs)
- [Operating System](#operating-system)
  - [OS Structure](#os-structure)
  - [OS Responsibilities](#os-responsibilities)
  - [Types of OS](#types-of-os)
  - [Terminology](#terminology)
  - [Process Management](#process-management)
    - [Process](#process)
    - [Thread](#thread)

## Big Picture
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/big-picture.png" alt="Big Picture" width="60%" />
</p>

## Computer Hardware
- A modern general-purpose computer system consists of one or more CPUs and a number of device controllers (Ex: disk controller, USB controller, video adapter) connected through a common bus that provides access to shared memory.
  - Each device controller is responsible for a specific type of device.
  - The CPU and device controllers need access to the memory. They are loaded onto the main memory and can execute concurrently, competing for memory cycles.
    - Ex: typing while watching a video means concurrent execution is happening.
  - The memory controller ensures orderly access to the shared memory, effectively preventing lag in concurrent execution.
### I/O Devices
- Input Ex: keyboard, mouse, microphone.
- Output Ex: monitor, speaker.
- A device controller maintains two things:
  - Local Buffer Storage
  - Set of Special Purpose Registers
- **Device Driver**
  - A plug-in module of the OS that handles the management of a particular I/O device.
  - It provides a uniform interface to the device to connect to the rest of the OS.
  - Ex: USB mouse, AMD driver (Radeon graphics card), NVDIA driver.
### Central Processing Unit(CPU)
> A **central processing unit (CPU)**, also called a **central processor**, **main processor** or just **processor**, is the electronic circuitry that executes instructions comprising a computer program. The CPU performs basic arithmetic, logic, controlling, and input/output (I/O) operations specified by the instructions in the program. | Wikipedia

- A CPU is a chip (piece of metal and silicon) that contains a lot of **transistors** (on/off switches that represent 1s and 0s).
  - To perform mathematical calculations, a CPU combines multiple transistors together to form **logic gates** (OR, NOR, AND, NAND, XOR, XNOR, Buffer, NOT). These logic gates are used to solve the computations.
    - For example, the logic gate `AND` takes two binary inputs and validate that both are true to produce true output.
- A CPU calculates the necessary computations for running applications on the computer.
  - Ex: a software written in JS, which are essentially a set of instructions, will be compiled into machine code for the CPU to execute.
- The **clock speed/rate** of a CPU is measured in GHz (billions of cycles per second).
- **Instructions Per Cycle(IPC)** refers to the number of instructions that the CPU can complete per cycle.
  - Ex: CPU A with 2.4 GHz and 2 IPC is better than CPU B with 2.4 GHz and 1 IPC.

#### Parts of a CPU
- A CPU is comprised of a **Control Unit**, **Registers**, **Cache**, **Arithmetic-Logic Unit**, and **Wires/Buses** connecting them.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/CPU.png" alt="CPU" width="60%" />
</p>

##### Control Unit
> The control unit (CU) is a component of a computer's central processing unit (CPU) that directs the operation of the processor. | Wikipedia

- The control unit is responsible for controlling the order of execution of commands and uses **control signals** to direct how the memory, ALU, and I/O devices should respond to the instructions that the processor received.
##### Registers
- **Registers** are quickly and directly accessible temporary memory in the CPU (fastest memory in a computer) where data is stored in bits/byte code.
- Some registers include: accumulator register, index register, stack pointer, program counter, memory address register, memory buffer register, command register, flag register, general-purpose registers.
- **Program Counter(PC)**
  - A register storing the address of the instruction being or to be executed.
  - It automatically increments as instructions are executed.
- **Memory Address Register(MAR)**
  - A register temporarily storing the address of the instruction stored in the Program Counter *before being outputted to the memory bus*.
- **Memory Buffer/Data Register(MBR)**
  - A register storing data being transferred to and from main memory.
  - It serves as a buffer between the CPU and main memory, *which is what allows the two to operate concurrently*.
    - The processor accesses the data in the MBR, which allows the memory unit to start bringing next data from main memory to the MBR.
- **Instruction Register(IR)**
  - A register storing the instruction that was fetched from main memory to be executed now.
  - It holds the instruction code (operation code) and the operands (data or addresses) needed for the instruction in byte code.
  - Instructions are taken from the IR one at a time by the control unit for the processor to execute.
- **Accumulator Register**
  - A general-purpose register storing the result of ALU operations.
  - It is used to hold results and pass arguments to functions.
- **Stack Pointer**
  - A register storing the address for the value that is at the top of the call stack.
##### Cache
- **Cache** memory (L1, L2 Cache) is memory intended to be used to reduce the average time that the Register needs to access data from main memory.
  - Copies of frequently used data from main memory are stored.
  - The closer the cache is to the CPU, the faster that it can be accessed.
#### Memory Controller
- The memory controller is responsible for managing communication (reading, writing, memory addressing, memory refresh) between the CPU and main memory.
- It is usually integrated inside the CPU chip and connected to other components via the system bus.
##### Arithmetic-Logic Unit
- The CPU's ALU is responsible for performing arithmetic and logic operations.
- It performs: addition, subtraction, multiplication, division, AND, OR, NOT.
- It performs the required operation upon receiving the operation code and operands from the registers.
#### Wires/Buses
- The different parts of a computer (including components of the CPU) are connected via physical wires called **buses**.
##### System Bus
- A group of wires or connections between the CPU, memory controller, main memory, and I/O devices.
- It is the combination of: the **control bus**, **address bus**, and **data bus**.
##### Control Bus
- A group of wires for transmitting **control signals** to control the CPU's operation and other components of the computer.
  - i.e. pathway used by the control unit to coordinate actions.
##### Address Bus
- A group of wires for specifying the address of the target data in main memory.
  - i.e. serves as an indicator for where to store/retrieve the data to/from in main memory.
- The address bus' width indicates the maximum number of memory locations that can be accessed (i.e. that it's aware of).
  - Ex: 32-bit address bus simply means that the computer can access any one of 2^32 (4,294,967,296) number of different memory locations (about 4GB worth of memory addresses).
- *Note*
  - *The width of the address bus does not directly relate to the size of each memory location.*
##### Data Bus
- A group of wires for transferring data between the CPU, main memory, cache, I/O device, etc.
  - i.e. data is transmitted via the data bus to/from the location specified by the address bus.
- The data bus' width indicates the maximum possible size (in bits) of a single memory access, which also indicates the maximum size of a single item in each memory location.
  - i.e. how many bits maximum can be transferred at a time.
  - Ex: a data bus that is 8 bits wide means that a single memory access can be maximum 8 bits, and therefore, each memory location can hold an 8-bit value maximum (a single character or pixel).
- Usually between 32 and 512 bits.
#### CPU Architecture
##### CPU/Processor
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
##### Core
- A processing unit of the CPU.
- A CPU may have one (**singlecore**) or more cores (**multicore**) that work in ***parallel***, each doing their own thing on their own thread(s).
- **Physical Core** vs **Logical Core**
  - Physical Core: the physical hardware on the CPU itself.
  - Logical Core: the number of pathways that the computer has to process information (i.e. threads).
  - Ex: 4 Physical Cores and 8 Threads mean that there are 8 Logical Cores.
##### Thread
- A sequence of commands given to the core to execute.
- A core may use one (**singlethread**) or more threads (**multithreading**).
  - cf. **hyperthreading/simultaneous multithreading** is when a single physical core (hardware) is split into two virtual cores, and the OS recognizes them as two separate cores.
- A core cannot work on multiple threads at a time.
  - Rather, a core can switch between processing different threads ***concurrently***. Therefore, it can efficiently switch over to another thread when the current thread is on a downtime (Ex: waiting for resources, cache, etc).
- Threads share resources (Ex: computing units, caches) of the core that they belong to.
- **Context Switching** refers to storing the state of a thread so that it can resume execution later from that state. This allows for multitasking.
#### Machine Instruction Cycle
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
#### Instruction Pipelining
> Instruction pipelining is a technique of organising the instructions for execution in such a way that the execution of the current instruction is overlapped by the execution of its subsequent instruction. Instruction pipelining improves the performance of the processor by increasing its throughput i.e. number of instructions per unit time. | Binary Terms

<p align="center">
  <img src="https://binaryterms.com/wp-content/uploads/2021/03/Instruction-Pipelining.jpg" alt="Instruction Pipelining" width="80%" /><br/>
  <em>Source: https://binaryterms.com/wp-content/uploads/2021/03/Instruction-Pipelining.jpg</em>
</p>

- IF - Instruction Fetch
- ID - Instruction Decode
- OF - Operand Fetch
- IE - Instruction Execute
- OS - Operand Store
### Memory
#### Memory Hierarchy
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/memory-hierarchy.png" alt="Memory Hierarchy" width="60%" />
</p>

- **Register**
  - Quickly accessible memory location in CPU.
  - Stores data in bits.
- **Cache Memory**
  - Memory that is used to reduce the average time for the CPU register to access data from the main memory.
  - Uses SRAM to store copies of data from frequently used main memory locations.
  - L1 cache holds cache lines retrieved from L2 cache.
  - L2 cache holds cache lines retrieved from main memory.
- **Primary/Main/Internal Memory**
  - Temporary memory that is directly accessible by the CPU.
  - Processes are loaded and executed on the RAM.
    - Ex: MS Word is stored in secondary memory but is loaded onto the RAM when opened.
  - **Random Access Memory(RAM)**
    - Volatile memory that is used for normal operations.
      - Volatile: cannot retain data without power.
    - Dynamic RAM(DRAM)
      - The most common type of computer memory widely used as a computer's main memory.
      - Has to be refreshed every few ms to retain data.
      - Low cost, high capacity, high volatility, high power consumption.
    - Static RAM(SRAM)
      - Generally used for cache memory; not common to be used for consumer applications.
      - Retains data in memory as long as power is supplied to the system.
      - High cost, high speed.
  - **Read Only Memory(ROM)**
    - Non-volatile memory that is mainly used to boot up a computer as the OS is stored in the ROM.
      - Non-volatile: can retain data even without power.
    - Programmable ROM(PROM)
      - Once programmed, the data and instructions cannot be changed.
    - Erasable Programmable ROM(EPROM)
      - Can be reprogrammed by erasing all previous data (by exposing it to ultraviolet light).
    - Eletrically Erasable Programmable ROM(EEPROM)
      - Data can be erased by applying an eletric field.
    - Mask ROM(MROM)
      - Masked off at the time of production.
- **Local Secondary/Auxiliary/External Memory**
  - Permanent memory (non-volatile) that is not directly accessible by the CPU.
  - Less expensive compared to primary memory devices.
- **Remote Secondary Memory**
  - Cloud storage.
#### Memory Segments and Addresses
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/memory-segments-and-addresses.png" alt="Process Memory Segments and Addresses" width="70%" />
</p>

- Each byte of memory is assigned an address (base 16 numbers by convention: 0x00000000 to 0xFFFFFFFF) in order to keep track of its location.
- A memory address does not represent the actual physical location in memory.
  - Instead, the OS lists and maintains address mappings in a **page table** (a.k.a. process memory table), which the CPU uses to translate virtual addresses into their corresponding physical addresses on the RAM.
  - The CPU translate the virtual address to a physical address each time a thread references an address.
- ***Each process has its own Virtual Address Space (Code, Data, Stack, Heap) and the OS manages a separate page table for each process. Therefore, each process can only access its own memory.***
  - If a process tries to use a part of the address space which is not mapped to actual RAM in the page table, the CPU triggers a hardware exception. Then the OS aborts the process with an error message called **page fault**.
    - **Page:** a fixed-length contiguous block of virtual memory, described by a single entry in the page table.
    - **Page Frame:** a fixed-length contiguous block of physical memory into which pages are mapped by the OS.
##### Virtual Address Space
- **The private set of virtual memory addresses for the particular process.**
- The virtual address spaces of processes are stored in secondary memory (disk).
  - Therefore, it can be larger than the physical memory.
###### Code/Text
- **The portion of memory for storing the code of the process to run.**
  - i.e. the source code that we wrote compiled to machine language.
- The CPU executes these codes one by one.
- It is placed in lower memory address so that it is not overwritten by heap or stack.
###### Data
- **The portion of memory for storing global and static variables.**
- Lifetime is tied to the program as memory is allocated once the process starts and deallocated once it ends.
- **2 Types of Data**
  - **Initialized Data**
    - Global and static variables that are explicitly initialized with values.
    - Variables can be changed but they can also be read-only.
      - Ex: constants.
  - **Uninitialized Data (Block Started by Symbol)**
    - Global and static variables that are not initialized with any value.
###### Heap
- a.k.a. free storage.
- **The portion of memory for storing everything else.**
- Dynamic memory allocation that occurs during program execution by the programmer.
  - Ex: `malloc`, `realloc`, `calloc`, `free` are functions used to manage heap memory in C.
- *The process must explicitly request for allocation and deallocation of space using the system call.*
  - The process specifies the size needed but the OS decides where to place it in the address space.
  - Heap chunks are not necessarily adjacent.
    - Fragmentation of the memory space can be an issue here, as the size of heap chunks that the OS can allocate shrinks.
- Deallocating unneeded heap memory is important to prevent **memory leaks**.
  - Memory leak: the memory available to the program shrinks over time.
- It is placed inbetween low and high memory addresses.
###### Stack
- **The portion of memory for storing local variables used by the process.**
- When a function is called, its local variables are stored on the stack in a grouping called a **frame**, along with the size of the frame, and the return address (the address to jump back to when execution completes).
  - A stack pointer points to the top of the stack to keep track of where to add subsequent frames.
  - Another pointer, the stack boundary, points to the bottom of the stack to keep track of the size of the stack.
  - If the stack pointer runs past the stack boundary, a hardware exception is triggered and the exception handler may increase the stack space by moving the stack boundary, or results a stack overflow.
  - Notes
    - There is no need to manually delete frames after use because they will be overwritten by subsequent frames as needed.
    - The first frame is special because the program ends when the first function returns.
- It is placed in a high memory address and grows downwards.
##### RAM and Hard Drive
- The RAM keeps in store pages belonging to processes that are currently running.
- To free up RAM, the OS can swap out pages of a process to the HD.
  - Therefore, the total memory used by all processes can exceed the RAM capacity in the system.
  - Data is temporarily copied out to a HD, hence, those data are marked **"swapped"** in the page table.
  - If the process tries to access an address in a swapped page, an exception is triggered and the OS will copy the swapped page back to RAM and update the page table before the process proceeds.
  - Swapping pages is relatively slow but it is better than not being able to continue due to lack of RAM.
### Motherboard
- Handles the connection between the CPU and the Memory.
  - A **bus** is a path on the motherboard that is used to transfer data between different parts of the motherboard.
- It contains expansion slots where other things can be inserted to improve the computer without putting more load on the CPU.
  - Ex: graphics card, sound card.
- It also contains ports where outside sources can connect to the CPU (to get/give information).
  - Ex: USB, SD card, ethernet, audio.

## System Programs and Application Programs
- System Programs: software that are used to directly give commands to the computer hardware.
  - OS is also a kind of system software
- Application Programs: programs that are used to perform a specific task and can be directly used by the user.
  - Ex: MS Word, MS Excel, Compilers, Text Editors, Web Browsers.

## Operating System
- **A program that manages the hardware and runs other programs.**
  - Ex: Windows, descendants of the Unix OS (MacOS, Linux), iOS, Android.
- **The OS acts as an interface/intermediary between the user (using a program installed on the OS) and the hardware.**
### OS Structure
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/dual-core.png" alt="Dual Core" width="60%" />
</p>

- Modern OSes are concerned with allowing multiple processes to run concurrently.
  - The problem is that each CPU core can only execute the code of one process at a time.
    - And the OS' own code cannot run on a core at the same time as any process.
    - Each process only runs on one core at a time (i.e. process A does not run simultaneously on both cores).
  - The solution: have each CPU core alternate between running each open process and alternate running processes with running OS code.
    - The **scheduler**, in the OS, runs after each process to decide what OS code to run and what process to run next.
#### Multiprogramming
- Running multiple programs by the CPU (CPU does not stay idle).
- Increase CPU utilization because the CPU is capable of executing multiple programs.
  - For example, if job A has to use some other resource (Ex: i/o device) during the course of its execution, the CPU stays idle for that time. Therefore, job A releases the CPU for another job to use in the meantime.
#### Multitasking (Time-Sharing)
- The CPU executes multiple jobs by switching among them.
- Switches occur so frequently and quickly that the users can interact with each program while it is running.
- A time-shared OS allows multiple users to share the computer simultaneously.
  - It uses CPU scheduling and multiprogramming to provide each user with a small portion of a time-shared computer.
### OS Responsibilities
#### User Interface
- Command Line Interface(CLI)
  - a.k.a. command interpreter.
  - Location
    - Some OS include the CLI in the kernel (heart of the OS).
    - Some OS treat the CLI as a special program separate from the OS.
  - Systems that can choose from multiple command interpreters refer to interpreters as **shells**.
    - Ex: BASH, Z shell, Korn shell.
    - The **Shell** is where you write code directly in the CLI.
    - The **Interpreter** is the program that interprets the code and executes it.
      - Built-in code.
        - Ex: creating a file (`touch filename.extension`).
      - Code written in certain programs.
        - The command interpreter is not aware of the command so it simply calls the program based on the entered command.
        - Ex: git, npm.
- Graphical User Interface(GUI)
#### Program Execution
- Source Code &rarr; Compiler &rarr; Object Code &rarr; Executor &rarr; Output
#### I/O Operations
- Input &rarr; CPU & Memory &rarr; Output
#### File System Manipulation
- File system for processes to read and write (CRUD).
- Access restrictions (certain files can be opened by certain programs).
#### Communications
- Communication between processes.
- The OS retains all direct control of the hardware devices. So, the processes only interact with the devices via system calls.
  - **System calls** are the means by which processes initiate requests to the OS.
    - They are **routines** at fixed addresses in the OS' portion of memory.
  - System calls allow reading/writing files, sending/receiving data over the network.
  - To use a system call, a process must use a specific CPU instruction(**syscol**).
  - The processor receives the syscol and looks it up in the system code table for the address of the system call corresponding to the number. Then it jumps execution to that address.
#### Error Detection/Handling
- Errors in CPU, memory, hardware, etc.
- Ex: no paper in printer.
#### Resource Allocation and Deallocation
- Processes share resources such as the CPU core, memory, files, and I/O devices.
- Efficiently allocate and deallocate resources (CPU, files, I/O devices, memory, etc.) to different processes.
#### Accounting
- Keep track of which users use how much and what kind of resources.
#### Protection and Security
- Data is secured and activity is protected.
- Access to resources is controlled.
  - Each process can only access its own portion of memory.
  - When multiple processes are executing at the same time, one process should not be able to interfere with another process or the OS itself.
### Types of OS
- Batch OS
- Time Sharing OS
- Distributed OS
- Network OS
- Real Time OS
- Multi Programming/Processing/Tasking OS
### Terminology
#### Bootstrap Program
- OS is stored in the ROM.
- The **boostrap program** is the first program that runs when the computer is powered up or rebooted.
  - It locates and loads the OS Kernel into the main memory to start the OS.
    - **Kernel:** the "heart"/main part of the OS.
#### Interrupt and System Call
- The hardware or software can **interrupt** the CPU, which is working on a task, to make the CPU execute another task.
  - Hardware can trigger an interrupt by using the system bus to send a signal to the CPU.
  - Software can trigger an interrupt by executing a **system call**.
- The CPU, when interrupted, stops what is is doing and immediately transfers execution to a fixed location.
  - The fixed location contains the starting address where the Interrupt Service Routine is located.
  - The **Interrupt Service Routine** contains information on the Interrupt's objective.
  - The CPU executes the task specified in the Service Routine.
  - When it is completed, it resumes the interrupted computation.
### Process Management
- For a program to execute, it needs some resources (memory, etc.) of the computer system. The OS handles this.
#### Process 
- Refers to a program that is being executed by one or many threads.
  - i.e. the resources associated with a computation.
- Consists of the code that runs the program and its activity.
- A program can multiple many processes.
#### Thread
> ...a **thread** of execution is the smallest sequence of programmed instructions that can be managed independently by a scheduler, which is typically a part of the operating system. | Wikipedia

- A thread is the unit of execution within a process.
- It consists of the values of the CPU's registers.
- A CPU makes you think that it is capable of doing multiple computation at the same time. However, that is not true. The CPU goes back and forth, spending a bit of time on each computation/process. It is able to go back and forth between processes because it has an execution context (threads) for each computation.
##### Single-threaded
- There is only one thread in a process.
- Ex: if the thread is rendering the page, it will not be able to download a file.
- A single-threaded process can only run on one CPU (even if running on a multiprocessor).
##### Multi-threaded
- Allows multiple tasks to be executed **concurrently** in a single process.
  - Each thread is assigned a different task in the process.
  - Ex: one thread could be responsible for rendering the page, and another thread could be responsible for downloading a file.
  - **Concurrently** means that a single thread will be executing at a time but the processor goes back and forth so quickly that it seems that threads can run simultaneously.
- cf. [**Multiprocessing**](https://github.com/Kakamotobi/Learned/main/Computer%20Science/Computer-System.md#multiprocessor-system).
- **Benefits of Multi-threading**
  - Responsiveness
    - May allow a program to continue running even if a part of it is lagging or blocked.
  - Resource sharing
    - Threads share the memory and the resources (code, data, files) of the process that they belong to.
      - i.e. within the same address space.
    - Therefore is efficient.
#### Process and Thread Analogy
- A book is a process. A bookmark is a thread.
- If there are multiple bookmarks in a book, it is a multi-threaded process.

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

[Random Access Memory (RAM) and Read Only Memory (ROM) - GeeksforGeeks](https://www.geeksforgeeks.org/random-access-memory-ram-and-read-only-memory-rom/#:~:text=ROM%20is%20further%20classified%20into,PROM%2C%20EPROM%2C%20and%20EEPROM.)  
[Page (computer memory) - Wikipedia](https://en.wikipedia.org/wiki/Page_(computer_memory))  
[Virtual Address Space (Memory Management) - Win32 apps | Microsoft Docs](https://docs.microsoft.com/en-us/windows/win32/memory/virtual-address-space)  
[CS 225 | Stack and Heap Memory](https://courses.engr.illinois.edu/cs225/sp2022/resources/stack-heap/)  

[Operating system for beginners || Operating system basics - YouTube](https://www.youtube.com/watch?v=6-mdtMKfEYM&ab_channel=Geek%27sLesson)  
[Introduction to Operating System - YouTube](https://www.youtube.com/watch?v=vBURTt97EkA&list=PLBlnK6fEyqRiVhbXDGLXDk_OQAeuVcp2O&ab_channel=NesoAcademy)  
