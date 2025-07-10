Go programs are fairly lightweight. Each program includes a small amount of extra code that's included in the executable binary called the [Go Runtime](https://go.dev/doc/faq#runtime). One of the purposes of the Go runtime is to clean up unused memory at runtime. It includes a [garbage collector](https://en.wikipedia.org/wiki/Garbage_collection_\(computer_science\)) that automatically frees up memory that's no longer in use.
![[Pasted image 20250616124604.png]]

## Comparison

As a general rule, Java programs use _more_ memory than comparable Go programs. There are several reasons for this, but one of them is that Java uses a virtual machine to interpret bytecode at runtime and typically allocates more on the [heap](https://courses.grainger.illinois.edu/cs225/fa2022/resources/stack-heap/).

On the other hand, Rust and C programs use slightly _less_ memory than Go programs because more control is given to the developer to optimize the memory usage of the program. The Go runtime just handles it for us automatically.

## Idle Memory Usage

![idle memory](https://miro.medium.com/max/1400/1*Ggs-bJxobwZmrbfuoWGpFw.png)

In the chart above, [Dexter Darwich compares the memory usage](https://medium.com/@dexterdarwich/comparison-between-java-go-and-rust-fdb21bd5fb7c) of three _very_ simple programs written in Java, Go, and Rust. As you can see, Go and Rust use _very_ little memory when compared to Java.