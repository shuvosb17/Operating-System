

# Operating Systems — Deep Theory Notes

---

## 📋 Exam Strategy Recap

| Layer | Type | Marks |
|-------|------|-------|
| L1, L2 | Theory | 12–14 marks |
| L3 | Analytical | 10–12 marks (5 algorithms) |
| L6 | Mostly Math | — |

> **Exam order:** Start with math/algorithms → then theory.

---

# Chapter 1 — Introduction to Operating Systems

---

## 1.1 What is an Operating System?

### Definition
An **Operating System (OS)** is system software that acts as an **intermediary (bridge)** between the **user** and the **computer hardware**. It manages hardware resources and provides services for application software.

### Feel the Concept
Imagine you walk into a restaurant. You (the **user**) don't go into the kitchen (the **hardware**) yourself. Instead, you talk to a **waiter** (the **OS**), who takes your order to the kitchen, manages what gets cooked when, and brings the result back to you. Without the waiter, the kitchen would be chaos — everyone shouting at the chef simultaneously.

```
┌─────────────────────────────────────────────┐
│                    USERS                     │
│         (You, me, other programs)            │
├─────────────────────────────────────────────┤
│            APPLICATION SOFTWARE              │
│     (Word, Chrome, Games, Compilers)         │
├─────────────────────────────────────────────┤
│            SYSTEM SOFTWARE                   │
│  ┌───────────────────────────────────────┐   │
│  │        OPERATING SYSTEM               │   │
│  │   (The Bridge / Intermediary)         │   │
│  └───────────────────────────────────────┘   │
├─────────────────────────────────────────────┤
│              HARDWARE                        │
│      (CPU, RAM, Disk, I/O Devices)           │
└─────────────────────────────────────────────┘
```

### Software Types Explained
- **Application Software:** Programs users interact with directly (Word, Chrome, Photoshop). They *cannot* talk to hardware directly.
- **System Software:** Programs that manage the computer itself (OS, drivers, utilities). The OS is the most important system software — it translates application requests into hardware operations.

**Why the bridge matters:** If Chrome wants to read a file from disk, it doesn't know the disk's physical addresses. It asks the OS. The OS knows the disk layout, performs the read, and hands the data back. Without this bridge, every application would need to understand every possible hardware configuration — impossible.

---

## 1.2 Goals of an Operating System

| Goal | What it means | Analogy |
|------|--------------|---------|
| **1. Efficiency** (Solving problems efficiently) | Maximize resource utilization — CPU shouldn't sit idle, memory shouldn't be wasted | A factory manager ensuring every machine and worker is productive |
| **2. Convenience** (User-friendly interface) | Provide an easy UI so users don't need to understand hardware | ATM machine — you don't need to know bank's internal database to withdraw money |
| **3. Security** | Protect data, processes, and users from unauthorized access | A bank vault — only authorized personnel can enter |

### Exam Trap:
> Q: "Are the goals of OS and functions of OS the same?"  
> **No.** Goals = *what it aims to achieve*. Functions/Operations = *how it achieves them*.

---

## 1.3 Four Components of a Computer System

```
┌──────────────────────────────────────────────┐
│  Component 4: USERS                          │
│  (People, machines, other computers)         │
├──────────────────────────────────────────────┤
│  Component 3: APPLICATION PROGRAMS           │
│  (Compilers, Web browsers, Games,            │
│   Database systems, Word processors)         │
├──────────────────────────────────────────────┤
│  Component 2: OPERATING SYSTEM               │
│  (Controls and coordinates hardware use      │
│   among various applications and users)      │
├──────────────────────────────────────────────┤
│  Component 1: HARDWARE                       │
│  (CPU, Memory, I/O devices)                  │
│  (Provides basic computing resources)        │
└──────────────────────────────────────────────┘
```

**Deep understanding:** These four layers form a **hierarchy of abstraction**. Each layer only communicates with its adjacent layers. Users never directly touch hardware; they use applications, which use the OS, which uses hardware.

---

## 1.4 Operations of an OS

### 1. Resource Allocation
The OS decides **who gets what, when, and for how long**.

Resources include: CPU time, memory space, disk space, I/O devices.

**Analogy:** Think of a traffic policeman at a busy intersection. Multiple cars (processes) want to cross (use resources). The policeman (OS) decides which car goes first, how long it gets, and ensures no collision (deadlock).

### 2. Program Controller
The OS **controls the execution** of programs to prevent errors and misuse of the computer.

It ensures:
- Programs don't interfere with each other
- Programs don't access unauthorized memory
- Programs are properly started, paused, resumed, and terminated

---

## 1.5 OS Components (OS(A) Components)

The OS provides several major components/services:

1. **Process Management** — Creating, scheduling, terminating processes
2. **Memory Management** — Allocating/deallocating memory
3. **File System Management** — Creating, deleting, reading, writing files
4. **I/O System Management** — Managing input/output devices
5. **Secondary Storage Management** — Managing disk space
6. **Networking** — Communication between systems
7. **Protection & Security System** — Controlling access to resources
8. **Command Interpreter (Shell)** — Interface between user and OS

---

## 1.6 Kernel & Caching

### What is a Kernel?

The **Kernel** is the **core program** of the OS that is **always loaded in memory**. It manages all I/O requests from software and translates them into data-processing instructions for the CPU and other hardware.

```
┌─────────────────────────────────────┐
│           OPERATING SYSTEM          │
│  ┌───────────────────────────────┐  │
│  │          SHELL                │  │
│  │    (User Interface Layer)     │  │
│  │  ┌─────────────────────────┐  │  │
│  │  │        KERNEL           │  │  │
│  │  │  ┌───────────────────┐  │  │  │
│  │  │  │  Process Mgmt     │  │  │  │
│  │  │  │  Memory Mgmt      │  │  │  │
│  │  │  │  Device Drivers   │  │  │  │
│  │  │  │  System Calls     │  │  │  │
│  │  │  │  File System      │  │  │  │
│  │  │  └───────────────────┘  │  │  │
│  │  │   ♥ HEART OF THE OS ♥   │  │  │
│  │  └─────────────────────────┘  │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

**Why it's important:**
- It's the **first program loaded** during boot and the **last one to shut down**
- It runs in **privileged/kernel mode** — it can access ALL hardware
- Without the kernel, no program can communicate with hardware
- It handles **interrupts, scheduling, memory management** — the critical stuff

**Analogy:** The kernel is like the **brain stem** in the human body. You can survive without some parts of your brain, but without the brain stem (which controls breathing, heartbeat), you die. Similarly, the OS can lose some features, but without the kernel, nothing works.

### What is Caching?

**Caching** = Storing copies of frequently accessed data in a **faster, smaller storage** to speed up future access.

**Why it exists:** There's a fundamental trade-off in storage — **fast storage is expensive and small**, while **cheap storage is large but slow**. Caching bridges this gap.

### Storage Hierarchy (Pyramid — Top is fastest, Bottom is largest)

```
            ┌───────┐
            │Registers│  ← Fastest, smallest, most expensive
            ├─────────┤     (inside CPU, nanoseconds)
            │  Cache  │  ← Very fast (L1, L2, L3 cache)
            ├───────────┤    (SRAM, nanoseconds)
            │ Main Memory│ ← Fast (RAM)
            ├─────────────┤   (DRAM, microseconds)
            │  SSD / HDD   │ ← Moderate (Secondary storage)
            ├───────────────┤  (milliseconds)
            │ Optical Disk   │ ← Slow
            ├─────────────────┤
            │  Magnetic Tape   │ ← Slowest, largest, cheapest
            └──────────────────┘   (backup/archival)
            
   SPEED ↑  decreases downward
   SIZE  ↑  increases downward
   COST  ↑  decreases downward
