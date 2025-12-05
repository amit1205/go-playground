# Overview

The Go runtime is the integral software stack that is statically linked into every Go compiled executable binary. It is essentially an operating system for your Go program, responsible for managing the low-level functions that the Go language abstracts away from the developer.


It is a crucial component that makes Go a highly efficient, concurrent, and garbage-collected language.

# Key Responsibilities of the Go Runtime

The runtime handles several complex tasks to ensure your Go program runs smoothly and performs well:

## 1. Concurrency Management (The Scheduler)

The runtime's scheduler is arguably its most important component. It manages the execution of goroutines (Go's lightweight concurrency units) on a smaller number of operating system (OS) threads.

- Goroutines (G): These are extremely cheap, user-space tasks managed by the runtime. They start with a small stack (typically 2KB) that can grow or shrink dynamically. You can easily run millions of goroutines.


- The M-P-G Model: The scheduler uses the M-P-G model to coordinate work:

  - G (Goroutine): The actual task to run.

  - M (Machine): The OS thread that executes the code. The OS kernel schedules the M's.

  - P (Processor): A logical processor that acts as a local scheduler. It holds a local queue of runnable G's and ensures an M is always executing them. The number of P's is typically equal to the number of available CPU cores.


- Work-Stealing: If an M/P runs out of local goroutines to execute, it will steal a portion of the workload from another P's queue to keep all CPU cores busy and maximize parallelism.

## 2. Memory Management (Garbage Collection)

The runtime includes a highly optimized, concurrent Garbage Collector (GC).

- Automatic Cleanup: The GC automatically identifies and reclaims memory that is no longer being used by the program.

- Low Latency: Go's GC is designed to be non-intrusive, using a concurrent mark-and-sweep process that runs alongside the application. This results in very short "stop-the-world" pause times, which is essential for low-latency web services.

## 3. I/O and System Calls


The runtime handles blocking I/O (Input/Output) efficiently.

- Non-Blocking Behavior: When a goroutine makes a system call that would otherwise block the entire OS thread (like waiting for a network response), the Go runtime detaches the P from the blocking M. It then assigns that P to a new, available M, allowing the P to continue executing other runnable goroutines from its queue. This prevents a single slow I/O operation from stalling execution on a CPU core.


The Go runtime's core function is to handle the complexities of concurrency, memory, and OS interaction, allowing developers to write high-performance, concurrent code using the simple go keyword.

Watch the video for more details : [Understanding the Go runtime](https://www.youtube.com/watch?v=YpRNFNFaLGY)
