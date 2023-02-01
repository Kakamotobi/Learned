# Memory

## Table of Contents
- [What is Memory?](#what-is-memory)
- [Memory Hierarchy](#memory-hierarchy)
- [Virtual Memory](#virtual-memory)
  - [Virtual Memory Layout](#virtual-memory-layout)
    - [Text Segment](#text-segment)
    - [Data Segment](#data-segment)
    - [Block Starting Symbol(BSS) Segment](#block-starting-symbolbss-segment)
    - [Heap Segment](#heap-segment)
      - [JS Heap Structure](#js-heap-structure)
    - [Stack Segment](#stack-segment)
      - [Stack Pointers](#stack-pointers)
      - [How the Stack Segment Works](#how-the-stack-segment-works)
- [Memory Management](#memory-management)
  - [Manual Memory Management](#manual-memory-management)
  - [Garbage Collection](#garbage-collection)
- [V8 Memory Structure](#v8-memory-structure)
  - [Resident Set](#resident-set)
      - [Stack](#stack)
      - [Heap](#heap)
        - [Components of Heap](#components-of-heap)
        - [Page](#page)
        - [V8 Garbage Collectors](#v8-garbage-collectors)

## What is Memory?
> In computing, memory is a device or system that is used to store information for immediate use in a computer or related computer hardware and digital electronic devices. | Wikipedia

## Memory Hierarchy
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/memory-hierarchy.png" alt="Memory Hierarchy" width="60%" />
</p>

### Register
- Quickly accessible memory location in CPU.
- Stores data in bits.
### Cache
- Memory that is used to reduce the average time for the CPU register to access data from the main memory.
- Uses SRAM to store copies of data from frequently used main memory locations.
- L1 cache holds cache lines retrieved from L2 cache.
- L2 cache holds cache lines retrieved from main memory.
### Primary/Main/Internal Memory
- Temporary memory that is directly accessible by the CPU.
- Processes are loaded and executed on the RAM.
  - Ex: MS Word is stored in secondary memory but is loaded onto the RAM when opened.
#### Random Access Memory(RAM)
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
#### Read Only Memory(ROM)
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
### Local Secondary/Auxiliary/External Memory
  - Permanent memory (non-volatile) that is not directly accessible by the CPU.
  - Less expensive compared to primary memory devices.
### Remote Secondary Memory
  - Cloud storage.

## Virtual Memory
- a.k.a. Virtual Address Space for a Process
- **The memory layout of a process refers to the process-specific memory management on the virtual memory level (not the physical memory).**
- When the OS loads a new process, it allocates a certain amount (about 2GB to 4GB nowadays) of main memory for that process.
  - This memory may be in different locations across the physical main memory. However, the OS generates a **virtual memory space** for the process.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/refImg/memory-segments-and-addresses.png" alt="Process Memory Segments and Addresses" width="80%" />
</p>

- The address on the virtual memory ranges from 0x00000000 to 0xFFFFFFFF by convention.
  - A memory address does not represent the actual physical location in memory.
  - Instead, the OS lists and maintains address mappings in a **page table** (a.k.a. process memory table), which the CPU uses to translate virtual addresses into their corresponding physical addresses on the RAM.
  - The CPU translates the virtual address to a physical address each time a thread references an address.
  - If a process tries to use a part of the address space which is not mapped to actual RAM in the page table, the CPU triggers a hardware exception. Then the OS aborts the process with an error message called page fault.
    - Page: a fixed-length contiguous block of virtual memory, described by a single entry in the page table.
    - Page Frame: a fixed-length contiguous block of physical memory into which pages are mapped by the OS.
- The RAM keeps in store pages belonging to processes that are currently running.
  - To free up RAM, the OS can **swap** out pages of a process to the HD.
  - *Therefore, the total memory used by all processes can exceed the RAM capacity in the system.*
  - Data is temporarily copied out to a HD, hence, those data are marked "swapped" in the page table.
  - If the process tries to access an address in a swapped page, an exception is triggered and the OS will copy the swapped page back to RAM and update the page table before the process proceeds.
  - *Swapping pages is relatively slow but it is better than not being able to continue due to lack of RAM.*

### Virtual Memory Layout
- The virtual memory space is divided into segments: **stack**, **heap**, **BSS**, **GVAR**, and **text**.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/memory-layout-of-a-process.png" alt="Memory Layout of a Process" width="80%" />
</p>

#### Text Segment
- **The portion of memory that contains the executable instructions (binary) of the program.**
  - i.e. the source code that is compiled to machine code.
- Once loaded, this portion usually remains unchanged.
  - It is usually set to read-only/execute to prevent accidental modifications of the program's instructions.
- The CPU executes these instructions one by one.
- It is placed in lower memory address so that it is not overwritten by the heap or stack.
#### Data Segment
- **The portion of memory that contains explicitly initialized global and static variables.**
  - i.e. if the initial value is a value other than 0.
- Since these variables are susceptible to change, this portion is usually not read-only.
- Example
  ```js
  const iAmAGlobalVariable = "hello";
  ```
#### Block Starting Symbol(BSS) Segment
- **The portion of memory that contains uninitialized global and static variables.**
  - i.e. if the initial value is 0 or not explicitly initialized in source code.
- Example
  ```js
  let iAmGlobalVariable;
  ```
#### Heap Segment
- **The portion of memory that is allocated dynamically to variables whose size can only be known at run time (cannot be determined at compile time).**
  - i.e. store **reference types** (Ex: objects, arrays, functions) and data whose size is not fixed.
  - It is managed through `malloc`, `new`, etc.
- The process must explicitly request for allocation and deallocation of space in this segment using the system call.
  - The process specifies the size needed but the OS decides where to place it in the address space.
  - Heap chunks are not necessarily adjacent.
- The stack pointer variable is often used to reference the address of memory space that was allocated to the heap segment since they both grow into the unused memory portion.
##### JS Heap Structure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/js-heap-structure.png" alt="JS Heap Structure" width="80%" />
</p>

#### Stack Segment
- **The portion of memory that is allocated statically to stack frames.**
  - A **stack frame** is the set of values that a function being called needs.
    - These values include:
      - Local variables declared in the function.
        - _In JS, only primitive types are stored in the stack frame. Reference types are stored in the heap segment and the reference is stored in the stack frame._
      - Arguments that were passed in to the function.
      - The return address where the program should continue executing when the current function's execution completes.
        - i.e. the address of the instruction immediately following the return of the current function call.
  - _Details differ by implementation._
##### Stack Pointers
- The **stack pointer** tracks the stack to indicate where to add subsequent frames, which also indicates how much of the stack segment is being used right now.
  - **Extended Stack Pointer(ESP)**
    - a.k.a. Stack Pointer.
    - The pointer(register) that always **indicates the top stack**.
  - **Extended Base Pointer(EBP)**
    - a.k.a. Frame Pointer.
    - The pointer(register) that **indicates the base address for the current stack frame**.
    - Offsets from EBP is used to access the data stored in the stack.
    - When a new function is called, the EBP of the previous stack frame is pushed onto the stack first.
      - This is so that when the new function returns, the EBP is reassigned to the previous stack frame's EBP.
    - When the current stack frame is done executing, the values stored between the ESP and the EBP is popped off the stack.
- **Extended Instruction Pointer(EIP)**
  - The pointer(register) that **indicates the memory address for the next instruction to be executed**.
  - In a stack frame, it essentially represents the **return address** (*in the Text/Code segment*) to which the execution should resume after the current function completes.
##### How the Stack Segment Works
1. **Prologue - a function is called**
    - The function's **arguments** are pushed onto the stack.
    - **The EIP's current value is pushed onto the stack as "Previous/Old EIP"** (a.k.a. return address, return instruction pointer).
      - **The EIP is then updated to the address of the callee function** (the function that is being pushed onto the stack to be executed).
    - **The EBP's current value is pushed onto the stack as "Previous/Old EBP"** (a.k.a. "saved frame pointer").
      - **The EBP is then updated to the ESP's current value**, which is the start of the new stack frame.
        - Previous EIP and Previous EBP are technically not in the new stack frame as per the new EBP points above it on the stack.
      - EBP and ESP are pointing to the same place at this point.
    - *Note*
      - *All while anything is pushed onto the stack, ESP adjusts accordingly to always point to the top of the stack.*
2. **The function executes**
    - The function's **local variables** are pushed onto the stack.
    - Variables that are **reference types** are stored in the **heap**.
      - The reference itself is stored in the stack.
3. **Epilogue - the function returns**
    - The **ESP is updated to the EBP's current value**.
    - The **top stack, which is currently the Previous EBP, is popped off**, and **EBP is updated to that value**.
      - *When popped off, ESP is increased.*
    - The **top stack, which is currently the Previous EIP, is popped off**, and **EIP is updated to that value**.
    - At this point, **the top stack is currently the arguments are discarded by incrementing ESP**.
      - *There is no need to manually delete contents of frames after use because they will be overwritten by subsequent frames as needed.*

## Memory Management
- Memory management usually refers to managing _heap memory_.
- Since heap chunks are not strictly adjacent, **fragmentation** of memory space can be an issue.
  - The size of heap chunks that the OS can allocate shrinks.
  - Therefore, deallocating unneeded heap memory is important to prevent **memory leaks**.
    - Memory Leak: when memory that is no longer needed is not being released.
### Manual Memory Management
- Some languages like C an C++ provide methods for the developer to manually manage memory.
  - Ex: `malloc`, `realloc`, `calloc`, `free`.
### Garbage Collection
- Some languages like JavaScript and Java provide a **garbage collector** that automatically frees up any memory allocations that are not used anymore.
#### Mark and Sweep Algorithm
- This garbage collection algorithm, periodically, starts from the global object and down the tree to mark objects that are still reachable (i.e. being referenced) as "alive", and sweep those that are not being referenced.
- However efficient this algorithm may be, developers still cannot manually free up any space and have to rely on the algorithm.
#### Reference Counting Algorithm
- This garbage collection algorithm assigns every object a reference count, which refers to the number of variables that are referencing it. When the reference count becomes 0 (no variable is pointing to it so the reference can no longer be accessed), the space is freed up.
- **Circular References**
  - When two objects reference each other and even though we can no longer access them, the algorithm is unable to identify whether the objects should be freed up or not, which leads to memory leaks.

## V8 Memory Structure
### Resident Set
- **Resident Set** is the portion of (virtual) memory that is being used by a V8 process.
  - It can be used to check the memory usage, memory leaks of a V8 process.
  - It is basically the **heap** and **stack**.

<p align="center">
  <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--J4DjsB_m--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://i.imgur.com/kSgatSL.png" alt="V8 Resident Set" width="80%" />
</p>

#### Stack
- Consist of stack frames of the instruction being executed.
- The stack is automatically managed by the OS rather than V8.
#### Heap
- V8 stores all reference types in the heap.
- The heap is managed by the garbage collector.
- The heap consists of different parts.
##### Components of Heap
###### New Space
- a.k.a. young generation.
- This is a small space with two semi-spaces (**to-space** and **from-space**) where new objects are stored.
- This portion of the heap is managed by the **Minor GC (Scavenger)**.
###### Old Space
- a.k.a. old generation.
- This is where objects that have survived the Scavenger in the New Space are stored.
  - It consists of an **Old Pointer Space** and an **Old Data Space**.
    - Objects with pointers to other objects are stored in the **Old Pointer Space**.
    - Objects that do not have pointers to other objects are stored in the **Old Data Space**.
- This portion of the heap is managed by the **Major GC**.
###### Large Object Space
- This is where objects that do not fit in other spaces are stored.
###### Code-Space
- This is where the JIT compiler stores blocks of compiled code.
###### Cell Space, Property Cell Space, Map Space
- These are where `Cells`, `PropertyCells`, and `Maps` are stored, respectively.
- They contain same-sized objects.
##### Page
- Each space listed above is composed of a set of pages.
- A **Page** is a contiguous chunk of memory (1MB but larger for Large Object Space) that is allocated by the OS.
##### V8 Garbage Collectors
###### Minor GC (Scavenger)
- The Major GC is responsible for managing the **young generation space**.
- It uses Cheney's algorithm and is very quick.
- How It Works
  - **First GC Cycle**
    - New objects are allocated to **From-Space**. When the allocation pointer reaches the end, the Minor GC starts scanning everything in **From-Space**.
    - Objects that survive this cycle are moved to **To-Space**.
      - At this point, **To-Space** is compact and fragmentation is minimized, and **From-Space** is emptied.
    - All objects in the **To-Space** are moved over back to the **From-Space**.
  - **Second GC Cycle**
    - When there is no more space in the **From-Space**, the Minor GC starts the second cycle of scanning.
    - Objects that are experiencing the cycle for the first time are moved to the **To-Space**.
    - Objects that are on their second cycle are moved to **Old Space**.
    - All objects in the **To-Space** are moved over back to the **From-Space**.
###### Major GC
- The Major GC is responsible for managing the **old generation space**.
- It uses the Mark-Sweep-Compact algorithm.
  - This algorithm is a three-step process that uses a white-grey-black marking system.
  - This GC is a heavy process that pauses the whole program for a bit.
- How It Works
  - V8 triggers the Major GC when **Old Space** is almost full.
  - **Marking**
    - The GC traverses (dfs) the objects and marks objects that are reachable as "alive".
  - **Sweeping**
    - The GC traverses the objects and gathers a **free-list** indicating which memory gaps are free to use.
      - Later when allocating memory, the free-list can be looked up for an appropriately sized chunk of memory.
  - **Compacting**
    - If required, the GC reduces fragmentation by moving all objects that have survived to be compact.
- Some Ways that V8 uses to Optimize
  - Incremental GC
  - Concurrent marking
  - Concurrent sweeping/compacting
  - Lazy sweeping

## Reference
[Computer memory - Wikipedia](https://en.wikipedia.org/wiki/Computer_memory)  
[Random Access Memory (RAM) and Read Only Memory (ROM) - GeeksforGeeks](https://www.geeksforgeeks.org/random-access-memory-ram-and-read-only-memory-rom/#:~:text=ROM%20is%20further%20classified%20into,PROM%2C%20EPROM%2C%20and%20EEPROM.)  
[What is flash storage? | IBM](https://www.ibm.com/topics/flash-storage)  

[Virtual Memory, Paging, and Swapping « Gabriele Tolomei](https://gabrieletolomei.wordpress.com/miscellanea/operating-systems/virtual-memory-paging-and-swapping/)  
[In-Memory Layout of a Program (Process) « Gabriele Tolomei](https://gabrieletolomei.wordpress.com/miscellanea/operating-systems/in-memory-layout/)  
[🚀 Demystifying memory management in modern programming languages | Technorage](https://deepu.tech/memory-management-in-programming/)  
[Stack-based memory allocation - Wikipedia](https://en.wikipedia.org/wiki/Stack-based_memory_allocation)  
[2. x86 Assembly and Call Stack - Computer Security](https://textbook.cs161.org/memory-safety/x86.html)  
[Writing a Function in Assembly: Intel x86 Att Assembly Stack Part 1](https://www.youtube.com/watch?v=5iQkR69H_1M&ab_channel=YoungSlothLearning)  
[(PDF) Extending a Light-weight Runtime System by Dynamic Instrumentation for Performance Evaluation](https://www.researchgate.net/publication/220826900_Extending_a_Light-weight_Runtime_System_by_Dynamic_Instrumentation_for_Performance_Evaluation)  
[Virtual Address Space (Memory Management) - Win32 apps | Microsoft Docs](https://docs.microsoft.com/en-us/windows/win32/memory/virtual-address-space)  
[CS 225 | Stack and Heap Memory](https://courses.engr.illinois.edu/cs225/sp2022/resources/stack-heap/)  

[Memory management - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)  
[Trash talk: the Orinoco garbage collector · V8](https://v8.dev/blog/trash-talk)  
[🚀 Visualizing memory management in V8 Engine (JavaScript, NodeJS, Deno, WebAssembly) - DEV Community 👩‍💻👨‍💻](https://dev.to/deepu105/visualizing-memory-management-in-v8-engine-javascript-nodejs-deno-webassembly-105p)  
