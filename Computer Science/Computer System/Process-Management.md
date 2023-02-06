# Process Management

## Table of Contents
- [Program](#program)
- [Process](#process)
  - [What is a Process?](#what-is-a-process)
  - [Process Control Block(PCB)](#process-control-blockpcb)
  - [Process Scheduling](#process-scheduling)
    - [The 5 States of a Process](#the-5-states-of-a-process)
    - [Types of Process Schedulers](#types-of-process-schedulers)
      - [Long-Term Scheduler](#long-term-scheduler)
      - [Short-Term Scheduler](#short-term-scheduler)
      - [Mid-Term Scheduler](#mid-term-scheduler)
    - [Scheduling Disciplines](#scheduling-disciplines)
      - [Earliest Deadline First(EDF) Scheduling](#earliest-deadline-firstedf-scheduling)
      - [Priority Scheduling](#priority-scheduling)
      - [FIFO Scheduling](#fifo-scheduling)
      - [Shortest Job First(SJF) Scheduling](#shortest-job-firstsjf-scheduling)
      - [Round-Robin Scheduling](#round-robin-scheduling)
    - [Process Context Switching](#process-context-switching)

## Big Picture
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Concepts-_Program_vs._Process_vs._Thread.jpg/800px-Concepts-_Program_vs._Process_vs._Thread.jpg" alt="Program, Process, Thread" width="80%"/>
</p>

## Program
- **A program is a set of instructions for a particular application or system program stored on a disk or secondary memory.**
  - Programs usually consist of multiple subprograms.
- When a program starts, an instance of it is read into RAM and executed by the kernel.

## Process
### What is a Process?
- **A process is an executing instance of a program that is loaded onto RAM (is allocated virtual memory).**
  - Ex: opening two VSCode windows means there are two processes that are running the VSCode program.
- _Each individual process is allocated a private memory space for security reasons. Hence, they cannot communicate with each other._
- A process has one or more threads.
### Process Control Block(PCB)
- a.k.a. process descriptor
- **A PCB is a data structure that the OS uses to store all the information related to a particular process.**
  - When a process is created, the OS creates a PCB for that process in the _kernel memory space_.
- PCB is generally composed of:
  - Process ID
  - Process State
  - Register Values used by the Process
    - Program Counter
    - Stack Pointer, etc.
  - CPU Scheduling Information
  - Memory Information
    - The memory address where the process is stored in.
    - Page table information.
  - Open File Information
    - Information on files being used.
  - I/O Status Information
- How to check running processes in Terminal
  - View processes that are running: `top`, `ps -ef`, `ps aux`.
    - `ps` is short for "process status".
    - More on the `ps` command [here](https://man7.org/linux/man-pages/man1/ps.1.html).
  - Kill a process: `kill -<PID>`
### Process Scheduling
- Processes require resources (memory, CPU time, I/O devices, etc.) in order to execute. Therefore, the OS has to decide how to allocate these resources to different processes in an efficient way.
- The objective in process scheduling is to keep the CPU busy at all times and deliver quick response times for the programs being used.
- The Process Scheduler is responsible for **deciding which process should run at a certain point in time and for how long.**
  - i.e. which process's turn it is to consume resources.
#### The 5 States of a Process
<p align="center">
  <img src="https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_02_ProcessState.jpg" alt="States of a Process" width="80%"/>
</p>

- **New**
  - The process is being created.
- **Ready**
  - The process is ready to be executed.
  - i.e. waiting for its turn to be executed by the CPU in the ready queue.
- **Running**
  - The process is being executed by the CPU.
- **Waiting**
  - The process is not ready to be executed yet.
  - Ex: waiting on resources to be available.
- **Terminated**
  - The process has completed.
#### Types of Process Schedulers
- These types are based on the frequency of decisions that need to be made.
##### Long-Term Scheduler
- a.k.a. **Job Scheduler**
  - "Job" Background
    - Historically, a program (essentially a set of instructions) used to be on cards, which you would hand over to someone who takes the card and executes it using a card reader. This was an actual **_job_**(a piece of work) of a person. Hence, these cards/programs were referred to as "jobs".
  - A program is comprised of multiple subprograms.
    - Ex: the program Word is comprised of subprograms including a spell checker, printer, etc.
    - Ex: the program VSCode is comprised of subprograms including intellisense, extensions, terminal, debugger, file explorer, etc.
    - Therefore, an overall _job_ simply refers to something that the user wants the OS to do, which comprises of smaller processes.
  - **A job is a unit of work that the user requests the OS to do.**
    - It may require multiple tasks involving one or more processes.
    - For example, the user double clicks on VSCode. This is a **_job_** in itself (requesting to open VSCode). The OS receives this job and breaks it down into a series of tasks that are required.
  - It is a _high-level representation of a work_ that needs to be done, and contains information including the program to be executed, input data, resource requirements, priority level, etc.
  - cf. Process
    - A job is a essentially a request to execute a program.
    - A process is the actual execution of that program.
    - _When a job is selected from the ready queue to be executed, it becomes a process._
  - cf. Task
    - A task is a unit of execution within a job.
    - Ex: every step required in opening VScode (a job) is a task.
- **A Long Term Scheduler is responsible for maintaining the _jobs_ in the _Ready Queue_ that are ready and waiting to be executed.**
  - i.e. when the user attempts to execute a program, the job scheduler decides whether to accept it into the ready queue or delay it.
  - Thereby, it controls the degree of multi-programming (the number of processes that are in "ready" state at any point in time).
  - It makes decisions based on factors including memory requirements and priority levels.
##### Short-Term Scheduler
- a.k.a. **CPU Scheduler**
- **A Short Term Scheduler is responsible for deciding which process in the ready queue should be executed (i.e. allocated CPU time).**
  - i.e. decides which job in the ready queue is going to be dispatched to the CPU for execution.
##### Medium-Term Scheduler
- **A Medium Term Scheduler is responsible for temporarily removing processes from RAM and placing them in secondary memory (i.e. _swapping_).**
- Inactive or low priority processes may be swapped out, and swapped in later when needed.
#### Scheduling Disciplines
- These are some scheduling algorithms that schedulers can use to manage processes.
- Preemptive vs Non-Preemptive
  - **Preemptive:** a job that is currently executing can be interrupted if a shorter job exists.
  - **Non-Preemptive:** a job that is currently executing cannot be interrupted and occupies the CPU until its state reaches "waiting" or "terminated".
- Burst Time vs Arrival Time
  - **Burst Time:** the (CPU) time, in milliseconds, required for a process to complete execution.
    - i.e. time it takes from "running" state to "terminated" state.
  - **Arrival Time:** the point in time, in milliseconds, at which a process is added to the ready queue waiting to be executed.
    - i.e. time at which process state changes from "new" to "ready".
##### Earliest Deadline First(EDF) Scheduling
- **EDF Scheduling executes the job with the earliest deadline.**
  - i.e. the job that is closest to its deadline will be executed first.
##### Priority Scheduling
- **Priority Scheduling executes jobs with higher priorities first.**
- Priority can be based on memory requirements, time requirements, etc.
- When jobs have the save priority, the first that arrived is executed first.
- Pros and Cons
  - Priorities can be fixed or change dynamically.
  - Jobs of lower priorities may be continuously delayed.
##### FIFO Scheduling
- a.k.a. First-Come-First-Serve(FCFS) Scheduling
- **FIFO Scheduling is a non-preemptive algorithm executes jobs in the order that they are ready.**
  - i.e. the process that was ready first is executed first.
- FIFO Scheduling is often used for short-term scheduling.
- Pros and Cons
  - Easy and simple to implement.
  - Minimum overhead since non-preemptive.
  - Good response time.
  - High average wait time.
  - Not very efficient.
##### Shortest Job First(SJF) Scheduling
- **SJF Scheduling is a greedy algorithm that executes jobs in order of the shortest execution time (estimated) to longest execution time (estimated).**
- _Preemptive SJF_ is also known as _Shortest Remaining Time First(SRTF)_.
  - When a job with a shorter burst time arrives at the ready queue, the current process is preempted from execution for the shorter job to be executed first.
- SJF is often used for long-term scheduling.
- SJF cannot be used for short-term scheduling since it can't really predict the length of the upcoming CPU burst.
- Pros and Cons
  - High throughput since shortest jobs are executed first.
  - Lowest average waiting time among scheduling algorithms.
  - Accurate estimation of the execution time of each process is key to produce optimal results.
  - Starvation since long processes can stay in the queue for a long time.
##### Round-Robin Scheduling
- **Round-Robin Scheduling is a preemptive algorithm that executes jobs in FIFO order but grants the same amount of CPU time (time slice) for all jobs until completion (in a cyclic queue).**
  - i.e. each job gets an equal share of CPU time in turns, and the preempted job is queued again.
- Round-Robin Scheduling is widely used in traditional OS.
- Pros and Cons
  - Easy to implement.
  - Best average response time.
  - Fair allocation of CPU time.
  - Jobs are not prioritized.
  - Overhead due to context switching (need to decide on an optimal time slice).
  - Throughput could be low depending on time slice.
#### Process Context Switching
- **Context Switching is what allows us to go back and forth between executing different processes.**
- Example
  - Process A's time with the CPU ends.
  - Context is switched:
    - Process A saves the current execution status (PCB stuff).
    - Process B's context is restored and resumes executing from where it left off.
  - Repeat.
- Process switching is costly since it involves switching all the process resources (different memory address space) including memory addresses, page tables, kernel resources, processor caches, etc.

## Reference

[Operating Systems: Processes - UIC](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/3_Processes.html)  
[Differences Between Program Vs Process Vs Threads](https://javaconceptoftheday.com/differences-between-program-vs-process-vs-threads/)  
[Job vs. Task vs. Process | Baeldung on Computer Science](https://www.baeldung.com/cs/job-vs-task-vs-process)  
[Operating System Jobs and Processes](https://www.youtube.com/watch?v=yoKUagj5MxI&t=79s&ab_channel=JohnPhilipJones)  
[operating system - job, task and process, what's the difference - Stack Overflow](https://stackoverflow.com/questions/3073948/job-task-and-process-whats-the-difference)  
[Scheduling (computing) - Wikipedia](<https://en.wikipedia.org/wiki/Scheduling_(computing)>)  
[Job Scheduling | Technology Glossary Definitions | G2](https://www.g2.com/glossary/job-scheduling-definition)  
[Priority Scheduling Algorithm: Preemptive, Non-Preemptive EXAMPLE](https://www.guru99.com/priority-scheduling-program.html)  
[FCFS Scheduling Algorithm: What is, Example Program](https://www.guru99.com/fcfs-scheduling.html)  
[Shortest Job First (SJF): Preemptive, Non-Preemptive Example](https://www.guru99.com/shortest-job-first-sjf-scheduling.html)  
[Round Robin Scheduling Algorithm with Example](https://www.guru99.com/round-robin-scheduling-example.html)  
