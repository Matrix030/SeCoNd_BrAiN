"if you don’t care about performance parallel programming is very easy"
We will show that with the CUDA programming model that focuses on data parallelism, one can achieve
both high performance and high reliability in their applications.

(1) identifying the part of application pro-
grams to be parallelized, 
(2) isolating the data to be used by the parallelized
code by using an API function to allocate memory on the parallel comput-
ing device, 
(3) using an API function to transfer data to the parallel com-
puting device, 
(4) developing a kernel function that will be executed by
individual threads in the parallelized part, 
(5) launching a kernel function
for execution by parallel threads, and 
(6) eventually transferring the data
back to the host processor with an API function call.