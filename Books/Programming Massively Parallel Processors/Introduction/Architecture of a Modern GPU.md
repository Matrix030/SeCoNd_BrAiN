![[Pasted image 20250708001122.png]]
Figure 1.3 shows the architecture of a typical CUDA-capable GPU. It is
organized into an array of highly threaded streaming multiprocessors
(SMs). In Figure 1.3, two SMs form a building block; however, the number
of SMs in a building block can vary from one generation of CUDA GPUs
to another generation. Also, each SM in Figure 1.3 has a number of stream-
ing processors (SPs) that share control logic and instruction cache.

Each GPU currently comes with up to 4 gigabytes of **graphics double data rate
(GDDR) DRAM**, referred to as global memory in Figure 1.3.

These GDDR DRAMs differ from the system DRAMs on the CPU motherboard in that
they are essentially the frame buffer memory that is used for graphics.
For graphics applications, they hold video images, and texture information
for three-dimensional (3D) rendering, but for computing they function
as very-high-bandwidth, off-chip memory, though with somewhat more
latency than typical system memory. For massively parallel applications,
the higher bandwidth makes up for the longer latency.

### 🔸 Original:

> but for computing they function as very-high-bandwidth, off-chip memory, though with somewhat more latency than typical system memory.

### ✅ Explanation:

When a GPU is used for **general-purpose computing** (not graphics), the GDDR memory acts like **external RAM** for the GPU — we call this **off-chip memory** (meaning it's not on the GPU chip itself).

- It has **very high bandwidth** → it can transfer **a lot of data per second**.
    
- But it has **somewhat higher latency** than CPU system memory → the **delay** before data arrives is **a bit longer** than with standard DRAM in your computer.
    

---

### 🔸 Original:

> For massively parallel applications, the higher bandwidth makes up for the longer latency.

### ✅ Explanation:

In **massively parallel workloads** (like running thousands of threads on a GPU), what's more important than latency is **bandwidth** — because you’re moving lots of data in and out **at the same time**.

So even though each memory access **starts slower (higher latency)**, the system can move **huge amounts of data quickly (high bandwidth)** — which is **perfectly fine for parallel computing**, where many threads can wait while others are being serviced.

the future.
The massively parallel G80 chip has 128 SPs (16 SMs, each with 8 SPs).
Each SP has a multiply–add (MAD) unit and an additional multiply unit.
With 128 SPs, that’s a total of over 500 gigaflops. In addition, special-
function units perform floating-point functions such as square root (SQRT),
as well as transcendental functions. With 240 SPs, the GT200 exceeds 1 ter-
flops. Because each SP is massively threaded, it can run thousands of
threads per application. A good application typically runs 5000–12,000
threads simultaneously on this chip. For those who are used to simultaneous
multithreading, note that Intel CPUs support 2 or 4 threads, depending on
the machine model, per core. The G80 chip supports up to 768 threads
per SM, which sums up to about 12,000 threads for this chip.