# CS Fundamentals Cheatsheet

Core computer science terms covering processes, concurrency, memory, garbage collection, input and output, data encoding and hashing.

> Related: [data-structures-and-algorithms.md](data-structures-and-algorithms.md) · [programming-languages.md](programming-languages.md) · [networking-and-web.md](networking-and-web.md) · [linux-and-git-commands.md](linux-and-git-commands.md) · [CS fundamentals topic](../topics/13-cs-fundamentals/README.md) · [Debugging topic](../topics/12-debugging-troubleshooting/README.md)

## Processes and threads

| Term | What it means |
|---|---|
| **Process** | A running program with its own address space and resources. Isolation protects processes from each other, at higher creation and communication cost. |
| **Thread** | Unit of execution within a process, sharing its memory. Cheap communication, but shared state requires synchronisation. |
| **Kernel thread** | Thread scheduled by the operating system. Runs in parallel across cores, but costs more memory and switching time. |
| **User-space thread** | Thread scheduled by a language runtime, such as a goroutine. Very cheap, allowing huge numbers multiplexed over fewer kernel threads. |
| **Coroutine** | Function that can pause and resume, cooperatively yielding control. Enables lightweight concurrency for asynchronous code. |
| **Context switch** | Saving one execution's state and loading another's. Direct cost is small, but cache disruption hurts at high rates. |
| **PCB** (Process Control Block) | Kernel record of a process's state, registers and open resources. Allows the process to be paused and resumed. |
| **System call** | Controlled request from user code to the kernel, such as `read`. Slower than a normal function call because of the mode switch. |
| **Kernel and user space** | Privilege levels separating the operating system from applications. Stops a faulty application directly damaging the system. |
| **`fork`** | Creates a child process, sharing memory copy-on-write. Often followed by `exec`, which replaces the process with a new program. |
| **Zombie process** | A finished child whose parent has not collected its exit status. Uses a process table entry until reaped. |
| **IPC** (Inter-Process Communication) | Ways for processes to exchange data: pipes, sockets, shared memory, signals. Choice trades speed against simplicity and isolation. |

| Process vs thread | Process | Thread |
|---|---|---|
| Memory | Separate address space | Shared within the process |
| Failure impact | Usually contained | Can crash the whole process |
| Creation and switching | Heavier | Lighter |
| Communication | IPC | Shared memory, with locks |

## Concurrency primitives

| Term | What it means |
|---|---|
| **Concurrency vs parallelism** | Concurrency structures multiple tasks with overlapping progress; parallelism runs them simultaneously. One core can be concurrent but not parallel. |
| **Race condition** | Outcome depends on unpredictable timing. Causes intermittent bugs that testing often misses. |
| **Data race** | Unsynchronised threads access the same memory, with at least one writing. Undefined or unsafe behaviour in many languages. |
| **Critical section** | Code accessing shared state that only one thread should run at a time. Keep it short to reduce contention. |
| **Mutex** (Mutual Exclusion lock) | Lock owned by one thread at a time. The default tool for protecting shared data. |
| **Semaphore** | Counter limiting how many threads proceed. Suits bounding access to a pool of resources. |
| **Spinlock** | Lock that busy-waits rather than sleeping. Useful only for very short waits, as it wastes CPU otherwise. |
| **Read-write lock** | Allows many concurrent readers or one writer. Helps read-heavy workloads, but overhead can outweigh benefits under short critical sections. |
| **Condition variable** | Lets a thread sleep, releasing a lock, until signalled. Always recheck the condition in a loop because of spurious wake-ups. |
| **Monitor** | Combines a lock and condition waiting within an object. The model behind Java's `synchronized`, `wait` and `notify`. |
| **Atomic operation** | Indivisible operation that other threads cannot see half done. Enables fast counters and flags without locks. |
| **CAS** (Compare-And-Swap) | Writes a new value only if the current value matches the expected one. The foundation of lock-free algorithms. |
| **Memory barrier** | Instruction limiting reordering of memory operations. Needed because compilers and CPUs reorder for performance. |
| **False sharing** | Threads update separate variables on the same cache line, causing cores to fight over it. Slows code invisibly. |

| Primitive | Holders | Waiting style | Best for |
|---|---|---|---|
| Mutex | One | Sleeps | Protecting shared data |
| Semaphore | N | Sleeps | Limiting concurrency |
| Spinlock | One | Busy-waits | Tiny critical sections |
| Read-write lock | Many readers or one writer | Sleeps | Read-heavy data |
| Atomic | Not applicable | None | Single counters and flags |

## Deadlock

| Term | What it means |
|---|---|
| **Deadlock** | Threads wait forever on resources held by each other. Progress stops until something intervenes. |
| **Coffman conditions** | Mutual exclusion, hold and wait, no preemption, circular wait. All four are needed, so breaking one prevents deadlock. |
| **Lock ordering** | Always acquire locks in one global order. Eliminates circular wait, making it the most common prevention. |
| **Livelock** | Threads keep reacting to each other but make no progress. Random backoff usually resolves it. |
| **Starvation** | A thread never gets the resources it needs while others proceed. Fair scheduling or ageing addresses it. |
| **Priority inversion** | A high-priority task waits on a low-priority lock holder that medium-priority work preempts. Priority inheritance fixes it. |

