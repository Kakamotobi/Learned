# Performance

## Table of Contents
- [Sample System](#sample-system)
- [Understanding Performance](#understanding-performance)
  - [What is Performance?](#what-is-performance)
    - [Performance-related Problems](#performance-related-problems)
    - [Principles of Performance](#principles-of-performance)
  - [Performance Objectives](#performance-objectives)
  - [Measuring Performance](#measuring-performance)


- [Improving Performance](#improving-performance)
  - [Latency](#latency)
  - [Concurrency](#concurrency)
  - [Caching](#caching)

## Sample System
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Software%20Architecture/refImg/sample-system.png" alt="Sample System" width="100%" />
  <em>Representation of most software systems.</em>
</p>

## Understanding Performance
### What is Performance?
- **The measure of how fast or responsive a system is on (these should be fixed variables):**
  - a given **workload** (request volume, backend data).
  - a given **hardware** (kind, capacity).
- *i.e. how fast requests are satisfied with responses and how fast batch processes are completed.*
- The goal is to maintain or mitigate performance even if workload increases, and to improve performance by upgrading hardware.
#### Performance-related Problems
- **Every performance problem is the result of some queue building up somewhere (like a traffic jam).**
  - Ex: network socket queue (too many requests), DB IO queue (too many requests to DB), OS run queue (how many threads are available in the CPU), etc.
  - *i.e. system resources are congested.*
- **Why do queues build up?**
  - Inefficient Slow Processing
    - Ex: slow algorithm.
  - Serial Resource Access / Low Concurrency
    - Ex: only a single thread can access the resource at a time.
  - Limited Resource Capacity
    - Ex: limited number of CPUs to process the queue.
#### Principles of Performance
- **High Efficiency** (the deciding factor for serial request processing)
  - Efficient System Resource Utilization
    - IO - Memory, Network, Disk
    - CPU
  - Efficient Logic - goal is to be minimal.
    - Algorithms
    - DB Queries
  - Efficient Data Storage
    - Data Structures
    - DB Schema
  - Caching
- **High Concurrency** (in the context of concurrent request processing, which is simply multiple serial requests together)
  - Hardware
  - Software
    - Queuing (resulting from multiple unique requests simultaneously present)
    - Coherence
- **Adequate Capacity**
  - CPU, Memory, Disk, Network
### Performance Objectives
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Software%20Architecture/refImg/performance-objectives.png" alt="Sample System" width="100%" />
</p>

#### 1) Minimize Request-Response Latency
- **Latency** is a measure of how much time a request-response process spends within a system.
  - There are 2 types of Latency: serial request latency and parallel/concurrent request latency.
- Latency = Wait/Idle Time + Processing Time
#### 2) Maximize Throughput
- **Throughput** is a measure of how many requests a system can process in a given time.
  - i.e. rate of request processing.
- Throughput depends on latency and capacity.
  - Hence, minimizing latency and increasing capacity means throughput is maximized.
### Measuring Performance
#### Latency
- High Latency &rarr; Bad User Experience
- Hence, latency should be minimized.
- **Tail Latency**
  - An indication of queuing of requests (these requests may not have been able to get system resources due to other requests).
  - Some requests may have a rather high latency. This means that as the workload increases, the tail latency gets worse (i.e. more requests will shift towards tail latency from average latency).
  - It can be determined by computing the 99th percentile latency.
#### Throughput
- Low Throughput &rarr; Less Number of Users Served
- Hence, throughput should be at least higher than the peak number of users at a given time.
#### Errors
- Errors &rarr; Functional Incorrectness
- Performance related errors are usually due to exceeding the time. But functional errors make latency and throughput figures meaningless.
#### Resource Saturation
- How much hardware capacity is utilized.
  - Ex: 100% CPU usage or Network bottleneck indicate that the resource/capacity is completely utilized.
- Determine whether the system resources are being efficiently utilized.
### Network Latency
- The two kinds of network being referred to here are 1) the browser and the web application (i.e. the Internet), 2) intranet communications (i.e. within the system).
  - The Internet is generally more unreliable while the network between the system servers are more reliable. Therefore, the two networks require different considerations.
#### Latency between Browser and Web Application
- Any network is connected by wires and data physically travels over those wires. Therefore, there is bound to be *some* latency - called **Data Transfer Latency**.
- For a client to communicate with a server, it has to first create a [**TCP connection**](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Network-Protocols.md#tcp-3-way-handshake), which HTTP is used on top of it. This causes for a latency of one round trip (SYN ,SYN+ACK).
- For clients and servers (and between system servers if necessary) to communicate securely, a cryptographic protocol like [**TLS**](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Network-Protocols.md#tls-handshake) is used on top of TCP. This causes for a latency of two round trips (three round trips including TCP).
##### How to Minimize Latency between Browser and Web Application
#### Latency between System Servers



## Improving Performance
### Latency
- Ways to minimize latency of a system.
#### CPU
- How to maximize CPU utilization.
#### Memory
- How to minimize memory related latency.
#### Network
- How to minimze network related latency while transferring data.
#### Disk
- How to improve disk utilizaiton and minimize disk latency.
### Concurrency
- Ways to improve throughput of a system.
#### Locking
##### Pessimistic
##### Optimistic
#### Coherence
### Caching
- Ways to cache static and dynamic data.
#### Static Data
#### Dynamic Data
