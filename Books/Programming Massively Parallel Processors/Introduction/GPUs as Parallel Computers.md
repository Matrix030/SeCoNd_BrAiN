The multicore trajectory seeks to maintain the execution speed of sequential programs while moving into multiple cores.

In contrast, the many-core trajectory focuses more on the execution
throughput of parallel applications. The many-cores began as a large number of much smaller cores, and, once again, the number of cores doubles with each generation.

### **Original:**

> A current exemplar is the NVIDIA GeForce GTX 280 graphics processing unit (GPU) with 240 cores, each of which is a heavily multithreaded, in-order, single-instruction issue processor that shares its control and instruction cache with seven other cores.

### **Explanation:**

Let’s break that down:

- **GTX 280 GPU** has **240 cores** (called CUDA cores).
    
- Each core is:
    
    - **Heavily multithreaded**: It can manage **many threads** (lightweight processes) at once.
        
    - **In-order**: Instructions are executed **in the order they appear**, unlike out-of-order CPUs that rearrange instructions for performance.
        
    - **Single-instruction issue**: Each core can **issue only one instruction per cycle**, making it simple.
        
    - **Shared control/instruction cache**: One **cache and control unit is shared among 8 cores** (i.e., each core shares it with 7 others).
        

This is a **cost-efficient design** that works well when you have lots of data to process in parallel.

### **Summary (in simple terms):**

Many-core chips like GPUs are built for **maximum parallel work**, not speed-per-task. Instead of a few powerful cores like CPUs, they use **hundreds of smaller, simpler cores**. For example, NVIDIA’s GTX 280 has **240 such cores**, each good at running **many threads** but only **one instruction at a time**, and each group of 8 cores **shares some parts** (like control logic and instruction memory) to save space. These designs make GPUs great at **handling massive numerical workloads**, which is why they’ve been top performers in floating-point math for over a decade.
![[Pasted image 20250707234141.png]]


One might ask why there is such a large performance gap between
many-core GPUs and general-purpose multicore CPUs. The answer lies in the differences in the fundamental design philosophies between the two types of processors, as illustrated in Figure 1.2. The design of a CPU is optimized for sequential code performance. It makes use of sophisticated control logic to allow instructions from a single thread of execution to execute in parallel or even out of their sequential order while maintaining the appearance of sequential execution. More importantly, large cache memories are provided to reduce the instruction and data access latencies of large complex applications. Neither control logic nor cache memories contribute to the peak calculation speed.
![[Pasted image 20250707234358.png]]
Because of frame buffer requirements and the relaxed memory model—the way various system software, applications, and input/output (I/O) devices expect their memory accesses to work—general-purpose processors have to satisfy requirements from legacy operating systems, applications, and I/O devices that make memory bandwidth more difficult to increase. In contrast, with simpler memory models and fewer legacy constraints, the GPU designers can more easily achieve higher memory bandwidth.

> This demand motivates the GPU vendors to look for ways to maximize the chip area and power budget dedicated to floating-point calculations. The prevailing solution to date is to optimize for the execution throughput of massive numbers of threads. The hardware takes advantage of a large number of execution threads to find work to do when some of them are waiting for long-latency memory accesses, thus minimizing the control logic required for each execution thread. Small cache memories are provided to help control the bandwidth requirements of these applications so multiple threads that access the same memory data do not need to all go to the DRAM. As a result, much more chip area is dedicated to the floating-point calculations.

### 🔸 **Original:**

> This demand motivates the GPU vendors to look for ways to maximize the chip area and power budget dedicated to floating-point calculations.

### ✅ **Explanation:**

GPUs are in high demand for tasks that do lots of **floating-point math** (like graphics, simulations, machine learning). So GPU designers (like NVIDIA) want to make sure **as much of the chip as possible** (in terms of **physical space** and **power usage**) is focused on **actual number crunching**, rather than things like control or logic circuits.

---

### 🔸 **Original:**

> The prevailing solution to date is to optimize for the execution throughput of massive numbers of threads.

### ✅ **Explanation:**

The best way found so far to do this is to **run a huge number of threads in parallel** — not caring much about the speed of individual threads, but rather how much total work the GPU can do per second (high **throughput**).

---

### 🔸 **Original:**

> The hardware takes advantage of a large number of execution threads to find work to do when some of them are waiting for long-latency memory accesses...

### ✅ **Explanation:**

GPUs run **thousands of threads** at once. If some threads are **waiting for memory** (which can take a long time, like hundreds of clock cycles), the GPU just **switches to other threads** that are ready to run. This way, the chip is always busy doing something useful — it's **hiding memory latency** by keeping lots of threads active.

---

### 🔸 **Original:**

> ...thus minimizing the control logic required for each execution thread.

### ✅ **Explanation:**

Because each thread is **simple** and the switching is managed efficiently, you don’t need a lot of **control hardware** per thread. This keeps things small and saves space.

---

### 🔸 **Original:**

> Small cache memories are provided to help control the bandwidth requirements of these applications so multiple threads that access the same memory data do not need to all go to the DRAM.

### ✅ **Explanation:**

GPUs use **small caches** to reduce how often they need to access the main memory (DRAM). If many threads need the **same data**, the GPU **stores it in a nearby cache** so it doesn’t fetch it from DRAM every time — saving **bandwidth** and **speeding things up**.

---

### 🔸 **Original:**

> As a result, much more chip area is dedicated to the floating-point calculations.

### ✅ **Explanation:**

By keeping control logic small and minimizing memory traffic with smart caching, GPUs can use **more of the chip space for actual computation**, especially **floating-point units (FPUs)** — the parts that do the heavy math.

Until 2006, graphics chips were very difficult to use because programmers
had to use the equivalent of graphic application programming interface
(API) functions to access the processor cores, meaning that OpenGLÒ or
Direct3DÒ techniques were needed to program these chips. This technique
was called GPGPU, short for general-purpose programming using a graphics
processing unit. Even with a higher level programming environment, the
underlying code is still limited by the APIs. These APIs limit the kinds
of applications that one can actually write for these chips. That’s why only
a few people could master the skills necessary to use these chips to achieve
performance for a limited number of applications; consequently, it did not
become a widespread programming phenomenon. Nonetheless, this technol-
ogy was sufficiently exciting to inspire some heroic efforts and excellent
results.