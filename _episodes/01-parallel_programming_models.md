---
title: "Introduction to Parallel Programming"
teaching: 0
exercises: 0
questions:
- "What parallel computing is and why it is important"
- "Where "
objectives:
- "Explain differences between a serial and a parallel program"
- "Discuss advantages and disadvantades of parallel programming"
- "Explain the various criteria on which classification of parallel computers are based"
- "Discuss the Flynn’s classification based on instruction and data streams"

keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

## Why to learn parallel computing?
- Building faster serial computers became constrained due to both physical and practical reasons.

- The increase in serial performance slowed down because processor designs aproached limits of miniaturization, clock frequency, power consumption, data transmission speed.

- It is increasingly expensive to make a single processor faster. Using a larger number of moderately fast commodity processors to achieve the same (or better) performance is more practical.


### Parallel Computing Applications

Here are just a few examples how parallel computing is helping to accelerate research and solve the previously unsolvable problems.

1. Numeric Weather prediction
    - Taking current observations uses mathematical models  to forecast the future state of the weather
2. Computational Astrophysics
    -  Numerical relativity, magnetohydrodynamics, stellar and galactic magnetohydrodynamics
3. Seismic Data Processing
    - Analyse seismic data to find oil and gas bearing layers
4. Commercial world
    - Nearly every major aspect of today’s banking, from credit scoring to risk modeling to fraud detection, is GPU-accelerated.
5. Pharmaceutical design
    - Uses parallel computing for running simulations of molecular dynamics

<figure class="video_container">
  <video width="360" controls="true" loop allowfullscreen="true">
    <source src="../fig/calmodulin.mp4" type="video/mp4">
  </video>
</figure>


### Benefits of Parallel Computing:
 - Allows to solve problems in a shorter time.
 - Allows to solve larger, more detailed problems compared to serial execution.
 - Overall parallel computing is much better suited for modeling, simulating and understanding complex, real-world phenomena.

### Limitations and Disadvantages:

- Some problems can not be broken into pieces of work that can be done independently
- Not every problem can be solved in less time with multiple compute resources than with a single compute resource. Some problkems may not have enough inherent parallelism to exploit.
- May be more costly in equipment (expensive interconnect)
- Coding expertise
- Require time and effort


### Before you start your parallel computing project

- Select the most appropriate parallelization options for your problem.
- Estimate the effort and potential benefits.

We begin here to give you the knowledge and skills to make decisions on parallel computing projects up front.


### The limit to parallel computing speedup

#### Speedup of a fixed size problem: Amdahl’s Law.

This law describes the speedup of a fixed size problem as the number of processors increases:

```
Speedup (N) = 1/(S+P/N)
```
P is the parallel fraction of the code
S is the serial fraction of the code
N is the number of processors

- Speedup is limited by the serial fraction.

#### Speedup for a problem size that grows with the number of available processors: Gustafson-Barsis’s Law

SpeedUp(N) = N − S * (N − 1)

-  If the problem size grows proportionally to the number of processors, the scaling is much better.

## How does Parallel Computing Work?

 Parallelism is the future of computing. Parallel programming  allows to increase the amount of processing power in a system. How does a parallel program work?

Figures, strong and weak scaling

### Traditional serial program
```
               Instructions
Problem  ->  |||| | | |  |    |  ->  CPU  ->  Result
```

- A problem is broken into a set of instructions
- Instructions are executed sequentially on a single CPU
- Only one instruction can be executed at a time

###  Parallel program

- Parallel computing is breaking a problem into smaller chunks and executing them concurrently on a set of different CPUs and/or computers.

```
                         Instructions
            Chunk_1  ->  || |  |   |  ->  CPU_1
Problem --> Chunk_2  ->  || |  |   |  ->  CPU_2   ->  Result
            Chunk_3  ->  || |  |   |  ->  CPU_2
```

- A problem is broken into parts that can be done concurrently
- Each part is further broken down into a set of instructions
- Instructions from each part execute simultaneously on different processors
- Requires an overall execution control & coordination mechanism.


## Architecture of Parallel Computers

- Modern desktop/laptop computers come equipped with multiple central processing units (cores) that can process many sets of instructions simultaneously.

- Each core is equipped with vector unit that can process multiple data at one time. Vector units are described by the number of the bits that the vector unit can process at one time. For example, a 256 bit-wide vector unit can process four double precision floating point numbers simultaneously.

- Networks connect multiple stand-alone computers (nodes) to make larger parallel computer clusters.

For example, a typical Compute Canada cluster has thousands of computers (network nodes):
    - Each compute node is a multi-processor parallel computer
    - Each processor is equipped with AVX-2 or AVX-512 vector units
    - Multiple compute nodes are interconnected with an Infiniband network
    - Special purpose nodes (login, storage, scheduling, etc.)