## Memory

| Term | What it means |
|---|---|
| **Stack memory** | Per-thread region for function frames and local variables. Allocation is very fast and freed automatically on return. |
| **Heap memory** | Shared region for dynamically allocated data. Flexible lifetimes, but slower and subject to fragmentation or garbage collection. |
| **Memory leak** | Memory that is no longer needed but is never released. Garbage collected languages still leak through lingering references. |
| **Dangling pointer** | Pointer to memory already freed. Using it causes corruption and security bugs. |
| **Buffer overflow** | Writing beyond a buffer's bounds. Corrupts neighbouring memory and is a classic security vulnerability. |
| **Stack overflow** | The stack limit is exceeded, usually by deep or unbounded recursion. Convert to iteration or reduce depth. |
| **Fragmentation** | Free memory is split into pieces too small to use. Causes allocation failures or wasted memory despite free space. |
| **CPU cache** (Central Processing Unit) | Small, fast memory levels (L1, L2, L3) between core and main memory. Access patterns often matter more than Big-O constants. |
| **Cache locality** | Accessing data close in time or address runs faster. Explains why arrays usually beat linked lists in practice. |
| **NUMA** (Non-Uniform Memory Access) | Multi-socket machines where memory near a CPU is faster than distant memory. Matters for tuning large databases and JVMs. |

| Stack vs heap | Stack | Heap |
|---|---|---|
| Allocation | Automatic, very fast | Explicit or runtime-managed, slower |
| Lifetime | Tied to function scope | Until freed or collected |
| Scope | Per thread | Shared across threads |
| Typical failure | Stack overflow | Leak, fragmentation, out of memory |

## Virtual memory

| Term | What it means |
|---|---|
| **Virtual memory** | Gives each process its own address space mapped to physical memory. Provides isolation and lets programs exceed physical RAM. |
| **Page** | Fixed-size memory unit, commonly 4 KB. The granularity at which virtual memory is mapped and protected. |
| **Page table** | Maps virtual pages to physical frames. The kernel maintains it, and hardware walks it during translation. |
| **MMU** (Memory Management Unit) | CPU hardware that translates virtual addresses and enforces permissions on each access. |
| **TLB** (Translation Lookaside Buffer) | Small cache of recent address translations. Misses add latency, and huge pages help large-memory workloads. |
| **Page fault** | Access to a page not currently mapped in memory. Minor faults are cheap, while major ones read from disk. |
| **Swap** | Disk space used to hold evicted memory pages. Prevents some crashes, but heavy use severely degrades latency. |
| **Thrashing** | System spends more time paging than working, because memory demand exceeds what is available. Throughput collapses. |
| **`mmap`** (Memory Map) | Maps a file or anonymous memory into an address space. File reads then avoid explicit `read` calls. |
| **Copy-on-write** | Pages are shared until one side writes, then copied. Makes `fork` and snapshots fast and memory-efficient. |
| **Segmentation fault** | Process touched memory it is not permitted to access. Usually indicates a pointer bug. |
| **OOM killer** (Out Of Memory) | Linux mechanism that kills a process when memory runs out. Explains sudden process deaths and exit code 137. |

## Garbage collection

| Term | What it means |
|---|---|
| **GC** (Garbage Collection) | Automatically reclaims unreachable memory. Prevents many memory bugs, but adds CPU overhead and possible pauses. |
| **Reference counting** | Frees an object when its reference count reaches zero. Predictable timing, but cycles need extra handling. |
| **Mark-sweep** | Traces reachable objects from roots, then frees the rest. Handles cycles, but can leave memory fragmented. |
| **Mark-compact** | Marks live objects, then slides them together. Removes fragmentation, at the cost of moving objects. |
| **Copying collector** | Copies live objects to a new region and discards the old one. Cost scales with live data, which suits short-lived objects. |
| **Generational GC** (Garbage Collection) | Splits objects by age, collecting young ones often. Works because most objects die young. |
| **Stop-the-world pause** | All application threads halt for a GC phase. Directly creates latency spikes, so collectors aim to shorten it. |
| **Write barrier** | Small code run on reference writes to inform the collector. Enables generational and concurrent collection, with slight overhead. |

## I/O models

