---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.8.6.2", "JVM"]
---

# The Java Virtual Machine

1) Java is a popular object-oriented programming language. 
2) In addition to a language specification and a large API library, Java also provides a specification for a Java virtual machine—or JVM.
3) Java objects are specified with the class construct; a Java program consists of one or more classes. For each Java class, the compiler produces an architecture-neutral **bytecode** output (.class) file that will run on any implementation of the JVM.
4) The JVM is a specification for an abstract computer. It consists of a **class loader** and a Java interpreter that executes the architecture-neutral bytecodes, as diagrammed in the figure below. 
5) The class loader loads the compiled .class files from both the Java program and the Java API for execution by the Java interpreter. 
6) After a class is loaded, the verifier checks that the .class file is valid Java bytecode and does not overflow or underflow the stack. 
7) It also ensures that the bytecode does not perform pointer arithmetic, which could provide illegal memory access.
8) If the class passes verification, it is run by the Java interpreter. 
9) The JVM also automatically manages memory by performing garbage collection—the practice of reclaiming memory from objects no longer in use and returning it to the system. 
10) Much research focuses on garbage collection algorithms for increasing the performance of Java programs in the virtual machine.

![[Pasted image 20260601132312.png]]

11) The JVM may be implemented in software on top of a host operating system, such as Windows, Linux, or Mac OS X, or as part of a Web browser. 
12) Alternatively, the JVM may be implemented in hardware on a chip specifically designed to run Java programs. If the JVM is implemented in software, the Java interpreter interprets the bytecode operations one at a time. 
13) A faster software technique is to use a **just-in-time (JIT)** compiler. Here, the first time a Java method is invoked, the bytecodes for the method are turned into native machine language for the host system.
14) These operations are then cached so that subsequent invocations of a method are performed using the native machine instructions and the bytecode operations need not be interpreted all over again. A technique that is potentially even faster is to run the JVM in hardware on a

**THE .NET FRAMEWORK**
1) The .NET Framework is a collection of technologies, including a set of class libraries, and an execution environment that come together to provide a platform for developing software. 
2) This platform allows programs to be written to target the .NET Framework instead of a specific architecture. A program written for the .NET Framework need not worry about the specifics of the hardware or the operating system on which it will run.
3) Thus, any architecture implementing .NET will be able to successfully execute the program. 
4) This is because the execution environment abstracts these details and provides a virtual machine as an intermediary between the executing program and the underlying architecture.
5) At the core of the .NET Framework is the **Common Language Runtime (CLR).**
6) The CLR is the implementation of the .NET virtual machine.
7) It provides an environment for execution of programs written in any of the languages targeted at the .NET Framework. 
8) Programs written in languages such as C# (pronounced _C-sharp_) and VB.NET are compiled into an intermediate, architecture-independent language called Microsoft Intermediate Language (MS-IL). 
9) These compiled files, called assemblies, include MS-IL instructions and metadata. They have file extensions of either .EXE or .DLL. 
10) Upon execution of a program, the CLR loads assemblies into what is known as the **Application Domain**. As instructions are requested by the executing program, the CLR converts the MS-IL instructions inside the assemblies into native code that is specific to the underlying architecture using just-in-time compilation.
11) Once instructions have been converted to native code, they are kept and will continue to run as native code for the CPU. The architecture of the CLR for the .NET framework is shown in the figure below.

![[Pasted image 20260601132327.png]]

## Related

- [[> Examples]] — back to the Examples MOC
- [[VMware]] — the other example
- [[Modules]] — object-oriented modular kernel design
