# Computer System

## Table of Contents
- [Big Picture](#big-picture)
- [Computer Hardware](#computer-hardware)
  - [Motherboard](#motherboard)
  - [I/O Devices](#io-devices)
  - [Central Processing Unit(CPU)](#central-processing-unitcpu)
  - [Memory](#memory)
- [System Programs and Application Programs](#system-programs-and-application-programs)
- [Operating System](#operating-system)
  - [OS Structure](#os-structure)
  - [OS Responsibilities](#os-responsibilities)
  - [Types of OS](#types-of-os)
  - [Terminology](#terminology)
  - [Process Management](#process-management)

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
### Motherboard
- Handles the connection between the CPU and the Memory.
  - A **bus** is a path on the motherboard that is used to transfer data between different parts of the motherboard.
- It contains expansion slots where other things can be inserted to improve the computer without putting more load on the CPU.
  - Ex: graphics card, sound card.
- It also contains ports where outside sources can connect to the CPU (to get/give information).
  - Ex: USB, SD card, ethernet, audio.
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
### [Central Processing Unit(CPU)](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Computer%20System/CPU.md)
> A **central processing unit (CPU)**, also called a **central processor**, **main processor** or just **processor**, is the electronic circuitry that executes instructions comprising a computer program. The CPU performs basic arithmetic, logic, controlling, and input/output (I/O) operations specified by the instructions in the program. | Wikipedia

- A CPU is a chip (piece of metal and silicon) that contains a lot of **transistors** (on/off switches that represent 1s and 0s).
  - To perform mathematical calculations, a CPU combines multiple transistors together to form **logic gates** (OR, NOR, AND, NAND, XOR, XNOR, Buffer, NOT). These logic gates are used to solve the computations.
    - For example, the logic gate `AND` takes two binary inputs and validate that both are true to produce true output.
- A CPU calculates the necessary computations for running applications on the computer.
  - Ex: a software written in JS, which are essentially a set of instructions, will be compiled into machine code for the CPU to execute.
- The **clock speed/rate** of a CPU is measured in GHz (billions of cycles per second).
- **Instructions Per Cycle(IPC)** refers to the number of instructions that the CPU can complete per cycle.
  - Ex: CPU A with 2.4 GHz and 2 IPC is better than CPU B with 2.4 GHz and 1 IPC.
### [Memory](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Computer%20System/Memory.md)
> In computing, memory is a device or system that is used to store information for immediate use in a computer or related computer hardware and digital electronic devices. | Wikipedia

- There are many types of memory in a computer. However, it is usually used in reference to main memory.

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
### [Process Management](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Computer%20System/Process-Management.md)
- For a program to execute, it needs some resources (memory, etc.) of the computer system. The OS handles this.

## Reference
[Central processing unit - Wikipedia](https://en.wikipedia.org/wiki/Central_processing_unit)  
[Computer memory - Wikipedia](https://en.wikipedia.org/wiki/Computer_memory)  
[Operating system for beginners || Operating system basics - YouTube](https://www.youtube.com/watch?v=6-mdtMKfEYM&ab_channel=Geek%27sLesson)  
[Introduction to Operating System - YouTube](https://www.youtube.com/watch?v=vBURTt97EkA&list=PLBlnK6fEyqRiVhbXDGLXDk_OQAeuVcp2O&ab_channel=NesoAcademy)  
