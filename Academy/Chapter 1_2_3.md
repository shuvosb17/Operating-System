

# OS — Compact Theory Notes (Chapters 1–3)

---

## Chapter 1 — Introduction to OS

### What is an OS?
**Intermediary (bridge)** between user/software and hardware. Manages resources, provides services.

```
USERS → APPLICATION SOFTWARE → OS (Bridge) → HARDWARE
```

**Goals:** ① Efficiency (max resource use) ② Convenience (easy UI) ③ Security

**4 Components:** Hardware → OS → Application Software → Users

**OS Operations:** ① Resource Allocation (who gets what, when) ② Program Controller (control execution, prevent misuse)

---

### Kernel & Caching

**Kernel** = Core program of OS, always in memory, manages all I/O requests. **Heart of OS.** Runs in privileged mode.

**Caching** = Copy frequently used data in faster, smaller storage.

```
Storage Hierarchy (fast→slow, small→large):
Registers > Cache (L1>L2>L3) > RAM > SSD/HDD > Tape
```
Works because of **locality of reference** (programs access same/nearby data repeatedly).

---

### Multiprogramming vs Multitasking

| | Multiprogramming | Multitasking |
|--|-----------------|--------------|
| **Goal** | CPU never idle | User interaction + CPU utilization |
| **Switch trigger** | On I/O wait | Fixed time quantum |
| **Interaction** | Minimal | High |

---

### Protection vs Security

