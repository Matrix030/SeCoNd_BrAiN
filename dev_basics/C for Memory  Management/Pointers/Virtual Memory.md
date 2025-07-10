As it turns out, your code probably doesn't have direct access to the physical RAM in your computer.

Instead, your operating system provides a layer of abstraction called **virtual memory**. Virtual memory makes it seem like your program has direct access to all the memory on the machine, even if it doesn't.

- **Physical Memory**: The actual RAM sticks in your computer.
- **Operating System**: The software that manages access to the physical memory.
- **Your Program**: When it runs, it becomes a [`process`](https://en.wikipedia.org/wiki/Process_\(computing\)) and is given access to a chunk of virtual memory by the operating system.
- **Virtual Memory**: This abstracted chunk of memory that your program can use.

_There are exceptions to this, for example if you're using C to build embedded firmware that runs without an operating system, your code might interact directly with physical memory._

![virtual memory diagram](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/nXwUFUE.png)

By only giving processes access to a chunk of virtual memory, the operating system can do some cool things:

1. **Isolation**: One process can't access the memory of another process.
2. **Security**: The operating system can prevent processes from accessing certain parts of memory.
3. **Simplicity**: Developers don't have to worry about managing physical memory and the memory of other processes.
4. **Performance**: The operating system can optimize memory access depending on the hardware and needs of the program. For example, by moving data between physical memory and the hard drive.

At the end of the day, your program has direct access to a virtual chunk of memory. Just like physical memory, it can be thought of as a big array of bytes, where each byte has an address.