> ## What fraction of the total processing power can a serial program use?
> Let’s consider an 8 core CPU with AVX-512 vector unit, commonly found in home desktops. What percentage of the theoretical processing capability of this processor can a serial program without vectorization use?
>
>>## Solution
>> 8 cores x (512 bit-wide vector unit)/(64-bit double) = 64-way parallelism. 1 serial path/64 parallel paths = .016 or 1.6%.
>>
> {: .solution}
{: .challenge}


> ## [TOP 500](https://www.top500.org) publishes current details of the fastest computer systems.
> - In the application area statistics "Not Specified" is by far the largest category - probably means multiple applications.
{: .callout}

To make a full use of compute resources the programmer needs to have a working knowledge of the parallel programming concepts and tools available for writing parallel applications. To build a basic understanding of how parallel computing works, let's start with an explanation of the components that comprise computers and their function.

### Von Neumann Architecture

Von Neumann architecture was first published by John von Neumann in 1945. It is based on the stored-program computer concept where computer memory is used to store both program instructions and data. Program instructions tell the computer to do something.


<img src="../fig/Von-Neumann-Architecture-Diagram.jpg" alt="drawing" width="480">

- Central Processing Unit:
    -  the electronic circuit responsible for executing the instructions of a computer program.
-  Memory Unit:
    - consists of randor access memory (RAM) Unlike a hard drive (secondary memory), this memory is fast and directly accessible by the CPU.
- Arithmetic Logic Unit:
    - carries out arithmetic (add, subtract etc) and logic (AND, OR, NOT etc) operations.
- Control unit:
    - reads and interprets instructions from the memory unit.
    - controls the operation of the ALU, memory and input/output devices, telling them how to respond to the program instructions.
- Registers
    - Registers are high speed storage areas in the CPU.  All data must be stored in a register before it can be processed.
- Input/Output Devices:
    - The interface to the human operator, e.g. keyboard, mouse, monitor, speakers, printer, etc.

Parallel computers still follow this basic design, just multiplied in units. The basic, fundamental architecture remains the same.


Parallel computers can be classified based on various criteria:
- number of data & instruction streams
- computer hardware structure (tightly or loosely coupled)
- degree of parallelism (the number of binary digits that can be processed within a unit time by a computer system)

Today there is no completely satisfactory classification of the different types of parallel systems.The most popular taxonomy of computer architecture is Flynn classification.

> ## Classifications of parallel computers
> 1. Flynn classification: (1966) is based on multiplicity of instruction streams and the data streams in computer systems.
> 2. Feng’s classification: (1972) is based on serial versus parallel processing (degree of parallelism).
> 3. Handler’s classification: (1977) is determined by the degree of parallelism and pipelining in various subsystem levels.
{: .callout}



### Flynn's classification of Parallel Computers

- Flynn’s classification scheme is based on the notion of a stream of information. Two types of information flow into a processor: instructions and data.
- The instruction stream is defined as the sequence of instructions performed by the processing unit.
- The data stream is defined as the data traffic exchanged between the memory and the processing unit.

In Flynn’s classification, either of the instruction or data streams can be single or multiple. Thus computer architecture can be classified into the following four distinct categories:
```
                   Instruction Stream
                    Single  Multiple
Data   |  Single       SISD    SIMD
Stream |  Multiple     MISD    MIMD
```


#### SISD
Conventional single-processor von Neumann computers are classified as SISD systems.

Examples:
Historical supercomputers such as the Control Data Corporation 6600.

#### SIMD

- The SIMD model of parallel computing consists of two parts: a front-end computer of the usual von Neumann style, and a processor array.
- The processor array is a set of identical synchronized processing elements capable of simultaneously performing the same operation on different data.
- The application program is executed by the front-end in the usual serial way, but issues commands to the processor array to carry out SIMD operations in parallel.

Figure

All modern desktop/laptop processors are classified as SIMD systems.

#### MIMD

- Multiple-instruction multiple-data streams (MIMD) parallel architectures are made of multiple processors and multiple memory modules connected together via some interconnection network. They fall into two broad categories: **shared memory** or **message passing**.
- Processors exchange information through their central shared memory in shared memory systems, and exchange information through their interconnection network in message passing systems.

##### MIMD shared memory system
- A shared memory system typically accomplishes inter-processor coordination through a global memory shared by all processors.
- Because access to shared memory is balanced, these systems are also called SMP (**symmetric multiprocessor**) systems


##### MIMD message passing system
- A message passing system (also referred to as distributed memory) typically combines the local memory and processor at each node of the interconnection network.
- There is no global memory, so it is necessary to move data from one local memory to another by means of message passing.
- This is typically done by a Send/Receive pair of commands, which must be written into the application software by a programmer.

#### MISD
- In the MISD category, the same stream of data flows through a linear array of processors executing different instruction streams.
- In practice, there is no viable MISD machine




{% include links.md %}
