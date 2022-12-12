# Introduction
- Principles and concepts of software architecture essential for large-scale systems.

## Table of Contents
- [How do you architect a new system?](#how-do-you-architect-a-new-system)
- [Software Architecture vs System/Software Design](#software-architecture-vs-systemsoftware-design)

## How do you architect a new system?
  - First, consider **functional requirements**: Use Cases (user interaction), Schema (DB), Code Design (client and server).
    - Come up with an N-tier architecture to accommodate the functional requirements and consider the languages and tools to use.
  - Next, consider **system requirements**(non-functional requirements): Performance, Scalability, Reliability, Security, Deployment, Technology Stack.
    - This portion is concerned with the architecture side of things.
    - **Performance:** how to provide fast and efficient service.
      - Ex: satisfy 90% of requests within a second (performance requirement).
    - **Scalability:** how to accommodate a greater amount of usage.
      - Ex: handle 1 million users simultaneously (scalability requirement).
    - **Reliability:** how to make systems resilient to failures.
      - Ex: recovery for failing data center.
    - **Security:** how to securely transfer/store/authenticate/authorize data against external/internal threats.
    - **Deployment:** how to deploy this high-scale, highly reliable systems.
      - There are lots of components and instances of each component.
      - Requires a lot of automation and coordination with operations team.
    - **Technology Stack:** how to decide which platform/product to choose on which each component should run on.
      - The stack should be chosen based on the requirements of the system.

## Software Architecture vs System/Software Design
- **Software Architecture** focuses on the high-level infrastructure of the entire system.
  - Matching business and technical requirements (software and hardware) of the system.
- **System/Software Design** focuses on the code level (individual modules/components of the system) design.
  - Building a design plan for the elements that make up a system.
  - Ex: responsibilities/functions of individual modules/classes, what design pattern it should use, etc.

## Reference
[What’s the difference between software architecture and design? | by Concise Software | Medium](https://stackoverflow.com/questions/704855/software-design-vs-software-architecture)  
