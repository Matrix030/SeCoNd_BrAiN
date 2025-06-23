If we create an array of structs it gets crazy because we can access and manipulate the elements using either indexing _or_ pointer arithmetic. Let's see how multi-byte width structures are managed in memory.

First, let's say we're working with our familiar Coordinate struct:

```c
typedef struct Coordinate {
  int x;
  int y;
  int z;
} coordinate_t;
```

We can declare an array of 3 `Coordinate` structs like so:
```c
coordinate_t points[3] = {
  {1, 2, 3},
  {4, 5, 6},
  {7, 8, 9}
};
```

Then we can print out the values of the second element in the array:

```c
printf("points[1].x = %d, points[1].y = %d, points[1].z = %d\n",
  points[1].x, points[1].y, points[1].z
);
// points[1].x = 4, points[1].y = 5, points[1].z = 6
```

Or we can use a pointer:

```c
coordinate_t *ptr = points;
printf("ptr[1].x = %d, ptr[1].y = %d, ptr[1].z = %d\n",
  (ptr + 1)->x, (ptr + 1)->y, (ptr + 1)->z
);
// ptr[1].x = 4, ptr[1].y = 5, ptr[1].z = 6
```

## Memory Layout

Assuming each `int` is 4 bytes, the `Coordinate` structure will be 12 bytes (`3 * 4` bytes). Let's assume the `points` array starts at memory address `0x2000`.

Here is the memory layout:

|Address|Element|Value|Offset (bytes)|
|---|---|---|---|
|`0x2000`|`points[0].x`|1|0|
|`0x2004`|`points[0].y`|2|4|
|`0x2008`|`points[0].z`|3|8|
|`0x200C`|`points[1].x`|4|12|
|`0x2010`|`points[1].y`|5|16|
|`0x2014`|`points[1].z`|6|20|
|`0x2018`|`points[2].x`|7|24|
|`0x201C`|`points[2].y`|8|28|
|`0x2020`|`points[2].z`|9|32|

## Accessing Elements Using Pointers

- `points + 0` or `&points[0]` points to `0x2000`
- `points + 1` or `&points[1]` points to `0x200C` (next structure, offset by 12 bytes)
- `points + 2` or `&points[2]` points to `0x2018`