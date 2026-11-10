Week 1 covers what an operating system is, how a computer boots, how data is stored, the hardware inside a computer, the Von Neumann architecture, and how transistors work.

*Sources: the SYS-140 "Introduction" slides, the "Computer Architecture" slides, and my "How does a transistor work?" notes.*

## Contents

1. [Operating System Fundamentals](#1-operating-system-fundamentals)
   - [What Is an Operating System?](#what-is-an-operating-system)
   - [Architecture Overview](#architecture-overview)
   - [The Kernel](#the-kernel)
   - [Computer-System Operation](#computer-system-operation)
   - [The Boot Process](#the-boot-process)
   - [Data Storage](#data-storage)
   - [Computer-System Architecture](#computer-system-architecture)
   - [Operating-System Operations](#operating-system-operations)
   - [Operating-System Services](#operating-system-services)
2. [Hardware & OS Basics](#2-hardware--os-basics)
   - [Main Hardware Components](#main-hardware-components)
   - [Central Processing Unit (CPU)](#central-processing-unit-cpu)
   - [Buses](#buses)
   - [Clocks](#clocks)
   - [Memory Hierarchy](#memory-hierarchy)
   - [Input & Output Devices](#input--output-devices)
   - [Motherboard](#motherboard)
3. [Computer Architecture: The Von Neumann Model](#3-computer-architecture-the-von-neumann-model)
   - [Von Neumann Architecture](#von-neumann-architecture)
   - [Memory and Cache](#memory-and-cache)
   - [Input/Output and Mass Storage](#inputoutput-and-mass-storage)
   - [The Arithmetic/Logic Unit (ALU)](#the-arithmeticlogic-unit-alu)
   - [The Control Unit](#the-control-unit)
   - [Putting the Pieces Together](#putting-the-pieces-together)
   - [Non-Von Neumann Architectures](#non-von-neumann-architectures)
   - [Summary](#summary)
4. [How Does a Transistor Work?](#4-how-does-a-transistor-work)
5. [References & Image Credits](#5-references--image-credits)

---

# 1. Operating System Fundamentals

**Topics:** the operating system concept, computer-system operation, computer-system architecture and operations, and computing environments.

## What Is an Operating System?

- An **operating system (OS)** is the program that manages the computer hardware.
- It provides a basis for application programs and acts as an **intermediary between the computer user and the computer hardware**.

## Architecture Overview

<img src="images/week-1/intro-s04-1.png" alt="Abstract view of the components of a computer system: users, system and application programs, operating system, computer hardware" width="650">

A computer system can be divided roughly into **four components**:

| Component | Role |
|---|---|
| **Hardware** | The CPU, memory, and input/output (I/O) devices. Provides the basic computing resources. |
| **Application programs** | Word processors, spreadsheets, compilers, web browsers. Define how those resources are used to solve users' problems. |
| **Operating system** | Controls the hardware and coordinates its use among the various application programs and users. |
| **User** | The person (or system) using the computer. |

> **Note:** This is the most widely used structure, but the OS itself might be running inside a **hypervisor**.

### Two ways to view an OS

**User view**
- On a personal computer, the OS is designed mostly for **ease of use**, with some attention to performance and none to resource utilization (how hardware and software resources are shared).
- On a shared system such as a mainframe, the OS is designed to **maximize resource utilization**, so that CPU time, memory, and I/O are used efficiently and no single user takes more than their fair share.
- On workstations connected to networks and servers, the OS **compromises** between individual usability and resource utilization.

**System view**
- The OS is a **resource allocator**. It manages CPU time, memory space, file-storage space, I/O devices, and other resources.
- The OS is a **control program**. It manages the execution of user programs to prevent errors and improper use of the computer, and is especially concerned with operating and controlling I/O devices.

## The Kernel

- The **kernel** is the one program running at all times on the computer.

<img src="images/week-1/intro-s05-1.png" alt="Windows architecture: applications, Windows API and user-mode drivers in user mode; kernel, drivers and Hardware Abstraction Layer in kernel mode; hardware at the bottom" width="500">

> **Hardware Abstraction Layer (HAL):** "A layer of code that isolates the kernel, the device drivers, and the rest of the Windows executive from platform-specific hardware differences (such as differences between motherboards)."
> Source: [The Windows Club: What is a kernel in OS?](https://www.thewindowsclub.com/what-is-a-kernel-in-os-what-are-the-types-of-kernel)

## Computer-System Operation

<img src="images/week-1/intro-s06-1.png" alt="A modern computer system: CPU, disk controller, USB controller and graphics adapter connected to memory over a shared bus" width="650">

A modern computer has one or more CPUs and device controllers (disk, USB, graphics) that connect through a common bus to shared memory.

## The Boot Process

### Power-On Self Test (POST)

<img src="images/week-1/intro-s07-1.jpg" alt="Power-On Self-Test checks I/O devices, RAM, disk drives, and PC hardware" width="650">

- Before it loads Windows or Linux, the **BIOS** runs a **Power-On Self Test (POST)** to make sure the hardware is working properly.
- After POST is complete, the PC continues starting up.
- The BIOS then finds the **Master Boot Record (MBR)** to complete the boot.
- POST is handled by the system BIOS. When POST finishes successfully, **bootstrapping** begins, which starts the initialization of the OS.

### Boot sequence

1. The boot code is loaded from the hard drive.
2. **Bootstrapping:** the boot code starts the operating system.
3. The operating system loads device drivers and starts services.
4. The operating system finishes loading.

<img src="images/week-1/intro-s09-1.png" alt="Summary of the boot process: Power on, BIOS, POST, BIOS boot sequence, MBR" width="550">

The **BIOS boot order** can be changed by the user. It defaults to the hard drive, and may also include CD-ROM, USB, or network. The BIOS goes down the list in order until it finds an OS.

🔗 Interactive visual: [Windows Boot Process](https://research.cyfidant.com/sys140-visualizations/windowsBootVisual.html)

**Further reading:** [What is a boot sequence? (Lifewire)](https://www.lifewire.com/what-is-a-boot-sequence-2625814) · [How does UEFI boot work? (Stack Overflow)](https://stackoverflow.com/questions/58002542/how-does-uefi-boot-mode-boot-flows) · [BIOS vs UEFI (Knoldus)](https://blog.knoldus.com/uefi-v-s-bios/) · [The Linux boot process (Make Tech Easier)](https://www.maketecheasier.com/linux-boot-process/)

### BIOS and firmware

- **BIOS (Basic Input/Output System)** is the motherboard's firmware. It tells the computer which drive to boot from and how much RAM it has, and controls other vital settings such as CPU frequency.
- You can enter the BIOS menu to change things like the **boot order**.

<img src="images/week-1/intro-s10-1.jpg" alt="A BIOS chip on a motherboard" width="400"> <img src="images/week-1/intro-s11-1.png" alt="PhoenixBIOS Setup Utility showing the Boot menu" width="420">

### UEFI

- The setup program on modern computers is **UEFI (Unified Extensible Firmware Interface)**. It supports larger drives and has richer graphical menus than the BIOS.

**Further reading:** [BIOS vs firmware (Super User)](https://superuser.com/questions/879908/what-is-the-difference-between-bios-and-firmware) · [BIOS firmware definition (Tom's Hardware)](https://www.tomshardware.com/news/bios-firmware-definition,37646.html)

## Data Storage

### Storage definitions and notation: bit, byte, and word

<img src="images/week-1/intro-s14-1.png" alt="64 bits broken into nybbles, bytes, words, long words, and a very long word" width="600">

| Unit | Size |
|---|---|
| Bit | A single 0 or 1 |
| Nybble | 4 bits |
| Byte | 8 bits |
| Word | 16 bits |
| Long word | 32 bits |
| Very long word | 64 bits |

Reference: [TCP/IP Guide: Bits, Bytes, Nibbles](http://www.tcpipguide.com/free/t_BinaryInformationandRepresentationBitsBytesNibbles-2.htm)

### Storage structure

<img src="images/week-1/intro-s15-1.png" alt="Storage-device hierarchy from registers at the top down through cache, main memory, electronic disk, magnetic disk, optical disk, and magnetic tapes" width="450">

The hierarchy trades off **storage size**, **cost**, **speed**, and **volatility**. Higher levels are faster, smaller, and more expensive. Lower levels are slower, larger, and cheaper.

- A **cache** is a fast, temporary data-storage layer that keeps copies of frequently used files or instructions. Storing them closer to the processor or app avoids slow trips to main storage, so websites, programs, and devices run much faster.
- *This relates to evidence acquisition: volatile data disappears when power is lost.*

### Caching

<img src="images/week-1/intro-s16-1.png" alt="Migration of integer A from magnetic disk to main memory to cache to hardware register" width="650">

Data moves up the hierarchy as it is used: **disk → main memory → cache → register**.

### Performance of storage levels

<img src="images/week-1/intro-s17-1.jpg" alt="Table comparing registers, cache, main memory and disk storage by size, technology, access time, bandwidth, manager and backing store" width="700">

| Level | 1. Registers | 2. Cache | 3. Main memory | 4. Disk storage |
|---|---|---|---|---|
| Typical size | < 1 KB | < 16 MB | < 64 GB | > 100 GB |
| Technology | Custom memory with multiple ports, CMOS | On-chip or off-chip CMOS SRAM | CMOS DRAM | Magnetic disk |
| Access time (ns) | 0.25 – 0.5 | 0.5 – 25 | 80 – 250 | 5,000,000 |
| Bandwidth (MB/sec) | 20,000 – 100,000 | 5,000 – 10,000 | 1,000 – 5,000 | 20 – 150 |
| Managed by | Compiler | Hardware | Operating system | Operating system |
| Backed by | Cache | Main memory | Disk | CD or tape |

- **Compiler:** a program that translates human-readable source code written in a high-level language (like C++ or Java) into low-level machine code that the processor can execute directly.
- **CMOS (Complementary Metal-Oxide-Semiconductor):** a major semiconductor technology used to build microchips, integrated circuits, and image sensors in nearly all modern electronics.

## Computer-System Architecture

### Single-processor systems

<img src="images/week-1/intro-s18-1.png" alt="How a modern computer system works: CPU with cache, memory, and devices exchanging instructions, data, interrupts, and DMA" width="550">

### Multiprocessor systems (multiprocessing)

<img src="images/week-1/intro-s19-1.png" alt="Symmetric multiprocessing: three CPUs, each with its own registers and cache, sharing memory" width="550">

<img src="images/week-1/intro-s20-1.png" alt="Dual-core design with two CPU cores on the same chip sharing memory" width="450">

🔗 Interactive visual: [CPU Processing](https://research.cyfidant.com/sys140-visualizations/cpuProcessingVisual.html)

### Multiprogramming

<img src="images/week-1/intro-s21-1.png" alt="Memory layout for a multiprogramming system: the operating system followed by jobs 1 through 4" width="350">

> Virtually all modern systems use **both** multiprocessing and multitasking. They complement each other:
> - **Multiprocessing** provides the *hardware* capability (multiple CPUs or cores).
> - **Multitasking** provides the *software* management (the OS scheduling multiple tasks).

## Operating-System Operations

### Dual-mode operation

- **Kernel mode** (also called supervisor mode, system mode, or privileged mode): mode bit = 0
- **User mode**: mode bit = 1

<img src="images/week-1/intro-s22-1.png" alt="Transition from user mode to kernel mode: a user process calls a system call, traps to kernel mode, executes the call, and returns" width="700">

When a user process makes a **system call**, the CPU *traps* into kernel mode, runs the system call, and then *returns* to user mode.

## Operating-System Services

<img src="images/week-1/intro-s23-1.png" alt="Operating system services: user interfaces, system calls, and services such as program execution, I/O, file systems, communication, resource allocation, accounting, error detection, and protection" width="650">

- **User interfaces:** GUI, batch, command line
- **System calls** connect programs to OS services.
- **Services:** program execution, I/O operations, file systems, communication, resource allocation, accounting, error detection, protection and security

---

# 2. Hardware & OS Basics

## Main Hardware Components

**Hardware** is the physical components of a computer. The main categories are:

- Central Processing Unit (CPU)
- Main memory
- Secondary storage devices
- Input/output devices
- Motherboard

## Central Processing Unit (CPU)

The **CPU** is the hardware component that runs programs. It includes:

- **Control unit**
  - Retrieves and decodes program instructions
  - Coordinates computer operations
- **Datapath**
  - **Arithmetic Logic Unit (ALU)**
    - Arithmetic operations: `+`, `-`, etc.
    - Logical operations: `<`, `=`, etc.
  - **Registers**
    - Memory slots inside the processor that store data for fast lookup
    - General-purpose vs. special-purpose (example: the **Program Counter (PC)** register)

🔗 Interactive visual: [CPU Pins](https://research.cyfidant.com/sys140-visualizations/cpuPinsVisual.html)

### The CPU task cycle

1. **Fetch**
2. **Decode**
3. **Execute**

## Buses

<img src="images/week-1/intro-s28-1.jpg" alt="A school bus, as an analogy for a computer bus" width="300">

- Buses allow computer components to communicate.
- Only one system can use the bus at a time.
- Synchronous vs. asynchronous
- Point-to-point vs. multipoint
- Buses require protocols, much like network communications.

## Clocks

<img src="images/week-1/intro-s29-1.png" alt="An alarm clock, as an analogy for the system clock" width="250">

- Clocks synchronize the actions of a system.
  - Most systems have a **master clock** that governs everything.
  - Many hardware actions happen only on clock ticks.
  - Shorter clock cycles mean faster performance, but cycles must still allow time for actions to happen and propagate.
  - **Overclocking** components can be risky.

## Memory Hierarchy

<img src="images/week-1/intro-s31-1.jpg" alt="Where each memory level lives: registers and L1/L2 cache inside the CPU, L3 cache on the motherboard or CPU housing, RAM on the motherboard, disk storage in the hard drive" width="650">

<img src="images/week-1/intro-s32-1.png" alt="Computer memory hierarchy pyramid from processor registers at the top to tape backup at the bottom, showing size, capacity, speed, cost and volatility" width="550">

🔗 Interactive visual: [Disk to CPU](https://research.cyfidant.com/sys140-visualizations/disktoCPUVisual.html)

### Registers

- High-speed storage space within the processor
- Temporarily hold data as instructions are decoded and executed
- General-purpose vs. special registers (for example, the Program Counter)

<img src="images/week-1/intro-s33-1.png" alt="x86 general-purpose registers EAX, EBX, ECX, EDX, ESI, EDI, ESP and EBP, with 16-bit and 8-bit sub-registers" width="450">

🔗 Interactive visual: [ALU & Registers](https://research.cyfidant.com/sys140-visualizations/aluRegistersVisual.html)

### Caches

- Memory that stores frequently accessed data
- Broken into several levels:
  - **L1:** part of the processor
  - **L2:** near (but not on) the processor
  - **L3:** farther away than L2, possibly on the motherboard

<img src="images/week-1/intro-s34-1.png" alt="Pentium II processor cartridge showing the CPU and the separate second-level cache" width="400">

### Main memory (RAM)

<img src="images/week-1/intro-s35-1.jpg" alt="Two RAM memory sticks" width="300">

- Holds both program instructions and data
- **Volatile:** erased when the program ends or the computer is turned off
- Also called **Random Access Memory (RAM)**, because the CPU can access data and instructions from any memory location

### Secondary storage

<img src="images/week-1/intro-s36-1.jpg" alt="A 1 TB internal hard drive" width="250">

- Holds data when a program isn't running or the computer is off
- Forms of secondary storage:
  - **Disk drive:** mounted inside the computer or connected to an external port. Data is stored magnetically.
  - **Optical:** CD or DVD drive
  - **Flash:** USB flash drive

## Input & Output Devices

**Input devices** send information to the computer from the outside. Many different devices can provide input.

<img src="images/week-1/intro-s37-1.jpg" alt="Keyboard" height="90"> <img src="images/week-1/intro-s37-2.jpg" alt="Mouse" height="90"> <img src="images/week-1/intro-s37-3.jpg" alt="Game controller" height="90"> <img src="images/week-1/intro-s37-4.jpg" alt="CD" height="90"> <img src="images/week-1/intro-s37-5.jpg" alt="USB flash drive" height="90"> <img src="images/week-1/intro-s37-6.jpg" alt="Microphone" height="90"> <img src="images/week-1/intro-s37-7.jpg" alt="Scanner" height="90">

*Keyboard · mouse · game controller · CD · USB stick · microphone · scanner*

**Output devices** send information from the computer to the outside. Many different devices can be used for output.

<img src="images/week-1/intro-s38-1.jpg" alt="Printer" height="110"> <img src="images/week-1/intro-s38-2.jpg" alt="Monitor" height="110"> <img src="images/week-1/intro-s38-3.jpg" alt="Speakers" height="110">

*Printer · monitor · speakers*

## Motherboard

The **motherboard** is the central board that connects all the other components.

<img src="images/week-1/intro-s39-1.jpg" alt="Labeled motherboard showing the CPU socket, RAM slots, PCIe slots, SATA ports and I/O ports" width="650">

**Main points of interest:**
- Processor (CPU socket)
- RAM slots
- PCIe slots
- I/O ports: network, sound, USB, hard drive, PS/2

---

# 3. Computer Architecture: The Von Neumann Model

## Von Neumann Architecture

The **Von Neumann architecture** is the foundation for nearly all modern computers. Its characteristics:

- Four major subsystems:
  - **Memory**
  - **Input/output**
  - **Arithmetic/logic unit (ALU)**
  - **Control unit**

  The ALU and control unit together make up the **Central Processing Unit (CPU)**.
- **Stored program concept:** programs are stored in memory alongside data.
- **Sequential execution** of instructions

<img src="images/week-1/arch-s02-1.png" alt="Components of the Von Neumann architecture: memory, control unit and ALU (the CPU), and input-output, all connected by a bus" width="600">

## Memory and Cache

- **Memory** is the functional unit where data is stored and retrieved.
- **Random access memory (RAM)**
  - Organized into **cells**, each with a unique **address**
  - Accessing any cell takes the same amount of time.
  - Cell values can be read and changed.
- Cell size (memory width) is typically **8 bits**.
- Maximum memory size (address space) is **2<sup>N</sup>**, where *N* is the length of the address.

<img src="images/week-1/arch-s04-1.png" alt="Structure of random access memory: addresses 0 to 2^N minus 1, each cell W bits wide, with the MAR and MDR" width="550">

| N (address bits) | Maximum memory size (2<sup>N</sup>) |
|---|---|
| 16 | 65,536 |
| 20 | 1,048,576 |
| 22 | 4,194,304 |
| 24 | 16,777,216 |
| 32 | 4,294,967,296 |
| 40 | 1,099,511,627,776 |
| 50 | 1,125,899,906,842,624 |

### Fetch and store

- **Fetch:** retrieve from memory (*nondestructive* fetch)
- **Store:** write to memory (*destructive* store)
- **Memory access time:** the time required to fetch or store. Modern RAM takes **5–10 nanoseconds**.
- **MAR (Memory Address Register)** holds the memory address to access.
- **MDR (Memory Data Register)** receives data from a fetch and holds data to be stored.

### Memory system circuits

- Memory uses two circuits: a **decoder** and a **fetch/store controller**.
- The **decoder** converts the address in the MAR into a signal to a specific memory cell.
  - Memory can be organized in one dimension or two dimensions.
- The **fetch/store controller** is the "traffic cop" for the MDR.
  - It takes in a signal that says whether to fetch or store.
  - It routes data to and from the memory cells and the MDR.

<img src="images/week-1/arch-s08-1.png" alt="Organization of memory and the decoding logic: a 4-to-16 decoder selects one of 16 memory addresses" width="500">

<img src="images/week-1/arch-s09-1.png" alt="Two-dimensional memory organization: a 4 by 4 grid selected by row and column lines" width="480">

<img src="images/week-1/arch-s10-1.png" alt="Overall RAM organization: MAR split into row and column decoders, memory grid, MDR and fetch/store controller" width="480">

### Cache memory

- RAM speeds have increased more slowly than CPU speeds.
- **Cache memory** is fast but expensive.
- **Principle of locality**
  - Values close to recently accessed memory are more likely to be accessed next.
  - So the system loads neighboring values into cache and keeps recent values there.
- **Cache hit rate:** the percentage of times a value is found in the cache

## Input/Output and Mass Storage

- **Input/output (I/O)** connects the processor to the outside world:
  - Humans: keyboard, monitor, etc.
  - Data storage: hard drive, DVD, flash drive
  - Other computers: network
- **RAM** is **volatile** memory: its contents are gone without power.
- **Mass storage systems** are **nonvolatile** memory:
  - **Direct access storage devices (DASDs)**
  - **Sequential access storage devices (SASDs)**

### DASDs

- Hard drives, CDs, and DVDs contain **disks**.
  - **Tracks:** concentric rings around the disk surface
  - **Sectors:** fixed-size segments of tracks, the unit of retrieval
  - The time to retrieve data depends on:
    - **Seek time**
    - **Latency**
    - **Transfer time**
- Non-disk DASDs include flash memory and optical media.

<img src="images/week-1/arch-s14-1.png" alt="Overall organization of a typical rotating disk: tracks, a read/write head, and rotation" width="500">

### I/O controllers

- DASDs and SASDs are **orders of magnitude slower** than RAM (microseconds or milliseconds).
- An **I/O controller** manages data transfer with slow I/O devices, which frees the processor to do other work.
- The controller sends an **interrupt signal** to the processor when the I/O task is done.

<img src="images/week-1/arch-s16-1.png" alt="Organization of an I/O controller: processor, memory, and an I/O controller with buffer and control logic connected to an I/O device, sending an interrupt when done" width="600">

## The Arithmetic/Logic Unit (ALU)

- The ALU is part of the processor.
- It contains circuits for **arithmetic**: addition, subtraction, multiplication, and division.
- It contains circuits for **comparison and logic**: equality, AND, OR, NOT.
- It contains **registers**: super-fast, dedicated memory connected to the circuits.
- The **data path** is how information flows in the ALU:
  - From the registers to the circuits
  - From the circuits back to the registers

<img src="images/week-1/arch-s18-1.png" alt="Three-register ALU organization: registers A and B feed the ALU, which writes to register C" width="380"> <img src="images/week-1/arch-s19-1.png" alt="Multiregister ALU organization: registers R0 to R15 feeding the ALU through left and right buses" width="220">

### Choosing the operation

- How does the ALU choose which operation to perform?
  - **Option 1:** a decoder signals one circuit to run.
  - **Option 2:** all circuits run, and a **multiplexor** selects one output.
- In practice, **option 2** is usually chosen.
- Information flow:
  1. Data comes into the ALU's registers from outside.
  2. A signal tells the multiplexor which operation to select.
  3. The result goes back to a register, and then outside the ALU.

<img src="images/week-1/arch-s21-1.png" alt="Using a multiplexor circuit to select the proper ALU result from the a+b, a-b, a=b, and a·b circuits" width="600">

<img src="images/week-1/arch-s22-1.png" alt="Overall ALU organization: registers on buses feeding add, subtract, compare and AND circuits into a multiplexor" width="350">

## The Control Unit

- **Stored program characteristic:** programs are encoded in binary and stored in the computer's memory.
- The control unit **fetches** instructions from memory, **decodes** them, and **executes** them.
- Instructions are encoded as:
  - An **operation code (op code)**, which says which operation to perform
  - **Addresses**, which say which memory addresses or registers to operate on

<img src="images/week-1/arch-s24-1.png" alt="Typical machine language instruction format: operation code, address field 1, address field 2" width="550">

### Machine language and instruction sets

- **Machine language**
  - Binary strings that encode instructions
  - Instructions can be carried out directly by hardware.
  - Sequences of instructions encode algorithms.
- **Instruction set**
  - The instructions implemented by a particular chip
  - Each kind of processor speaks a different language.

### RISC vs. CISC

| RISC (Reduced Instruction Set Computer) | CISC (Complex Instruction Set Computer) |
|---|---|
| Small instruction set | Large instruction set |
| Each instruction is highly optimized | A single instruction can do a lot of work |
| Hardware is easy to design | Hardware is complex to design |

Modern hardware includes both RISC and CISC designs.

### Kinds of instructions

- **Data transfer:** for example, move data from memory to a register
- **Arithmetic:** for example, ADD, but also AND
- **Comparison:** compare two values
- **Branch:** jump to a non-sequential instruction
  - Branching makes conditionals and loops possible.
  - Example: `JUMPLT a` means "if the previous comparison of A and B found A < B, jump to the instruction at address *a*."

<img src="images/week-1/arch-s28-1.png" alt="Examples of simple machine language instruction sequences: setting a to b plus c, and an if-else using COMPARE and JUMPGT" width="450">

### Inside the control unit

- **Program counter (PC)** register: holds the address of the *next* instruction
- **Instruction register (IR)**: holds the encoding of the *current* instruction
- **Instruction decoder circuit**
  - Decodes the op code and signals helper circuits, one per instruction
    - The helpers send addresses to the right circuits.
    - The helpers signal the ALU, the I/O controller, and memory.

<img src="images/week-1/arch-s30-1.png" alt="Organization of the control unit registers and circuits: PC with +1 incrementer, IR with op code and address fields, and the instruction decoder" width="600">

<img src="images/week-1/arch-s31-1.png" alt="The instruction decoder: a 3-bit op code selects ADD, LOAD, JUMP, or HALT" width="420">

## Putting the Pieces Together

### The Von Neumann (fetch-decode-execute) cycle

- Combining the previous pieces gives the **Von Neumann machine**.
- The **fetch-decode-execute cycle** (also called the Von Neumann cycle) repeats until a HALT instruction or an error.
  - **Fetch phase:** get the next instruction from memory
  - **Decode phase:** the instruction decoder gets the op code
  - **Execute phase:** different for each instruction

<img src="images/week-1/arch-s33-1.png" alt="The organization of a Von Neumann computer: the memory unit, ALU, I/O, and control unit connected by a bus" width="650">

### Notation for the computer's behavior

| Notation | Meaning |
|---|---|
| `CON(A)` | Contents of memory cell A |
| `A -> B` | Send the value in register A to register B (special registers: PC, MAR, MDR, IR, ALU, R, GT, EQ, LT, +1) |
| `FETCH` | Initiate a memory fetch operation |
| `STORE` | Initiate a memory store operation |
| `ADD` | Tell the ALU to select the output of the adder circuit |
| `SUBTRACT` | Tell the ALU to select the output of the subtract circuit |

### Fetch and decode phases

```
Fetch phase
  PC -> MAR        Send address in PC to MAR
  FETCH            Initiate fetch, data to MDR
  MDR -> IR        Move instruction in MDR to IR
  PC + 1 -> PC     Add one to PC

Decode phase
  IRop -> instruction decoder
```

### Execute phase examples

```
LOAD X     meaning CON(X) -> R
  IRaddr -> MAR    Send address X to MAR
  FETCH            Initiate fetch, data to MDR
  MDR -> R         Move data in MDR to R

STORE X    meaning R -> CON(X)
  IRaddr -> MAR    Send address X to MAR
  R -> MDR         Send data in R to MDR
  STORE            Initiate store of MDR to X

ADD X      meaning R + CON(X) -> R
  IRaddr -> MAR    Send address X to MAR
  FETCH            Initiate fetch, data to MDR
  MDR -> ALU       Send data in MDR to ALU
  R -> ALU         Send data in R to ALU
  ADD              Select ADD circuit as result
  ALU -> R         Copy selected result to R

JUMP X     meaning get next instruction from X
  IRaddr -> PC     Send address X to PC

COMPARE X  meaning:
           if CON(X) > R then GT = 1 else 0
           if CON(X) = R then EQ = 1 else 0
           if CON(X) < R then LT = 1 else 0
  IRaddr -> MAR    Send address X to MAR
  FETCH            Initiate fetch, data to MDR
  MDR -> ALU       Send data in MDR to ALU
  R -> ALU         Send data in R to ALU
  SUBTRACT         Evaluate CON(X) - R
                   Sets EQ, GT, and LT

JUMPGT X   meaning: if GT = 1 then jump to X, else continue to next instruction
  IF GT = 1 THEN IRaddr -> PC
```

## Non-Von Neumann Architectures

- The problems we need to solve keep getting larger.
- Computer chip speeds no longer increase exponentially.
- Shrinking components puts gates closer together, which makes them faster, but:
  - Signals through wire are limited by the speed of light.
  - Gates can't be put much closer together.
  - Heat production increases too fast.
- **Von Neumann bottleneck:** the inability of sequential machines to handle larger problems

<img src="images/week-1/arch-s41-1.png" alt="Graph of processor speeds from 1945 to the present: actual growth falls below the exponential growth rate" width="420">

### Parallel processing

- **Non-Von Neumann architectures** are other ways to organize computers.
  - Most are experimental or theoretical, **except parallel processing**.
- **Parallel processing:** many processing units operating at the same time
  - Supercomputers (in the past)
  - Desktop multi-core machines (in the present)
  - "The cloud" (in the future)

### MIMD parallel processing

- **MIMD:** Multiple Instruction streams / Multiple Data streams (cluster computing)
- Multiple, independent processors
- Each ALU operates on its own data.
- Each processor can operate independently:
  - On its own data
  - On its own program
  - At its own rate

<img src="images/week-1/arch-s44-1.png" alt="Model of MIMD parallel processing: processors with their own local memory connected by an interconnection network" width="550">

**Varieties of MIMD systems:**
- **Special-purpose systems:** newer supercomputers
- **Cluster computing:** standard machines communicating over a LAN or WAN
- **Grid computing:** machines of varying power, spread over large distances or the Internet (example: the SETI project)
- An active research area is **parallel algorithms**, which are needed to take advantage of all this processing power.

## Summary

- We must use **abstraction** to manage system complexity.
- The **Von Neumann architecture** is the standard for modern computing.
- Von Neumann machines have **memory, I/O, an ALU, and a control unit**. Programs are stored in memory, and execution is sequential unless the program says otherwise.
- Memory is organized into **addressable cells**. Data is fetched and stored using the **MAR** and **MDR**, along with a decoder and a fetch/store controller.
- Mass data storage is **nonvolatile**. Disks store and fetch **sectors** of data stored in **tracks**.
- I/O is slow and needs a dedicated **controller** to free the CPU.
- The **ALU** performs computations, moving data to and from dedicated registers.
- The **control unit** fetches, decodes, and executes instructions, which are written in **machine language**.
- **Parallel processing** architectures can perform multiple instructions at the same time.

---

# 4. How Does a Transistor Work?

When a small electrical current is applied to the transistor's **base**, it allows a larger current to flow between the **collector** and the **emitter**. It works like a valve: a little pressure on the base controls a much bigger flow of electricity.

- If there is **no current** at the base, the transistor is **off**. No current flows between the collector and emitter.
- If there is **current** at the base, the transistor turns **on** and current flows through.

> **Clarification:** My original notes said the transistor "acts like a closed switch" when there's no base current. In switch terms, it's actually like an **open** switch (the circuit is broken, so no current flows). A **closed** switch is when current *does* flow.

Because it can control electrical current, a transistor can work as a **switch** (turning things on and off) or as an **amplifier** (making signals stronger).

| Role | How it works | Where it's used |
|---|---|---|
| **Switch** | Transistors can turn on and off very quickly, representing the binary states **0 and 1**. A small voltage at the base lets a larger current flow between the collector and emitter, so the transistor is "on." When that voltage is removed, the current stops and the transistor is "off." | The foundation of digital computing |
| **Amplifier** | A small input signal applied to the base controls a larger output signal between the collector and emitter, which amplifies it. | Radios, televisions, audio systems |

In digital electronics, large numbers of transistors are used to build **logic gates**, which are the foundation of computer processors. By switching on and off very quickly, transistors process the binary code that computers use to operate.

---

# 5. References & Image Credits

**References**
- *Operating System Concepts*, 8th Edition. Abraham Silberschatz, Peter Baer Galvin, and Greg Gagne.
- *Windows 10 Troubleshooting: Learn to Troubleshoot and Repair Windows 10 Problems Like the Pros Do.* Mike Halsey. Apress. ([O'Reilly](https://learning.oreilly.com/library/view/windows-10-troubleshooting/9781484274712/html/336897_2_En_6_Chapter.xhtml))
- Computer architecture figures are from the course's "Computer Architecture" slides.

<details>
<summary><b>Image credits (from the course slides)</b></summary>

- Computer memory hierarchy pyramid: [Wikimedia Commons](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0c/ComputerMemoryHierarchy.svg/800px-ComputerMemoryHierarchy.svg.png)
- School bus: [Wikimedia Commons](https://upload.wikimedia.org/wikipedia/commons/4/4d/ICCE_First_Student_Wallkill_School_Bus.jpg)
- Alarm clock: [Walmart](https://i5.walmartimages.com/asr/d3d8b574-a52e-40d0-9983-cad106ef899b_1.745d8e1c2faaee25f70cf4ba47603d59.jpeg)
- Keyboard: [Amazon](https://images-na.ssl-images-amazon.com/images/I/61OeLOPEz9L._SL1500_.jpg)
- Mouse: [Amazon](https://images-na.ssl-images-amazon.com/images/I/71uotBbbjNL._SL1500_.jpg)
- Pro controller: [Amazon](https://images-na.ssl-images-amazon.com/images/I/61mCWiQxxHL._SL1000_.jpg)
- AOL CD: [Now I Know](http://nowiknow.com/wp-content/uploads/2012/12/120866345IVflSI_ph.jpeg)
- USB stick: [Amazon](https://images-na.ssl-images-amazon.com/images/I/619QQcGJOLL._SL1500_.jpg)
- Microphone: [Amazon](https://images-na.ssl-images-amazon.com/images/I/51KYOFKC80L._SL1122_.jpg)
- Scanner: [eHow](https://img-aws.ehowcdn.com/340x221p/photos.demandstudios.com/getty/article/250/152/AA053789.jpg)
- Printer: [Staples](https://www.staples.com/HP-M553n-Color-LaserJet-Enterprise-Laser-Printer/product_1587729)
- Monitor: [Best Buy](https://pisces.bbystatic.com/image2/BestBuy_US/images/products/5734/5734900_sd.jpg)
- Speakers: [Amazon](https://images-na.ssl-images-amazon.com/images/I/91cR9woGc6L._SL1500_.jpg)
- Memory sticks: [Rick's Daily Tips](https://www.ricksdailytips.com/wp-content/uploads/2016/03/sodimm-ram-sticks-300x185.jpg)
- Register diagram: [Yale CS421](http://flint.cs.yale.edu/cs421/papers/x86-asm/x86-registers.png)
- Pentium II caches: [Stack Exchange](https://i.stack.imgur.com/r2Wic.png)
- Memory hierarchy diagram (from reading): [O'Reilly](https://learning.oreilly.com/library/view/complete-a-guide/9780135291542/graphics/03fig07.jpg)
- Hard drive: [Best Buy](https://pisces.bbystatic.com/image2/BestBuy_US/images/products/5638/5638723_sa.jpg)
- Motherboard: [O'Reilly](https://learning.oreilly.com/library/view/complete-a-guide/9780135291542/graphics/03fig38.jpg)

</details>