```

**How caching works in practice:**
1. CPU needs data → checks **Register** → found? Use it (cache *hit*)
2. Not in Register → checks **Cache (L1→L2→L3)** → found? Copy to register, use it
3. Not in Cache → checks **RAM** → found? Copy to cache AND register, use it
4. Not in RAM → checks **Disk** → found? Load into RAM → cache → register
5. Not on Disk → **Error / Page Fault**

**Key insight:** Caching works because of **locality of reference** — programs tend to access the same data (temporal locality) or nearby data (spatial locality) repeatedly.

---

## 1.7 OS Structure: Multiprogramming & Multitasking

### Multiprogramming

**Definition:** Multiple programs are loaded into memory **simultaneously**, and the CPU switches between them so that the **CPU is never idle**.

```
Time →  ─────────────────────────────────────────

        ┌─────┐      ┌─────┐      ┌─────┐
CPU:    │ P1  │      │ P2  │      │ P3  │
        └──┬──┘      └──┬──┘      └──┬──┘
           │            │            │
    P1 does I/O    P2 does I/O   P3 does I/O
    (CPU free,     (CPU free,    
     so run P2)     so run P3)   

Memory:  ┌──────┐
         │  P1  │  ← All loaded simultaneously
         │  P2  │
         │  P3  │
         │  OS  │
         └──────┘
```

**Key idea:** When Process 1 is waiting for I/O (disk read, keyboard input), rather than letting the CPU sit idle, the OS gives the CPU to Process 2. **CPU should ALWAYS be busy.**

**Analogy:** A chef (CPU) cooking multiple dishes. When the pasta is boiling (waiting/I/O), the chef doesn't stand there staring — they chop vegetables for the salad (another process). Maximum productivity.

### Multitasking (Time-Sharing)

**Definition:** A **logical extension** of multiprogramming where the CPU switches between processes **so rapidly** that users can interact with each program while it is running — giving the **illusion** of simultaneous execution.

```
Time →  ─────────────────────────────────────────

        ┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐
CPU:    │P1││P2││P3││P1││P2││P3││P1││P2││P3│
        └──┘└──┘└──┘└──┘└──┘└──┘└──┘└──┘└──┘
         ←time slice→

Each process gets a small time quantum (e.g., 10ms)
Switches happen so fast, USER thinks all run simultaneously
```

### Key Differences

| Feature | Multiprogramming | Multitasking |
|---------|-----------------|--------------|
| **Goal** | Maximize CPU utilization | User interaction + CPU utilization |
| **Switching trigger** | Only when current process does I/O | After fixed time quantum (even if process isn't done) |
| **User interaction** | Minimal | High — user interacts with running programs |
| **Response time** | Not a concern | Must be fast (< 1 second) |
| **Example** | Batch processing in mainframes | Windows/Mac/Linux desktops |

---

## 1.8 Protection vs Security

| Feature | Protection | Security |
|---------|-----------|----------|
| **Definition** | Mechanism for controlling access of processes/users to resources **within** the system | Defense of the system against **external and internal** threats |
| **Scope** | Internal — between processes and users | External — against attacks, viruses, unauthorized access |
| **Focus** | WHO can access WHAT resources | Ensuring the ENTIRE system is safe |
| **Example** | Process A cannot read Process B's memory | Firewall blocking hackers, antivirus detecting malware |
| **Analogy** | Doors with locks inside a building (restricting room access for employees) | The building's security guard, fence, and CCTV cameras |

**Key insight:** Protection is a **subset** of Security. You can have protection without full security, but you can't have security without protection.

---

## 1.9 Computing Environments

| Environment | Description | Example |
|-------------|------------|---------|
| **Traditional** | Stand-alone general-purpose machines | Desktop PCs |
| **Client-Server** | Server provides services; clients consume them | Web server & browser |
| **Peer-to-Peer (P2P)** | All nodes are equal — both client and server | BitTorrent, Blockchain |
| **Cloud Computing** | Resources delivered over the internet as services (SaaS, PaaS, IaaS) | AWS, Google Cloud |
| **Mobile Computing** | Handheld devices with OS | Android, iOS |
| **Embedded Systems** | OS runs on specific hardware for specific tasks | Smart TV, washing machine, car ECU |
| **Real-Time** | Strict time constraints — must respond within deadline | Medical devices, rocket control systems |
| **Distributed** | Multiple independent computers appear as single coherent system | Google's infrastructure |

---

## 1.10 Von Neumann Architecture

This is the **fundamental architecture** on which most modern computers are based.

### Core Idea
**Programs and data are stored in the SAME memory.** Instructions are *fetched* from memory, *decoded*, and *executed* — one at a time (sequential execution).

### Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                     VON NEUMANN ARCHITECTURE                  │
│                                                              │
│  ┌─────────────┐     System Bus      ┌──────────────────┐   │
│  │   INPUT      │◄──────────────────►│                  │   │
│  │  DEVICES     │                     │    MEMORY        │   │
│  │ (Keyboard,   │     ┌───────────┐   │  (RAM + ROM)     │   │
│  │  Mouse)      │     │           │   │                  │   │
│  └─────────────┘     │   CPU     │   │  ┌────────────┐  │   │
│                       │           │   │  │Instructions│  │   │
│  ┌─────────────┐     │ ┌───────┐ │◄─►│  │    +       │  │   │
│  │   OUTPUT     │     │ │  ALU  │ │   │  │   Data     │  │   │
│  │  DEVICES     │◄───►│ └───────┘ │   │  │ (stored    │  │   │
│  │ (Monitor,    │     │ ┌───────┐ │   │  │  together) │  │   │
│  │  Printer)    │     │ │  CU   │ │   │  └────────────┘  │   │
│  └─────────────┘     │ └───────┘ │   └──────────────────┘   │
│                       │ ┌───────┐ │                          │
│                       │ │Registers│                          │
│                       │ └───────┘ │                          │
│                       └───────────┘                          │
│                                                              │
│  Instruction Cycle: FETCH → DECODE → EXECUTE → STORE        │
└──────────────────────────────────────────────────────────────┘
```

### Components Explained:
- **CPU (Central Processing Unit):**
  - **ALU (Arithmetic Logic Unit):** Performs arithmetic (+, -, ×, ÷) and logic (AND, OR, NOT) operations
  - **CU (Control Unit):** Directs the operation of the processor — tells ALU, memory, and I/O what to do based on the instruction
  - **Registers:** Ultra-fast, tiny storage inside CPU (holds current instruction, addresses, data)
- **Memory:** Single memory stores BOTH instructions AND data (this is the key Von Neumann idea)
- **I/O Devices:** Interface between computer and outside world
- **System Bus:** Communication pathway connecting all components (Address Bus, Data Bus, Control Bus)

### The Fetch-Decode-Execute Cycle:
```
  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
  │  FETCH   │───►│  DECODE  │───►│ EXECUTE  │───►│  STORE   │
  │          │    │          │    │          │    │          │
  │ Get next │    │ Figure   │    │ Perform  │    │ Write    │
  │ instruc- │    │ out what │    │ the      │    │ result   │
  │ tion from│    │ it means │    │ operation│    │ back to  │
  │ memory   │    │          │    │          │    │ memory   │
  └──────────┘    └──────────┘    └──────────┘    └──────────┘
       ▲                                              │
       └──────────────────────────────────────────────┘
                    (Repeat forever)
```

**Von Neumann Bottleneck:** Since instructions and data share the same bus, the CPU can either fetch an instruction OR fetch/write data — not both at the same time. This creates a bottleneck.

---

## 1.11 ROM, EPROM, EEPROM

These are types of **non-volatile memory** (data persists without power).

