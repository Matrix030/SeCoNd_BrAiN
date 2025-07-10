In C, the "size" (in memory) of a type is not guaranteed to be the same on all systems. That's because the size of a type is dependent on the system's architecture. For example, on a 32-bit system, the size of an `int` is usually 4 bytes, while on a 64-bit system, the size of an `int` is usually 8 bytes - of course, you never know until you run `sizeof` with the compiler you plan on using.

However, some types are always guaranteed to be the same. Here’s a list of the basic C data types along with their typical sizes in bytes. Note that sizes can vary based on the platform (e.g., 32-bit vs. 64-bit systems):

### Basic C Types and Sizes:

- **`char`**
    - Size: **1 byte**
    - Represents: Single character.
    - Notes: Always 1 byte, but can be signed or unsigned.
- **`float`**
    - Size: **4 bytes**
    - Represents: Single-precision floating-point number.
- **`double`**
    - Size: **8 bytes**
    - Represents: Double-precision floating-point number.

The actual sizes of these types can be determined using the `sizeof` operator in C for a specific platform, which we'll learn about next.