- **Protection:** Internal — controlling resource access *between* processes/users (doors inside building)
- **Security:** External + Internal — defending against threats (building's guard & CCTV)

---

### Von Neumann Architecture
Programs & data stored in **SAME memory**. Sequential fetch-decode-execute cycle.

```
INPUT ──► ┌─────────────┐ ◄──► MEMORY (Instructions + Data)
OUTPUT ◄──┤ CPU         │
          │ ALU + CU +  │
          │ Registers   │
          └─────────────┘
Cycle: FETCH → DECODE → EXECUTE → STORE → repeat
```
**Bottleneck:** Instructions & data share same bus — can't fetch both simultaneously.

---

### ROM Types

| Type | Write | Erase | Key point |
|------|-------|-------|-----------|
| **ROM** | Factory only | Can't | Permanent |
| **PROM** | Once by user | Can't | One-time programmable |
| **EPROM** | Multiple | UV light | Inconvenient erase |
| **EEPROM** | Multiple | Electrically | Modern firmware, byte-level |
| **Flash** | Multiple | Electrically | Block-level, SSDs/USBs |

Bootstrap program stored in ROM/EEPROM (survives power-off, first thing CPU reads on boot).

---

### Interrupts

**Interrupt** = Signal to CPU: "Stop, handle this!" CPU pauses → saves state → runs ISR → restores state → resumes.

```
Hardware Interrupts (external): I/O, timer, keyboard
Software Interrupts (internal): Trap/Exception — errors, system calls
```

- **ISR (Interrupt Service Routine):** Code that handles a specific interrupt
- **Vectored Interrupt:** Each interrupt type maps directly to ISR address via interrupt vector table
- **Trap/Exception:** Software-generated interrupt (divide-by-zero, system call)
- **OS is interrupt-driven:** It reacts to interrupts rather than constantly polling

---

### Types of OS

| Type | Key Feature | Example |
|------|------------|---------|
| Batch | No user interaction, jobs batched | Payroll |
| Time-Sharing | Multiple users, time slicing | Unix server |
| Distributed | Multiple computers → single system | Google infrastructure |
| Network | Independent PCs sharing resources | Windows Server shares |
| Real-Time (Hard) | Miss deadline = failure | Pacemaker, missile |
| Real-Time (Soft) | Miss deadline = degraded quality | Video streaming |

**iOS vs Android:** iOS = closed source, XNU kernel, Apple only. Android = open source, Linux kernel, many manufacturers.

**Multiprocess Architecture:** Chrome — each tab = separate process. One tab crash ≠ all crash.

---

## Chapter 2 — OS Services & System Calls

### OS Services
**User-facing:** ① User Interface (CLI/GUI) ② Program Execution ③ I/O Operations ④ File System Manipulation ⑤ Communication ⑥ Error Detection

**System-facing:** ⑦ Resource Allocation ⑧ Accounting ⑨ Protection & Security

### CLI vs GUI

| | CLI | GUI | Touch |
|--|-----|-----|-------|
| Input | Typed commands | Mouse+KB | Gestures |
| Expert speed | Fastest | Moderate | Moderate |
| Learning | Steep | Moderate | Easiest |
| Automation | Excellent | Limited | Very limited |

**GUI Characteristics:** WIMP (Windows, Icons, Menus, Pointers), desktop metaphor, drag-and-drop.

---

### System Calls

**Definition:** Programmatic interface for user programs to request kernel services. Controlled gateway from **user mode → kernel mode**.

**API vs System Call:** API = higher-level wrapper. Use APIs for **portability, simplicity, safety**. (e.g., `fopen()` API → `open()` syscall)

**Types with examples:**

| Type | Examples |
|------|---------|
| Process Control | `fork()`, `exit()`, `wait()` |
| File Management | `open()`, `read()`, `write()`, `close()` |
| Device Management | `ioctl()`, `read()` |
| Information | `getpid()`, `time()`, `sleep()` |
| Communication | `pipe()`, `socket()`, `send()` |
| Protection | `chmod()`, `chown()` |

**Implementation flow:**
```
User program → C Library (puts syscall # in register)
  → TRAP instruction (mode switch) → Kernel reads syscall #
  → Looks up System Call Table → Executes kernel function
  → Returns result → Mode switch back to user
```

---

### Microkernel
Only **essentials** in kernel (IPC, basic scheduling, basic memory). Everything else (file system, drivers, networking) runs in **user space** as services communicating via **message passing**.

| Pros | Cons |
|------|------|
| Reliable (driver crash ≠ OS crash) | Slower (message passing overhead) |
| Secure (small attack surface) | More context switches |
| Extensible, Portable | Complex design |

---

### OS Debugging

| Term | Trigger | Scope |
|------|---------|-------|
| **Core Dump** | Application crash | Process memory snapshot |
| **Crash Dump** | OS/Kernel crash (BSOD) | Entire kernel memory snapshot |

---

### System Boot
Power ON → CPU reads **ROM/EEPROM** (bootstrap/BIOS) → POST (hardware test) → Loads **bootloader** from disk → Bootloader loads **kernel** into RAM → Kernel initializes system → Ready!

---

### Spooling
**SPOOL** = Simultaneous Peripheral Operations On-Line. Buffer on disk between fast CPU and slow device (printer). Process sends data to spool → continues immediately. Printer processes from spool at its own pace.

---

## Chapter 3 — Processes

### What is a Process?
**Program in execution.** Program = passive (code on disk). Process = active (executing).

### 5 Components:
```
HIGH  ┌─────────┐
      │  STACK  │ ← Temp: params, return addr, local vars (LIFO)
      ├─────────┤
      │  FREE   │ ← Stack↓ Heap↑ grow toward each other
      ├─────────┤
      │  HEAP   │ ← Dynamic allocation (malloc/new) at runtime
      ├─────────┤
      │  DATA   │ ← Global & static variables
      ├─────────┤
LOW   │  TEXT   │ ← Code (read-only instructions)
      └─────────┘
+ Program Counter → address of current instruction
```

---

### Process States (5 States)

```
         admitted        dispatch
NEW ──────────► READY ◄──────────► RUNNING ───► TERMINATED
                  ▲    interrupt      │
                  │                   │ I/O wait
            I/O done                  ▼
                  └──────── WAITING
```

| State | Meaning |
|-------|---------|
| **New** | Being created |
| **Ready** | In memory, waiting for CPU |
| **Running** | CPU executing it |
| **Waiting** | Blocked on I/O/event |
| **Terminated** | Finished/killed |

**Key rule:** WAITING → READY first (never directly to RUNNING).

---

### PCB (Process Control Block)
Data structure per process containing: **PID, State, PC, Registers, Scheduling info, Memory info, Accounting info, I/O info.**

### Context Switch
```
P0 running → Interrupt → Save P0 state to PCB₀ → Load P1 state from PCB₁ → P1 running
```
**Dispatcher:** Module that performs the switch (save/load state, switch to user mode, jump to correct location).

---

### 3 Schedulers

| Scheduler | Speed | Function |
|-----------|-------|----------|
| **Long-term** (Job) | Slow | Disk → Memory. Controls degree of multiprogramming |
| **Short-term** (CPU) | Fast | Ready Queue → CPU. Runs very frequently |
| **Medium-term** (Swapper) | — | Swap out process from RAM to disk to free memory |

**Swap Out:** Remove less-important process from RAM → disk. Swap In: bring it back later.

---

### Zombie vs Orphan

| | Zombie | Orphan |
|--|--------|--------|
| **What** | Child terminated, parent hasn't called `wait()` | Parent terminated, child still running |
| **Who's dead** | Child (but PCB lingers) | Parent |
| **Fix** | Parent calls `wait()` | Init (PID 1) adopts child |
| **Danger** | PID exhaustion | Not dangerous (init handles) |

---

### IPC (Inter-Process Communication)

| Shared Memory | Message Passing |
|--------------|-----------------|
| Both access common memory region | send()/recv() through kernel |
| Fast, but needs sync (locks) | Slower, but safer |
| Large data, same machine | Small messages, distributed |

---

### Multiprocessor Architecture
Multiple CPUs sharing memory + I/O, single OS.

- **SMP (Symmetric):** All CPUs equal — any can run any task
- **AMP (Asymmetric):** One master CPU, others are slaves

**Benefits:** ↑ Throughput, ↑ Economy, ↑ Reliability (graceful degradation)