| Type | Full Name | Description | Analogy |
|------|-----------|-------------|---------|
| **ROM** | Read-Only Memory | Data written once during manufacturing. **Cannot be modified.** | A printed book — you can read it, but you can't erase and rewrite the text |
| **PROM** | Programmable ROM | Can be written **once** by the user after manufacturing using a special device | A blank book you can write in with permanent ink — once written, can't change |
| **EPROM** | Erasable Programmable ROM | Can be **erased using UV light** and reprogrammed | A whiteboard that requires a special UV eraser — inconvenient but possible |
| **EEPROM** | Electrically Erasable Programmable ROM | Can be **erased electrically** and reprogrammed **byte by byte** | A digital whiteboard — erase and rewrite easily with electricity |
| **Flash Memory** | — | Evolution of EEPROM — can erase **blocks** at once (faster) | USB drives, SSDs |

### Why this matters for System Boot:
When you power on your computer, the CPU needs instructions to start. These initial instructions (**bootstrap program / firmware**) are stored in **ROM/EEPROM** because:
1. They must survive power-off (non-volatile)
2. They shouldn't be accidentally modified (read-only or controlled-write)
3. They're the **first thing the CPU reads** on boot

---

## 1.12 Interrupts

### What is an Interrupt?

An **interrupt** is a **signal** sent to the CPU that says: **"Stop what you're doing, something important needs attention!"**

The CPU pauses its current task, handles the interrupt, then resumes what it was doing.

```
CPU executing Process A
        │
        │ ←── INTERRUPT signal arrives!
        │     (e.g., keyboard key pressed)
        ▼
┌─────────────────────┐
│ Save current state   │  (Save registers, PC of Process A)
│ of Process A         │
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ Jump to ISR          │  (Interrupt Service Routine)
│ Handle the interrupt │  (Read keyboard input)
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ Restore state of     │  (Restore registers, PC)
│ Process A            │
└──────────┬──────────┘
           ▼
CPU resumes Process A exactly where it left off
```

### Why is the OS called Interrupt-Driven?

The OS doesn't constantly check "is there something to do?" (that would waste CPU). Instead, it **waits for interrupts**. Everything that happens — a key press, a disk read completion, a timer tick, an error — triggers an interrupt. The OS **reacts** to these interrupts.

**Analogy:** A doctor doesn't go door to door asking "are you sick?" Instead, patients **come to** the doctor (interrupt). The doctor handles each patient (ISR), then waits for the next one.

### Common Functions of Interrupts:
1. **Transfer control** to the appropriate interrupt service routine
2. **Save the state** of the interrupted process (registers, program counter)
3. **Incoming interrupts are disabled** while another interrupt is being processed (to prevent chaos)
4. After handling, **restore state** and resume the interrupted process

### Types of Interrupts:

```
                    INTERRUPTS
                        │
           ┌────────────┴────────────┐
      Hardware                    Software
      Interrupts                  Interrupts
      (External)                  (Internal)
           │                          │
    ┌──────┴──────┐            ┌──────┴──────┐
    │ I/O device  │            │   Trap /    │
    │ completion  │            │  Exception  │
    │ Timer       │            │ System Call │
    │ Keyboard    │            │ Division    │
    │ Mouse       │            │ by zero     │
    └─────────────┘            └─────────────┘
```

### Vectored Interrupt

In a **vectored interrupt** system, each interrupt type has a **specific number/vector** that directly points to its ISR's address in an **interrupt vector table**.

```
Interrupt Vector Table:
┌────────┬──────────────────────┐
│ Vector │  ISR Address         │
├────────┼──────────────────────┤
│   0    │  → Divide-by-zero    │
│   1    │  → Keyboard handler  │
│   2    │  → Timer handler     │
│   3    │  → Disk I/O handler  │
│  ...   │  ...                 │
└────────┴──────────────────────┘

When interrupt #2 fires → CPU immediately jumps to Timer handler
No searching needed — DIRECT mapping (like speed dial on a phone)
```

### ISR (Interrupt Service Routine)

The **ISR** is the **actual code** that handles a specific interrupt. Each interrupt type has its own ISR.

**Flow:** Interrupt occurs → CPU looks up the interrupt vector → jumps to ISR → ISR handles the event → returns control

### Trap / Exception

A **Trap** (also called an **Exception**) is a **software-generated interrupt** caused by:
- An **error** (division by zero, invalid memory access, stack overflow)
- A **deliberate system call** (user program requesting OS service)

**Key difference from hardware interrupt:**
- Hardware interrupt = **external** event (keyboard press). Asynchronous.
- Trap/Exception = **internal** event (error in the code or deliberate call). Synchronous.

### Interrupt Handling Process:
```
1. Interrupt/Trap occurs
2. CPU finishes current instruction
3. CPU acknowledges the interrupt
4. CPU saves state (PC, registers) onto the stack
5. CPU determines interrupt type (polling or vectored)
6. CPU jumps to the ISR
7. ISR executes
8. ISR completes → CPU restores saved state
9. CPU resumes the interrupted process
```

---

## 1.13 Types of Operating Systems

### 1. Batch OS
```
┌─────────┐    ┌─────────┐    ┌─────────┐
│ Job 1   │───►│ Job 2   │───►│ Job 3   │───► ...
└─────────┘    └─────────┘    └─────────┘
  No user interaction during execution
  Jobs are grouped (batched) and processed sequentially
```
- **No direct interaction** between user and computer during execution
- Jobs are collected, grouped, and executed one after another
- **Problem:** If one job takes forever, all others wait
- **Example:** Payroll processing, bank statement generation

### 2. Time-Sharing OS (Multitasking)
- Multiple users share the computer **simultaneously**
- CPU switches between users/processes rapidly (time quantum)
- Each user feels they have the **entire computer** to themselves
- **Example:** Unix, Linux servers with multiple SSH users

### 3. Distributed OS
- Multiple independent computers connected by a network
- Appear to users as a **single coherent system**
- **Resources are shared** across machines
- **Example:** Google's data centers, Apache Hadoop cluster

### 4. Network OS
- Computers connected via a network but each has its **own OS**
- Provides services like **file sharing, print sharing** across the network
- Users are **aware** they're on different machines
- **Example:** Windows Server providing file shares

### 5. Real-Time OS (RTOS)
- **Strict time constraints** — must respond within a **deadline**
- **Hard Real-Time:** Missing a deadline = system failure (pacemaker, missile system)
- **Soft Real-Time:** Missing a deadline = degraded quality but system continues (video streaming, gaming)

### Comparison Table (Scenario-Based):

| Scenario | OS Type |
|----------|---------|
| "Process bank transactions overnight without user interaction" | Batch |
| "10 students using same server simultaneously for coding" | Time-Sharing |
| "Cars' anti-lock braking system must respond within 5ms" | Real-Time (Hard) |
| "Netflix streaming video" | Real-Time (Soft) |
| "Google processing search queries across thousands of servers" | Distributed |
| "Office computers sharing a printer" | Network |

### iOS vs Android

| Feature | iOS | Android |
|---------|-----|---------|
| **Kernel** | XNU (hybrid kernel, based on Mach + BSD) | Linux kernel (monolithic) |
| **Source** | Closed source (proprietary) | Open source (AOSP) |
| **Hardware** | Only Apple devices | Many manufacturers (Samsung, Xiaomi, etc.) |
| **App Store** | App Store (strict review) | Play Store (less strict) + sideloading |
| **Customization** | Limited | Highly customizable |
| **Security** | Sandboxed apps, strict | More vulnerable due to openness |
| **Architecture** | ARM-based, tightly integrated | ARM-based, fragmented |

### Multiprocess Architecture (Scenario-Based)
Chrome browser uses **multiprocess architecture** — each tab runs as a **separate process**. If one tab crashes, others keep working. This illustrates process isolation.

