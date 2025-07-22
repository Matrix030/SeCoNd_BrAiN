when an application is suitable for parallel execution, a good implementa-
tion on a GPU can achieve more than 100 times (100) speedup over
sequential execution. If the application includes what we call data parallel-
ism, it is often a simple task to achieve a 10 speedup with just a few hours
of work.

How many times speedup can be expected from parallelizing these super-
application? It depends on the portion of the application that can be paral-
lelized. If the percentage of time spent in the part that can be parallelized is
30%, a 100 speedup of the parallel portion will reduce the execution time
by 29.7%. The speedup for the entire application will be only 1.4. In fact,
even an infinite amount of speedup in the parallel portion can only slash
less 30% off execution time, achieving no more than 1.43 speedup.

On the other hand, if 99% of the execution time is in the parallel portion,
a 100 speedup will reduce the application execution to 1.99% of the
original time. This gives the entire application a 50 speedup; therefore,
it is very important that an application has the vast majority of its execution
in the parallel portion for a massively parallel processor to effectively
speedup its execution.

one must give the CPU a fair chance
to perform and make sure that code is written in such a way that GPUs
complement CPU execution, thus properly exploiting the heterogeneous
parallel computing capabilities of the combined CPU/GPU system. This is
precisely what the CUDA programming model promotes, as we will further
explain in the book.