| Term | What it means |
|---|---|
| **File descriptor** | Integer handle for an open file, socket or pipe in Unix-like systems. Exhausting the limit causes "too many open files" errors. |
| **Blocking I/O** (Input/Output) | The call waits until the operation completes. Simple code, but typically needs one thread per connection. |
| **Non-blocking I/O** (Input/Output) | The call returns immediately if it cannot proceed. Lets one thread manage many connections with readiness polling. |
| **I/O multiplexing** (Input/Output) | One thread waits on many descriptors for readiness, using `select`, `poll` or `epoll`. The basis of high-concurrency servers. |
| **`epoll`** (Event Poll) | Linux readiness mechanism that scales to many connections. Avoids rescanning every descriptor as `select` and `poll` do. |
| **`kqueue`** (Kernel Queue) | BSD and macOS event notification system, broadly comparable to `epoll`. |
| **IOCP** (Input/Output Completion Ports) | Windows completion-based mechanism. The kernel performs I/O and notifies the application when it is finished. |
| **`io_uring`** | Linux interface using shared ring buffers for submitting and completing I/O. Reduces system calls and supports true asynchronous file operations. |
| **Reactor and proactor** | Reactor reports readiness and the app performs I/O; proactor reports completion after the system performs it. Two event-driven server designs. |
| **Zero-copy** | Moves data without copying through user space, as with `sendfile`. Cuts CPU use and speeds file serving. |
| **Page cache** | Kernel's in-memory cache of file data. Makes repeat reads fast, but writes are not durable until flushed with `fsync`. |

| Model | Application waits for | Scales to many connections | Typical API |
|---|---|---|---|
| Blocking | Completion | Poorly (one thread each) | `read` |
| Non-blocking plus multiplexing | Readiness | Well | `epoll`, `kqueue` |
| Asynchronous completion | Completion notification | Well | IOCP, `io_uring` |

## Event loop

| Term | What it means |
|---|---|
| **Event loop** | Single thread repeatedly takes ready events and runs their callbacks. Handles many I/O-bound connections without one thread each. |
| **Task queue** | Holds callbacks waiting to run on the loop. A long queue shows up directly as request latency. |
| **Blocking the loop** | CPU-heavy or synchronous work prevents all other events running. Move such work to worker threads or processes. |
| **Cooperative multitasking** | Tasks run until they voluntarily yield. Low overhead, but one misbehaving task can hold everything up. |

## Bits and encoding

| Term | What it means |
|---|---|
| **Two's complement** | Standard signed integer form, where negating means inverting bits and adding one. Range for n bits is -2^(n-1) to 2^(n-1)-1. |
| **Integer overflow** | A result exceeds the type's range. Can wrap silently, leading to logic and security bugs. |
| **Endianness** | Byte order of multi-byte values. Most CPUs are little-endian; network byte order is big-endian. |
| **XOR** (Exclusive Or) | Outputs 1 when bits differ, and `x ^ x` equals 0. Used for parity, simple toggling and finding unpaired values. |
| **Floating point (IEEE 754)** (Institute of Electrical and Electronics Engineers standard 754) | Binary approximation of real numbers, so `0.1 + 0.2` is not exactly `0.3`. Use decimals or integers for money. |
| **ASCII** (American Standard Code for Information Interchange) | 7-bit character set for English text and control characters. The compatible base of UTF-8. |
| **Unicode** | Universal character catalogue assigning code points to characters. Separate from how those points are stored as bytes. |
| **UTF-8** (Unicode Transformation Format, 8-bit) | Variable-length encoding using 1–4 bytes per code point. Backward compatible with ASCII and the web's default. |
| **UTF-16** (Unicode Transformation Format, 16-bit) | Uses 2 or 4 bytes per code point. Used internally by JavaScript and Java strings, so some characters count as length 2. |
| **Base64** | Represents binary as 64 text characters, about 33% larger. An encoding for text-only channels, not encryption. |

```text
x & (x - 1)   clears the lowest set bit
x & -x        isolates the lowest set bit
x & 1         checks for an odd number
1 << n        equals 2^n
x ^ x         equals 0
```

## Hashing

| Term | What it means |
|---|---|
| **Hash function** | Maps input to a fixed-size value deterministically. Enables fast lookups, sharding, checksums and integrity checks. |
| **Hash collision** | Different inputs produce the same hash. Unavoidable, so structures and protocols must handle it. |
| **Chaining** | Each bucket holds a list of colliding entries. Simple, and tolerates high load, but follows pointers. |
| **Open addressing** | Colliding entries probe for another slot in the table. Cache-friendly, but sensitive to high load and deletions. |
| **Load factor** | Entries divided by buckets. Passing a threshold triggers resizing and rehashing, keeping average lookups fast. |
| **Cryptographic hash** | Hash designed to resist finding collisions or inputs, such as SHA-256. MD5 and SHA-1 are broken for security use. |
| **Password hashing** | Deliberately slow, salted hashing with bcrypt, scrypt or Argon2. Slows brute force if password data leaks. |
| **Salt** | Random per-password value stored and hashed with it. Defeats precomputed tables and hides identical passwords. |
| **HMAC** (Hash-based Message Authentication Code) | Hash combined with a secret key. Verifies a message's integrity and authenticity, such as webhook signatures. |
| **CRC** (Cyclic Redundancy Check) | Fast checksum for detecting accidental corruption. Provides no protection against deliberate tampering. |

| Hash type | Goal | Examples | Use for |
|---|---|---|---|
| Non-cryptographic | Speed, even spread | MurmurHash, xxHash | Hash tables, sharding |
| Cryptographic | Resist deliberate attacks | SHA-256, SHA-3 | Integrity, signatures |
| Password | Deliberately expensive | Argon2, bcrypt | Storing passwords |
| Checksum | Detect accidental errors | CRC32 | Storage, network frames |
