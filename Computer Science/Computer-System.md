# Computer System

## Table of Contents
- [Big Picture](#big-picture)
- [Computer Hardware](#computer-hardware)
  - [I/O Devices](#io-devices)
  - [Central Processing Unit(CPU)](#central-processing-unitcpu)
    - [Machine Instruction Cycle](#machine-instruction-cycle)
    - [Parts of a CPU](#parts-of-a-cpu)
    - [Number of General Purpose Processors](#number-of-general-purpose-processors)
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
#### Machine Instruction Cycle
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/machine-instruction-cycle.png" alt="Machine Instruction Cycle" width="80%" /><br />
  <em>This cycle is repeated billions of times per second.</em>
</p>

1. Fetch
    - A program is essentially a set of instructions stored in the **RAM**.
    - The CPU has registers to temporarily store the address in memory that it wants to interact with.
    - The **Program Counter** starts at 0 and copies that address to the **Memory Address Register**.
    - The **Control Unit** sends out a signal to copy the data from that address to the **Instruction Register**, effectively fetching the set of instructions.
2. Decode
    - The Control Unit parses the actual bits/instructions.
      - OptCode - instruction (Ex: add, subtract).
      - Operand - address in memory to perform that operation on.
    - The decoded information is passed as electrical signals to the relevant parts of the CPU.
3. Execute
    - The **Arithmetic Logic Unit(ALU)** performs math on the data and stores the result in memory to change the state of the program.
#### Parts of a CPU
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/CPU.png" alt="CPU" width="60%" />
</p>

##### Processor Register
- A small amount of fast storage to store data necessary for calculations.
##### Control Unit
- Responsible for controlling the order of execution of commands.
##### Combinational Logic
- Handles arithmetic and logical operations.
#### Number of General Purpose Processors
##### Single Processor System
- Previously a CPU only had one core, which meant that only one task could be executed at a time.
##### Multiprocessor System
- a.k.a. parallel systems, tightly coupled systems.
- A system that uses two or more CPUs that execute **concurrently** and share a common RAM.
  - *Multithreading on a multiprocessor increases concurrency.*
- cf. **Multicore System**
  - Contains multiple cores/processors in a single CPU.
  - Ex: dual-core simply means that there are two processors in a CPU.
- **Symmetric Multiprocessing**
  - All core processors are involved in executing the same tasks.
- **Asymmetric Multiprocessing**
  - One processor acts as a "master" and the remaining processors serve as "slaves".
  - The "master" supervises the "slaves", and the "slaves" take care of individual tasks.
##### Clustered Systems
- Use multiple CPUs to accomplish computational work.
- Two or more individual systems are coupled together.
- Can be structured symmetrically or asymmetrically.
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
[Operating system for beginners || Operating system basics - YouTube](https://www.youtube.com/watch?v=6-mdtMKfEYM&ab_channel=Geek%27sLesson)  
[Introductino to Operating System - YouTube](https://www.youtube.com/watch?v=vBURTt97EkA&list=PLBlnK6fEyqRiVhbXDGLXDk_OQAeuVcp2O&ab_channel=NesoAcademy)  
[Central processing unit - Wikipedia](https://en.wikipedia.org/wiki/Central_processing_unit)  
[Cache Memory in Computer Organization - GeeksforGeeks](https://www.geeksforgeeks.org/cache-memory-in-computer-organization/)  
[Random Access Memory (RAM) and Read Only Memory (ROM) - GeeksforGeeks](https://www.geeksforgeeks.org/random-access-memory-ram-and-read-only-memory-rom/#:~:text=ROM%20is%20further%20classified%20into,PROM%2C%20EPROM%2C%20and%20EEPROM.)  
[Page (computer memory) - Wikipedia](https://en.wikipedia.org/wiki/Page_(computer_memory))  
[Virtual Address Space (Memory Management) - Win32 apps | Microsoft Docs](https://docs.microsoft.com/en-us/windows/win32/memory/virtual-address-space)  
[CS 225 | Stack and Heap Memory](https://courses.engr.illinois.edu/cs225/sp2022/resources/stack-heap/)  
[Thread (computing) - Wikipedia](https://en.wikipedia.org/wiki/Thread_(computing))  
[Multithreading and Multiprocessing in 10 Minutes | by Kay Jan Wong | Towards Data Science](https://towardsdatascience.com/multithreading-and-multiprocessing-in-10-minutes-20d9b3c6a867)  
[How a CPU Works in 100 Seconds // Apple Silicon M1 vs Intel i9 - YouTube](https://www.youtube.com/watch?v=vqs_0W-MSB0&ab_channel=Fireship)  