```
Chrome Browser
├── Browser Process (main)
├── Tab 1 → Renderer Process (PID 1001)
├── Tab 2 → Renderer Process (PID 1002)  ← THIS CRASHES
├── Tab 3 → Renderer Process (PID 1003)  ← Still works!
├── Tab 4 → Renderer Process (PID 1004)  ← Still works!
└── GPU Process
```

### System Call Diagram
*(Covered in detail in Chapter 2)*

---

# Chapter 2 — OS Services & System Calls

---

## 2.1 Services of the Operating System

The OS provides two categories of services:

### Category 1: Helpful to the User
```
┌────────────────────────────────────────────────────┐
│              OS SERVICES (User-Facing)              │
├────────────────────────────────────────────────────┤
│                                                    │
│  1. USER INTERFACE                                 │
│     └─ CLI, GUI, Touch Screen                      │
│                                                    │
│  2. PROGRAM EXECUTION                              │
│     └─ Load program into memory, run it, end it    │
│                                                    │
│  3. I/O OPERATIONS                                 │
│     └─ File read/write, device access              │
│                                                    │
│  4. FILE SYSTEM MANIPULATION                       │
│     └─ Create, delete, read, write, search files   │
│                                                    │
│  5. COMMUNICATION                                  │
│     └─ Between processes (IPC) or across network   │
│                                                    │
│  6. ERROR DETECTION                                │
│     └─ CPU errors, memory errors, I/O errors       │
│                                                    │
└────────────────────────────────────────────────────┘
```

### Category 2: Ensuring Efficient System Operation
```
┌────────────────────────────────────────────────────┐
│           OS SERVICES (System Efficiency)           │
├────────────────────────────────────────────────────┤
│                                                    │
│  7. RESOURCE ALLOCATION                            │
│     └─ CPU scheduling, memory allocation           │
│                                                    │
│  8. ACCOUNTING                                     │
│     └─ Track which user uses how much resources    │
│                                                    │
│  9. PROTECTION & SECURITY                          │
│     └─ Controlling access, authentication          │
│                                                    │
└────────────────────────────────────────────────────┘
```

### OS Services Diagram

```
┌───────────────────────────────────────────────────────────────┐
│                          USERS                                │
│              ┌──────┐  ┌──────┐  ┌──────┐                    │
│              │User 1│  │User 2│  │User 3│                    │
│              └──┬───┘  └──┬───┘  └──┬───┘                    │
│    ┌────────────┼─────────┼────────┼────────────────┐        │
│    │      USER INTERFACE (GUI / CLI / Touch Screen)  │        │
│    └────────────────────────┬────────────────────────┘        │
│                             │                                 │
│    ┌────────────────────────┴────────────────────────┐        │
│    │              SYSTEM CALL INTERFACE               │        │
│    └──┬──────┬──────┬──────┬──────┬──────┬──────┬───┘        │
│       │      │      │      │      │      │      │            │
│    ┌──┴──┐┌──┴──┐┌──┴──┐┌──┴──┐┌──┴──┐┌──┴──┐┌──┴──┐       │
│    │Prog ││ I/O ││File ││Comm-││Error││Resrc││Prot-│       │
│    │Exec ││ Ops ││Sys  ││unic ││Det- ││Allo-││ect &│       │
│    │     ││     ││Manip││ation││ect  ││cation││Sec  │       │
│    └─────┘└─────┘└─────┘└─────┘└─────┘└─────┘└─────┘       │
│                             │                                 │
│    ┌────────────────────────┴────────────────────────┐        │
│    │                   KERNEL                         │        │
│    └────────────────────────┬────────────────────────┘        │
│                             │                                 │
│    ┌────────────────────────┴────────────────────────┐        │
│    │                  HARDWARE                        │        │
│    └─────────────────────────────────────────────────┘        │
└───────────────────────────────────────────────────────────────┘
```

---

## 2.2 User Interface: CLI vs GUI

### CLI (Command Line Interface)
- User types **text commands** to interact with OS
- Requires memorizing commands
- **Powerful** — full control over the system
- **Fast** for experienced users
- **Example:** Terminal (Linux), Command Prompt (Windows), PowerShell

### GUI (Graphical User Interface)
- User interacts using **icons, windows, menus, mouse clicks**
- **Intuitive** — no need to memorize commands
- Easier for beginners
- **Example:** Windows Desktop, macOS Finder

### Characteristics of GUI:
- **Desktop metaphor** — files, folders, trash can
- **WIMP** — Windows, Icons, Menus, Pointers
- **Direct manipulation** — drag and drop
- **Visual feedback** — progress bars, animations
- **Consistency** — standard layouts across applications

### CLI vs GUI vs Touch Screen

| Attribute | CLI | GUI | Touch Screen |
|-----------|-----|-----|-------------|
| **Input method** | Keyboard (typed commands) | Mouse + Keyboard | Finger gestures |
| **Learning curve** | Steep | Moderate | Easiest |
| **Speed (expert)** | Fastest | Moderate | Moderate |
| **Speed (beginner)** | Slow | Fast | Fastest |
| **Resource usage** | Minimal | Heavy (graphics) | Heavy |
| **Automation** | Excellent (scripting) | Limited | Very limited |
| **Precision** | High | High | Low (fat finger problem) |
| **Accessibility** | Poor (need to see text) | Good | Very good |
| **Example** | bash, PowerShell | Windows 11, macOS | iOS, Android |

---

## 2.3 System Calls

### Definition
A **system call** is the **programmatic interface** through which a user program requests a service from the operating system's **kernel**. It is the **controlled gateway** from **user mode** to **kernel mode**.

**Analogy:** A system call is like going through **immigration at an airport**. You (user program) are in the public area (user mode). You need to enter a restricted area (kernel mode). You go to the immigration counter (system call interface), show your passport (parameters), the officer (OS) processes your request, and either lets you through or denies you.

