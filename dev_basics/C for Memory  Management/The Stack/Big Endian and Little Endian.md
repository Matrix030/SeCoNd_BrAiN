While we are on the topic of memory, it's worth knowing about "endianness". Endianness is the order in which bytes are stored in memory. The two most common formats are big endian and little endian.

## Big Endian

In a big-endian system, the most significant byte is stored first, at the lowest memory address. The "most significant byte" is just a fancy way of saying "the biggest part of the number".

Let’s say you have the hexadecimal number `0x12345678`. Here’s how it would be stored in big-endian format:

![big endian](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/1tXaE0D.png)  
The most significant byte (`0x12`) is stored at the lowest memory address.

## Little Endian

In a little-endian system, the least significant byte (the "smallest" part of the number) is stored first, at the lowest memory address. This is the format used by most modern computers.

Using the same number `0x12345678`, here’s how it would be stored in little-endian format:

![little endian](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/ZSZ1rxc.png)  
Here, the least significant byte (`0x78`) is stored first.

For the most part, you won’t have to worry about endianness when writing programs. The way data is read from memory automatically handles this, so we can spend our valuable time building e-commerce shops for the terminal instead. Endianness becomes important in certain scenarios, like networking and working with binary files.

For now, just know that most modern systems use little-endian, and the compiler takes care of how data is stored and accessed.