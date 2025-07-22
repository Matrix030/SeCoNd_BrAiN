Many parallel programming languages and models have been proposed in
the past several decades [Mattson 2004]. The ones that are the most widely
used are the Message Passing Interface (MPI) for scalable cluster com-
puting and OpenMPÔ for shared-memory multiprocessor systems. MPI is
a model where computing nodes in a cluster do not share memory
[MPI 2009]; all data sharing and interaction must be done through explicit
message passing. MPI has been successful in the high-performance scien-
tific computing domain. Applications written in MPI have been known to
run successfully on cluster computing systems with more than 100,000
nodes. The amount of effort required to port an application into MPI, however, can be extremely high due to lack of shared memory across com-
puting nodes. CUDA, on the other hand, provides shared memory for par-
allel execution in the GPU to address this difficulty. As for CPU and
GPU communication, CUDA currently provides very limited shared mem-
ory capability between the CPU and the GPU. Programmers need to man-
age the data transfer between the CPU and GPU in a manner similar to
“one-sided” message passing, a capability whose absence in MPI has been
historically considered as a major weakness of MPI.

CUDA achieves much higher scalability with simple, low-overhead thread management and no cache coherence hardware requirements. As we will see, however, CUDA does not support as wide a range of applications as OpenMP due to these scalability tradeoffs. On the other hand, many superapplications fit well into the simple thread management model of CUDA and thus enjoy the scalability and performance.

More recently, several major industry players, including Apple, Intel,
AMD/ATI, and NVIDIA, have jointly developed a standardized program-
ming model called OpenCLÔ [Khronos 2009]. Similar to CUDA, the
OpenCL programming model defines language extensions and runtime
APIs to allow programmers to manage parallelism and data delivery in
massively parallel processors. OpenCL is a standardized programming
model in that applications developed in OpenCL can run without modi-
fication on all processors that support the OpenCL language extensions
and API.

The level of programming constructs in OpenCL is still at a lower level
than CUDA and much more tedious to use. Also, the speed achieved in
an application expressed in OpenCL is still much lower than in CUDA on the platforms that support both. Because programming massively parallel
processors is motivated by speed