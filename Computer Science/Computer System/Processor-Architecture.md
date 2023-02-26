# Processor Architecture

## Table of Contents
- [Processor Architecture](#processor-architecture)
  - [Terminologies](#terminologies)
    - [Instruction Set Architecture(ISA)](#instruction-set-architectureisa)
    - [x-bit Processor](#x-bit-processor)
    - [Complex Instruction Set Computer(CISC)](#complex-instruction-set-computercisc)
    - [Reduced Instruction Set Computer(RISC)](#reduced-instruction-set-computerrisc)
- [x86, x86-64](#x86-x86-64)
  - [x86](#x86)
  - [x86-64](#x86-64)
- [ARM](#arm)

## Processor Architecture
- Processor Architecture is concerned with how the processor handles information.
### Terminologies
#### Instruction Set Architecture(ISA)
  - A part of the abstract model of a computer that defines how the software controls the CPU.
    - i.e. it defines the commands, written in machine code, that a processor can understand.
  - Refers to predefined actions/calculations to be done by a processor.
#### x-bit Processor
  - An x-bit Processor is capable of processing data in x-bit chunks.
  - Ex: a 32-bit processor can process 32 bits of data at a time, which means the maximum addressable memory space is 2<sup>32</sup> = 4,294,967,295 (memory locations that each store 1 byte) = 4GB.
  - Ex: a 64-bit processor can process 64 bits of data at a time, which means the maximum addressable memory space is 2<sup>64</sup> = 18,446,744,073,709,551,616 (memory locations) = 16EB.
#### Complex Instruction Set Computer(CISC)
  > A complex instruction set computer is a computer architecture in which single instructions can execute several low-level operations or are capable of multi-step operations or addressing modes within single instructions. | Wikipedia

  - A rich instruction set where complex series of operations can be executed in a single cycle.
  - As powerful the instructions are, it requires more transistors (eating up space and power).
  - Simplfies programmer's job, faster computing power, expensive chip.
#### Reduced Instruction Set Computer(RISC)
  > ...a reduced instruction set computer (RISC) is a computer architecture designed to simplify the individual instructions given to the computer to accomplish tasks. | Wikipedia

  - A more compact and simple instruction set where a single instruction executes only a simple operation.
  - As more energy efficient it is, it requires more complexity for the assembly language programmer.
    - i.e. more code is needed to perform an operation.
  - Simplifies processor's job, relatively slower computing power, cheaper chip.

## x86, x86-64
### x86
> x86 (also known as 80x86 or the 8086 family) is a family of complex instruction set computer (CISC) instruction set architectures initially developed by Intel based on the Intel 8086 microprocessor and its 8088 variant. | Wikipedia

- x86 started off as a 16-bit ISA and then grew to 32-bit later.
- x86 currently refers to any 32-bit processor that is compatible with the x86 ISA.
- x86 microprocessors can run msot types of computers (Ex: laptop, desktop, server)
- x86 CPUs, as a CISC, has powerful instructions but needs more transistors.
### x86-64
> x86-64 (also known as x64, x86_64, AMD64, and Intel 64) is a 64-bit version of the x86 instruction set, first released in 1999. It introduced two new modes of operation, 64-bit mode and compatibility mode, along with a new 4-level paging mode. | Wikipedia

- In x64 systems, hardware components (sound and graphics cards, memory, storage, CPU) are independent of one another, where each component has its own chip called a _controller_.
  - Therefore, a component can be switched/expanded without affecting the rest of the system.
- Intel and AMD currently use this architecture, and it is the most used as of date.

## ARM
> ARM (stylised in lowercase as arm, formerly an acronym for Advanced RISC Machines and originally Acorn RISC Machine) is a family of reduced instruction set computer (RISC) instruction set architectures for computer processors, configured for various environments. | Wikipedia

- Arm provides both 32-bit and 64-bit architectures.
- ARM architectures choose a different approach in designing the system hardware in comparison to x86/x64.
- In ARM systems, hardware components (and controllers) are on the same physical substrate, resulting in an integrated circuit.
- The company Arm Ltd. develops and licenses (but does not manufacture) these ISA to other companies (like Apple) that design their products based on those ISA such as System on a Chip(SoC) (like Apple Silicon).

## Reference
[Complex instruction set computer | Wikipedia](https://en.wikipedia.org/wiki/Complex_instruction_set_computer)  
[x86 - Wikipedia](https://en.wikipedia.org/wiki/X86)  
[x86-84 - Wikipedia](https://en.wikipedia.org/wiki/X86-64)  
[What is x86 Architecture and its difference between x64? - Latest Open Tech From Seeed](https://en.wikipedia.org/wiki/X86)  
[What is an ARM processor?](https://www.redhat.com/en/topics/linux/what-is-arm-processor)  
[ARM vs x86: What's the difference?](https://www.redhat.com/en/topics/linux/ARM-vs-x86)  
[ARM vs x86 - Explained | Engineering Education (EngEd) Program | Section](https://www.section.io/engineering-education/arm-x86/)  