### Why System Calls Exist:
User programs run in **user mode** (restricted — can't directly access hardware). To perform privileged operations (read file, allocate memory, send network data), they must ask the kernel through system calls.

### Example:
When you write `printf("Hello")` in C:
```
Your C code: printf("Hello")
     │
     ▼ (C library translates this)
System Call: write(1, "Hello", 5)
     │
     ▼ (Trap to kernel mode)
KERNEL: Sends "Hello" to the display hardware
     │
     ▼ (Return to user mode)
Your program continues
```

### What is an API?

An **API (Application Programming Interface)** is a **set of functions/definitions** that a programmer can use. It provides a **higher-level, more convenient** interface to system calls.

**Why use APIs instead of system calls directly?**

1. **Portability:** APIs remain the same even if underlying system calls change. Write once, run anywhere.
2. **Simplicity:** APIs wrap complex system call sequences into simple function calls
3. **Safety:** APIs add error checking and parameter validation
4. **Abstraction:** Programmers don't need to know OS internals

```
Programmer uses:    fopen("file.txt", "r")     ← API (C Library)
                           │
                           ▼ (translates to)
System Call:          open("file.txt", O_RDONLY)  ← System Call
                           │
                           ▼ (trap to kernel)
Kernel executes:    Actually opens the file on disk
```

**Common APIs:**
- **Win32 API** — Windows
- **POSIX API** — Unix/Linux/macOS
- **Java API** — Java (platform-independent)

### Types of System Calls (with examples)

| Category | Purpose | Example System Calls |
|----------|---------|---------------------|
| **1. Process Control** | Create/end/manage processes | `fork()`, `exec()`, `exit()`, `wait()`, `kill()` |
| **2. File Management** | Create/read/write/delete files | `open()`, `read()`, `write()`, `close()`, `unlink()` |
| **3. Device Management** | Request/release/manage devices | `ioctl()`, `read()`, `write()` to devices |
| **4. Information Maintenance** | Get/set system data | `getpid()`, `time()`, `sleep()`, `gethostname()` |
| **5. Communication** | Create connections, send/receive msgs | `socket()`, `send()`, `recv()`, `pipe()`, `shmget()` |
| **6. Protection** | Set/get permissions | `chmod()`, `chown()`, `umask()` |

### System Call Implementation: User Mode → Kernel Mode

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│   USER MODE (unprivileged)                                   │
│   ┌──────────────────────────────────┐                       │
│   │  User Application                │                       │
│   │  calls open("file.txt")         │                       │
│   └───────────────┬──────────────────┘                       │
│                   │                                          │
│   ┌───────────────▼──────────────────┐                       │
│   │  C Library (libc) wrapper        │                       │
│   │  Prepares parameters             │                       │
│   │  Puts system call number in      │                       │
│   │  register (e.g., EAX = 5 for    │                       │
│   │  open)                           │                       │
│   │  Executes TRAP instruction       │                       │
│   │  (software interrupt: int 0x80   │                       │
│   │   or syscall instruction)        │                       │
│   └───────────────┬──────────────────┘                       │
│                   │                                          │
│ ══════════════════╪════════════════════ MODE SWITCH ═════════ │
│                   │   (privilege escalation via TRAP)         │
│                   ▼                                          │
│   KERNEL MODE (privileged)                                   │
│   ┌──────────────────────────────────┐                       │
│   │  System Call Handler             │                       │
│   │  1. Reads system call # from     │                       │
│   │     register                     │                       │
│   │  2. Looks up in System Call      │                       │
│   │     Table                        │                       │
│   │  3. Calls the actual kernel      │                       │
│   │     function: sys_open()         │                       │
│   │  4. Performs the operation        │                       │
│   │  5. Returns result to user       │                       │
│   └───────────────┬──────────────────┘                       │
│                   │                                          │
│ ══════════════════╪════════════════════ MODE SWITCH ═════════ │
│                   ▼                                          │
│   USER MODE — receives result (file descriptor or error)     │
└──────────────────────────────────────────────────────────────┘
```

### System Call Table (Dispatch Table):

```
System Call Table:
┌───────────────┬───────────────────────┐
│ Call Number   │  Kernel Function      │
├───────────────┼───────────────────────┤
│     1         │  sys_exit()           │
│     2         │  sys_fork()           │
│     3         │  sys_read()           │
│     4         │  sys_write()          │
│     5         │  sys_open()    ◄──────│── Our call lands here
│     6         │  sys_close()          │
│    ...        │  ...                  │
└───────────────┴───────────────────────┘
```

**Key characteristics of system calls:**
1. They provide the **only** way for user programs to access kernel services
2. They trigger a **mode switch** (user → kernel → user)
3. Parameters are passed via **registers** or on the **stack**
4. Each system call has a **unique number** in the system call table
5. They are **synchronous** — the calling process waits for the result

---

## 2.4 Microkernel

### What is a Microkernel?

A **microkernel** is an OS design where the kernel contains only the **most essential functions** — everything else runs in **user space** as separate services.

```
MONOLITHIC KERNEL:                    MICROKERNEL:
┌───────────────────┐                ┌───────────────────────┐
│    USER SPACE     │                │      USER SPACE        │
│ ┌───────────────┐ │                │ ┌─────┐ ┌─────┐ ┌───┐│
│ │  Applications │ │                │ │File │ │Net- │ │Dev││
│ └───────────────┘ │                │ │Sys  │ │work │ │Drv││
├───────────────────┤                │ │Srvr │ │Srvr │ │   ││
│   KERNEL SPACE    │                │ └──┬──┘ └──┬──┘ └─┬─┘│
│ ┌───────────────┐ │                │    │       │      │   │
│ │ File System   │ │                │ ┌──┴───────┴──────┴─┐ │
│ │ Network Stack │ │                │ │  MESSAGE PASSING   │ │
│ │ Device Drivers│ │                │ └──┬───────────────┬─┘ │
│ │ Process Mgmt  │ │                ├────┴───────────────┴───┤
│ │ Memory Mgmt   │ │                │    KERNEL SPACE        │
│ │ IPC           │ │                │ ┌───────────────────┐  │
│ │ EVERYTHING!   │ │                │ │  IPC (Messaging)  │  │
│ └───────────────┘ │                │ │  Basic Scheduling │  │
└───────────────────┘                │ │  Basic Memory Mgmt│  │
  (Everything in kernel               │ │  (MINIMAL!)       │  │
   → fast but risky)                  │ └───────────────────┘  │
                                     └────────────────────────┘
                                       (Only essentials in kernel
                                        → safer but slower)
```

### Advantages (Merits):
1. **Reliability:** If a driver or service crashes, it doesn't bring down the entire OS (runs in user space)
2. **Security:** Smaller kernel = smaller attack surface
3. **Extensibility:** Easy to add new services without modifying the kernel
4. **Portability:** Smaller kernel is easier to port to new hardware
5. **Modularity:** Clean separation of concerns

### Disadvantages (Demerits):
1. **Performance overhead:** Services communicate via **message passing** through the kernel — slower than direct function calls in a monolithic kernel
2. **Complexity:** More complex to design correctly
3. **Latency:** More context switches between user space services and kernel

**Example Microkernels:** Mach, QNX, L4, MINIX

---

## 2.5 OS Debugging

**OS Debugging** = The process of **finding and fixing errors** (bugs) in the OS or programs running on it.

### 4 Key Topics:

**1. Log Files:**
The OS writes **error information** to log files. Applications can also log their activities. Analyzing logs helps identify problems.

**2. Core Dump:**
When an **application/process fails**, the OS captures a snapshot of the process's memory at the time of failure. This snapshot is called a **core dump**.

**3. Crash Dump:**
When the **OS itself (kernel) fails**, a snapshot of the kernel's memory is saved. This is called a **crash dump**.

**4. Performance Tuning:**
Monitoring and optimizing system performance using tools (task manager, profilers).

### Core Dump vs Crash Dump:

| Feature | Core Dump | Crash Dump |
|---------|-----------|------------|
| **Triggered by** | Application/process failure | OS/kernel failure |
| **Scope** | Single process's memory | Entire kernel memory |
| **Severity** | One app crashes | Entire system crashes (BSOD, kernel panic) |
| **Contains** | Process memory, registers, stack | Kernel memory, driver state, system state |
| **Example** | Segfault in your C program | Blue Screen of Death (Windows), Kernel Panic (Linux) |
| **Analogy** | One employee collapses at work | The entire office building collapses |

---

## 2.6 System Boot

### What happens when you press the power button?

```
POWER ON
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 1: CPU starts & reads from a FIXED memory address      │
│         (typically in ROM/EEPROM/Flash — firmware)           │
│         This is the BOOTSTRAP PROGRAM (BIOS/UEFI)           │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 2: BIOS/UEFI runs POST (Power-On Self Test)           │
│         - Tests CPU, RAM, keyboard, disk                     │
│         - Checks if hardware is working                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 3: BIOS/UEFI locates the BOOTLOADER on disk           │
│         (GRUB for Linux, Windows Boot Manager, etc.)         │
│         Loads bootloader into RAM                            │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 4: BOOTLOADER loads the KERNEL into RAM                │
│         (The OS kernel — e.g., Linux kernel, NT kernel)      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 5: KERNEL initializes the system                       │
│         - Initializes drivers, file systems                  │
│         - Starts system services/daemons                     │
│         - Shows login screen → SYSTEM READY!                 │
└─────────────────────────────────────────────────────────────┘
```

### Connection to Von Neumann & ROM/EEPROM:
- The CPU follows **Von Neumann architecture** — it fetches instructions from memory
- On boot, RAM is **empty** (volatile — lost data when powered off)
- So the **first instructions** must come from **non-volatile memory** (ROM/EEPROM)
- The **bootstrap program** is stored in ROM/EEPROM because:
  - It survives power-off
  - It's available instantly at a known address
  - Modern systems use **EEPROM/Flash** so firmware can be **updated** (BIOS updates)

**Definition:** System Boot is the process that **loads the kernel (OS) into main memory (RAM)** from secondary storage, starting from the bootstrap program stored in ROM/EEPROM.

---

## 2.7 Spooling

### What is Spooling?

**SPOOL = Simultaneous Peripheral Operations On-Line**

Spooling is a process where data is temporarily held in a **buffer** (usually on disk) to be used by a **slow device** (like a printer) at its own pace, while the CPU moves on to other tasks.

```
WITHOUT SPOOLING:                    WITH SPOOLING:

Process → Printer (WAIT)             Process → Disk Buffer → Printer
   │        (slow!)                      │        (fast!)      (at its own pace)
   │     Can't do anything               │     Free to continue!
   │     until printer finishes           │
   ▼                                     ▼
Process continues                    Process continues IMMEDIATELY

CPU blocked!                         CPU free!
```

**Analogy:** Sending a document to print. Without spooling, you'd have to stand at the printer waiting. With spooling, you "send it to the print queue" (spool) and go back to your work. The printer picks up jobs from the queue at its own speed.

**Key characteristics:**
1. Acts as a **buffer** between fast and slow devices
2. Commonly used with **printers** (print spooling) 
3. Multiple processes can "spool" their output simultaneously
4. The slow device processes them **one by one** from the spool

---

## 2.8 Different Kinds of OS

| Kind | Description |
|------|------------|
| **Batch OS** | Jobs batched, no interaction during execution |
| **Time-Sharing OS** | Multiple users share CPU via time slicing |
| **Distributed OS** | Multiple computers, single coherent system |
| **Network OS** | Independent computers sharing resources via network |
| **Real-Time OS (RTOS)** | Strict time deadlines (hard vs soft) |
| **Multi-processor OS** | Multiple CPUs in one system (parallel processing) |
| **Embedded OS** | Specific hardware, specific purpose (IoT, cars) |
| **Mobile OS** | For mobile devices (Android, iOS) |
| **Desktop OS** | Single-user, multi-tasking (Windows, macOS) |
| **Server OS** | Manages server resources, handles multiple clients |

---

# Chapter 3 — Processes

---

## 3.1 What is a Process?

A **process** is a **program in execution**. It is the **active** entity, while a program is the **passive** entity (just code on disk).

**Analogy:** A **recipe** (program) is just text on paper. When a chef (CPU) starts **cooking** that recipe — gathering ingredients, following steps — that's the **process**. The same recipe can be cooked multiple times simultaneously (multiple processes from the same program).

### The 5 Components of a Process

```
PROCESS MEMORY LAYOUT:

High Address  ┌──────────────────────┐
              │       STACK          │ ← Temporary data
              │ (grows downward ↓)   │    (function parameters,
              │                      │     return addresses,
              │                      │     local variables)
              ├──────────────────────┤
              │          │           │
              │     FREE SPACE       │ ← Stack and Heap grow
              │          │           │    toward each other
              │          ↓           │
              ├──────────────────────┤
              │       HEAP           │ ← Dynamic memory
              │ (grows upward ↑)     │    (malloc, new)
              │                      │    allocated at runtime
              ├──────────────────────┤
              │    DATA SECTION      │ ← Global & static variables
              │                      │    (exists for entire
              │                      │     program lifetime)
              ├──────────────────────┤
              │    TEXT SECTION      │ ← Program code
              │                      │    (machine instructions)
              │                      │    READ-ONLY
Low Address   └──────────────────────┘

+ PROGRAM COUNTER: Points to the CURRENT instruction being
                   executed (represents current activity)
```

### Detailed Explanation of Each Component:

**1. Text Section (Code):**
- The actual **executable instructions** (machine code)
- **Read-only** — doesn't change during execution
- Shared among processes running the same program (memory efficiency)

**2. Program Counter:**
- A **register** that holds the **address of the next instruction** to execute
- Represents the **current activity** of the process
- Essential for **context switching** — when the OS pauses a process, it saves the PC so it can resume exactly where it left off

**3. Stack:**
- Stores **temporary data** that follows **LIFO (Last In, First Out)** order
- Contains: **function parameters, return addresses, local variables**
- Grows/shrinks with function calls and returns
- **Example:** When `main()` calls `foo()` which calls `bar()`:
```
Stack:  | bar()'s locals  |  ← TOP
        | foo()'s locals  |
        | main()'s locals |  ← BOTTOM
```

**4. Data Section:**
- Stores **global variables** and **static variables**
- Allocated at **compile time**, exists for the **entire lifetime** of the process
- Example: `int globalVar = 42;` lives here

**5. Heap:**
- **Dynamically allocated memory** during runtime
- Managed by the programmer (or garbage collector)
- `malloc()` in C, `new` in Java/C++ allocates on the heap
- **Grows upward**, must be **explicitly freed** (or causes memory leaks)

---

## 3.2 Process States

A process goes through **5 states** during its lifetime:

### Process State Diagram:

```
                          admitted                scheduler
              ┌──────────┐      ┌──────────┐    dispatch    ┌──────────┐
  Process  ──►│   NEW    │─────►│  READY   │──────────────►│ RUNNING  │
  Created     │  (Born)  │      │          │◄──────────────│          │
              └──────────┘      └────┬─────┘   interrupt    └──┬───┬──┘
                                     ▲                         │   │
                                     │                         │   │
                             I/O or  │                         │   │  exit
                             event   │      I/O or event       │   │
                           completion│         wait            │   │
                                     │                         │   ▼
                                ┌────┴─────┐                ┌──────────┐
                                │ WAITING  │◄───────────────│TERMINATED│
                                │(Blocked) │                │ (Exit)   │
                                └──────────┘                └──────────┘
```

### State Descriptions:

| State | What's Happening | Analogy |
|-------|-----------------|---------|
| **NEW** | Process is being **created**. OS is allocating resources, initializing PCB | A student has just **registered** for a course — paperwork being processed |
| **READY** | Process is loaded in memory, has everything it needs, and is **waiting for CPU time** | Student is **in the classroom**, prepared, hand raised, waiting for the teacher to call on them |
| **RUNNING** | Process is currently **being executed by the CPU** | Student is at the **whiteboard solving the problem** — they have the teacher's (CPU's) full attention |
| **WAITING (Blocked)** | Process is waiting for some **event** (I/O completion, signal, resource) | Student is waiting for a **lab equipment delivery** — can't proceed without it, even if teacher is available |
| **TERMINATED** | Process has **finished execution** or been killed | Student has **graduated** — removed from active rolls, certificate issued |

### Transitions Explained:

| Transition | Trigger | What happens |
|-----------|---------|-------------|
| NEW → READY | *Admitted* | OS finishes creating the process, loads it into ready queue |
| READY → RUNNING | *Scheduler dispatch* | CPU scheduler selects this process to execute |
| RUNNING → READY | *Interrupt* (timer/preemption) | Process's time quantum expires, OR a higher-priority process arrives |
| RUNNING → WAITING | *I/O or event wait* | Process requests I/O (file read) or waits for an event — CPU can't help, so process blocks |
| WAITING → READY | *I/O or event completion* | The event the process was waiting for has occurred — it's ready to run again |
| RUNNING → TERMINATED | *Exit* | Process finishes execution or is forcibly killed |

**Key insight:** A process can ONLY go from READY → RUNNING (via scheduler). A WAITING process must go to READY first — it can never go directly to RUNNING.

---

## 3.3 PCB (Process Control Block)

The **PCB** is a **data structure** maintained by the OS for **every process**. It contains all the information about a process. Think of it as the process's **identity card + report card + resume** all combined.

```
┌─────────────────────────────────────┐
│      PROCESS CONTROL BLOCK (PCB)     │
├─────────────────────────────────────┤
│  Process ID (PID)                    │  ← Unique identifier
├─────────────────────────────────────┤
│  Process State                       │  ← New/Ready/Running/Waiting/Terminated
├─────────────────────────────────────┤
│  Program Counter (PC)                │  ← Address of next instruction
├─────────────────────────────────────┤
│  CPU Registers                       │  ← Contents of all process-centric
│  (Accumulators, Stack Pointers,      │     registers (saved during switch)
│   Index Registers, General Purpose)  │
├─────────────────────────────────────┤
│  CPU Scheduling Information          │  ← Priority, scheduling queue pointers
├─────────────────────────────────────┤
│  Memory Management Information       │  ← Page tables, segment tables,
│                                      │     base/limit registers
├─────────────────────────────────────┤
│  Accounting Information              │  ← CPU time used, time limits,
│                                      │     process numbers
├─────────────────────────────────────┤
│  I/O Status Information              │  ← List of I/O devices allocated,
│                                      │     open files list
└─────────────────────────────────────┘
```

**Why PCB matters:** When the OS **pauses** a process (context switch), it saves ALL the process's state into its PCB. When the OS **resumes** that process, it reads the PCB and restores everything. Without the PCB, the OS couldn't juggle multiple processes.

### CPU Switch from Process to Process (Context Switch)

```
      Process P0                Operating System               Process P1
      ─────────                 ─────────────────              ─────────
         │                                                        │
 executing│                                                       │ idle
         │                                                        │
         │──── interrupt or system call ────►│                    │
         │                                   │                    │
     idle│     save state into PCB₀          │                    │
         │     ┌──────────────┐              │                    │
         │     │ PCB₀:        │              │                    │
         │     │ PC = 0x4A20  │              │                    │
         │     │ Regs = {...} │              │                    │
         │     │ State = Ready│              │                    │
         │     └──────────────┘              │                    │
         │                                   │                    │
         │           reload state from PCB₁  │                    │
         │           ┌──────────────┐        │                    │
         │           │ PCB₁:        │        │                    │
         │           │ PC = 0x7C10  │        │                    │
         │           │ Regs = {...} │        │                    │
         │           │ State = Run  │        │                    │
         │           └──────────────┘        │                    │
         │                                   │───────────────────►│
         │                                                  executing
         │                                                        │
         │                                   │◄── interrupt ──────│
         │                                   │                    │
         │           save state into PCB₁    │                idle│
         │                                   │                    │
         │           reload state from PCB₀  │                    │
         │                                   │                    │
         │◄──────────────────────────────────│                    │
 executing│                                                       │ idle
         │                                                        │
```

### Role of the Dispatcher:
The **Dispatcher** is the module that gives **control of the CPU** to the process selected by the scheduler. It does:
1. **Context switching** — saving the state of the old process and loading the state of the new process
2. **Switching to user mode** — from kernel mode back to user mode
3. **Jumping to the proper location** — in the new process's code to restart it

**Dispatch latency** = Time it takes for the dispatcher to stop one process and start another. Should be as **fast** as possible.

---

## 3.4 Process Scheduling

### Process Scheduling Queues

Processes move between various **queues** as they change state:

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   JOB QUEUE                                             │
│   ┌───┬───┬───┬───┬───┬───┬───┐                       │
│   │P1 │P2 │P3 │P4 │P5 │P6 │P7 │  ALL processes        │
│   └───┴───┴───┴───┴───┴───┴───┘  in the system         │
│            │                                            │
│  (Long-term scheduler selects)                          │
│            ▼                                            │
│   READY QUEUE                                           │
│   ┌───┬───┬───┬───┐                                    │
│   │P1 │P3 │P5 │P7 │  Processes ready for CPU           │
│   └─┬─┴───┴───┴───┘                                    │
│     │                                                   │
│     │ (Short-term scheduler selects)                    │
│     ▼                                                   │
│   ┌─────┐                                               │
│   │ CPU │ ──── Process executes                         │
│   └──┬──┘                                               │
│      │                                                  │
│      ├── (I/O request) ──► I/O QUEUE ──── (I/O done)    │
│      │                     ┌───┬───┐      ──► back to   │
│      │                     │P2 │P4 │      Ready Queue   │
│      │                     └───┴───┘                    │
│      │                                                  │
│      ├── (time quantum expired) ──► back to Ready Queue │
│      │                                                  │
│      ├── (fork child) ──► child in Ready Queue          │
│      │                                                  │
│      └── (terminate) ──► EXIT                           │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Three Types of Schedulers:

```
┌───────────────────────────────────────────────────────────────────────┐
│                                                                       │
│  LONG-TERM SCHEDULER (Job Scheduler)                                  │
│  ┌─────────────────────────────────────┐                             │
│  │ • Selects which processes should be  │                             │
│  │   loaded into memory from the job    │                             │
│  │   pool (disk)                        │                             │
│  │ • Controls DEGREE OF MULTI-          │     JOB POOL    READY      │
│  │   PROGRAMMING (how many processes    │     (Disk) ────► QUEUE     │
│  │   are in memory)                     │     SLOW                   │
│  │ • Executes INFREQUENTLY (seconds to  │     (runs occasionally)    │
│  │   minutes)                           │                             │
│  │ • Must balance I/O-bound and CPU-    │                             │
│  │   bound processes                    │                             │
│  └─────────────────────────────────────┘                             │
│                                                                       │
│  SHORT-TERM SCHEDULER (CPU Scheduler)                                │
│  ┌─────────────────────────────────────┐                             │
│  │ • Selects which READY process gets   │                             │
│  │   the CPU next                       │     READY       CPU        │
│  │ • Executes VERY FREQUENTLY           │     QUEUE ────► EXECUTION  │
│  │   (milliseconds)                     │     FAST                   │
│  │ • Must be FAST (because it runs      │     (runs very often)      │
│  │   so often)                          │                             │
│  │ • Uses scheduling algorithms (FCFS,  │                             │
│  │   SJF, Priority, RR, etc.)          │                             │
│  └─────────────────────────────────────┘                             │
│                                                                       │
│  MEDIUM-TERM SCHEDULER (Swapper)                                     │
│  ┌─────────────────────────────────────┐                             │
│  │ • SWAP OUT: Remove a process from    │                             │
│  │   memory to disk to free up space    │     MEMORY ────► DISK      │
│  │ • SWAP IN: Bring it back later       │     DISK ────► MEMORY      │
│  │ • Reduces degree of multi-           │                             │
│  │   programming temporarily            │                             │
│  │ • Helps when memory is overcommitted │                             │
│  └─────────────────────────────────────┘                             │
│                                                                       │
└───────────────────────────────────────────────────────────────────────┘
```

### Swap Out (Medium-Term Scheduling):
The OS **removes** a currently less-important process from **main memory (RAM)** and writes it to **disk (swap space)**. This frees up memory for other processes. Later, when resources are available, the process can be **swapped back in**.

```
Before Swap:                         After Swap:
RAM: [P1][P2][P3][P4]  (FULL!)      RAM: [P1][  ][P3][P4]  (Space!)
                                     DISK: [P2] (swapped out)

Later - Swap In:
RAM: [P1][P2][P3][P4]
DISK: [  ] (P2 brought back)
```

**Analogy:** A restaurant (RAM) is full. The manager asks one table's guests (P2) to wait in the lobby (Disk) temporarily so a VIP (new high-priority process) can sit down. Later, when a table opens up, the lobby guests come back.

---

## 3.5 Special Process Concepts

### Zombie Process vs Orphan Process

```
ZOMBIE PROCESS:                          ORPHAN PROCESS:
                                         
Parent ──fork()──► Child                 Parent ──fork()──► Child
  │                  │                     │                  │
  │                  │ (executes)           │ (terminates!)    │ (still running)
  │                  │                     │    EXIT           │
  │                  ▼                     ▼                   │
  │               Child EXITS          Parent GONE             │
  │               (terminated)                                 │
  │               BUT parent hasn't                            │
  │               called wait() yet!                           │
  │                  │                     ┌──────────┐        │
  │               ┌──┴──────────┐          │  INIT    │        │
  │               │ ZOMBIE      │          │ (PID=1)  │◄── adopts
  │               │ (defunct)   │          │ new parent│    the orphan
  │               │ PCB exists  │          └──────────┘
  │               │ but process │
  │               │ is dead     │
  │               └─────────────┘
  │ (calls wait() to read exit status)
  │──────────────►│
  │               │ ← NOW fully removed
```

| Feature | Zombie Process | Orphan Process |
|---------|---------------|----------------|
| **Definition** | Child has **terminated** but parent hasn't read its exit status (via `wait()`) | Parent has **terminated** while child is **still running** |
| **Who's dead?** | The **child** is dead, but its PCB lingers | The **parent** is dead, child is alive |
| **Problem** | PCB entry wastes memory (PID occupied) | Child has no parent — who will read its exit status? |
| **Solution** | Parent calls `wait()` to reap the zombie | OS assigns **init process (PID 1)** as new parent, which will eventually call `wait()` |
| **Danger** | Too many zombies = PID exhaustion (can't create new processes) | Not inherently dangerous (init adopts them) |
| **Analogy** | Dead body lying in the hospital — paperwork not filed yet | A child whose parents died — goes to government foster care (init) |

**Exam tip:** If asked "can a zombie be an orphan?" — Yes! If the parent dies before calling `wait()`, the zombie becomes an orphan zombie. Init adopts it and immediately calls `wait()` to clean it up.

---

### IPC (Inter-Process Communication)

**IPC** is the mechanism that allows **processes to communicate** with each other and **synchronize** their actions.

**Why is IPC needed?**
Processes are normally **isolated** (one process can't access another's memory). But sometimes they need to cooperate — share data, coordinate work, or signal events.

### Two Models of IPC:

```
MODEL 1: SHARED MEMORY                MODEL 2: MESSAGE PASSING

┌──────────┐ ┌──────────┐            ┌──────────┐    ┌──────────┐
│ Process A│ │ Process B│            │ Process A│    │ Process B│
│          │ │          │            │          │    │          │
│  ┌────┐  │ │  ┌────┐  │            │   send() │    │  recv()  │
│  │    │  │ │  │    │  │            │    │     │    │    ▲     │
│  │    ├──┼─┼──┤    │  │            │    ▼     │    │    │     │
│  │    │  │ │  │    │  │            └────┬─────┘    └────┴─────┘
│  └────┘  │ │  └────┘  │                 │              ▲
└──────────┘ └──────────┘                 │              │
                                     ┌────▼──────────────┴────┐
Both processes share a                │   KERNEL (Message      │
common memory region.                 │   Queue / Pipe /       │
Fast but needs                        │   Socket)              │
synchronization (locks).              └─────────────────────────┘
                                    
                                    Processes communicate via
                                    kernel. Slower but safer.
```

### Comparison:

| Feature | Shared Memory | Message Passing |
|---------|--------------|-----------------|
| **Speed** | Fast (direct memory access) | Slower (kernel involvement) |
| **Synchronization** | Programmer must handle (locks, semaphores) | Handled by OS |
| **Ease of use** | Complex (race conditions possible) | Simpler (send/receive) |
| **Best for** | Large data transfers, same machine | Small messages, distributed systems |
| **Example** | POSIX shared memory (`shmget`) | Pipes, message queues, sockets |

---

### Multiprocessor Architecture

A **multiprocessor system** has **two or more CPUs** sharing a single physical memory and I/O devices, managed by a single OS.

```
┌─────────────────────────────────────────────────────┐
│                 SHARED MEMORY (RAM)                   │
└─────┬──────────┬──────────┬──────────┬──────────────┘
      │          │          │          │
   ┌──┴──┐   ┌──┴──┐   ┌──┴──┐   ┌──┴──┐
   │CPU 0│   │CPU 1│   │CPU 2│   │CPU 3│
   └─────┘   └─────┘   └─────┘   └─────┘
      │          │          │          │
└─────┴──────────┴──────────┴──────────┴──────────────┘
                    SYSTEM BUS
```

### Types:

**1. Symmetric Multiprocessing (SMP):**
- All CPUs are **equal** — any CPU can run any task
- Most common (your laptop/phone likely uses this)
- OS schedules processes on ANY available CPU

**2. Asymmetric Multiprocessing (AMP):**
- One CPU is the **master** (runs the OS)
- Other CPUs are **slaves** (assigned specific tasks by the master)
- Simpler but less efficient

### Advantages of Multiprocessor Systems:
1. **Increased throughput** — more CPUs = more work done per unit time
2. **Economy of scale** — cheaper than multiple separate computers (shared memory, disks)
3. **Reliability & fault tolerance** — if one CPU fails, others continue (graceful degradation)

---

# Quick Revision Table — All 3 Chapters

| Topic | Key Idea | Remember This |
|-------|----------|--------------|
| OS Definition | Bridge between user & hardware | Waiter analogy |
| OS Goals | Efficiency, Convenience, Security | — |
| 4 Components | Users → Apps → OS → Hardware | Layered abstraction |
| Kernel | Heart of OS, always in memory | Brain stem analogy |
| Caching | Fast small storage copies of frequent data | Storage pyramid |
| Multiprogramming | CPU never idle, switch on I/O wait | Chef cooking multiple dishes |
| Multitasking | Rapid switching, user interaction | Time slicing |
| Protection vs Security | Internal access control vs overall defense | Doors vs security guard |
| Von Neumann | Programs + Data in same memory | Fetch-Decode-Execute |
| ROM/EPROM/EEPROM | Non-volatile, for bootstrap | UV vs electrical erase |
| Interrupt | Signal to CPU to handle event | Doctor analogy |
| ISR | Code that handles interrupts | — |
| Trap/Exception | Software interrupt (error/syscall) | — |
| OS Types | Batch, Time-Sharing, Distributed, RT, Network | Scenario-based |
| System Call | Controlled gateway user→kernel mode | Immigration counter |
| API | High-level wrapper around system calls | Portability, simplicity |
| Microkernel | Minimal kernel, services in user space | Reliable but slower |
| Core vs Crash Dump | App failure vs OS failure | Employee vs building collapse |
| System Boot | Loads kernel into RAM from ROM | BIOS → Bootloader → Kernel |
| Spooling | Buffer between fast CPU and slow device | Print queue |
| Process | Program in execution, 5 parts | Recipe vs cooking |
| 5 Parts | Text, PC, Stack, Data, Heap | Memory layout diagram |
| Process States | New, Ready, Running, Waiting, Terminated | State diagram |
| PCB | Process's identity card | All info about a process |
| Context Switch | Save old state, load new state | Dispatcher handles this |
| 3 Schedulers | Long-term (slow), Short-term (fast), Medium (swap) | — |
| Zombie | Child dead, parent hasn't called wait() | Body in hospital |
| Orphan | Parent dead, child still running | Init adopts |
| IPC | Shared Memory vs Message Passing | Fast vs Safe |
| Multiprocessor | Multiple CPUs, shared memory | SMP vs AMP |

---

> **Final Tip:** For exam, draw diagrams wherever possible — they earn extra marks and show deep understanding. Practice the process state diagram, Von Neumann architecture, context switch diagram, and OS services diagram until you can draw them from memory.
