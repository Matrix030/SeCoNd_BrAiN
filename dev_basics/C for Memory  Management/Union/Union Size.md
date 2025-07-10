A downside of unions is that the size of the union is the size of the _largest_ field in the union. Take this example:

```c
typedef union IntOrErrMessage {
  int data;
  char err[256];
} int_or_err_message_t;
```

This `IntOrErrMessage` union is designed to hold an `int` 99% of the time. However, when the program encounters an error, instead of storing an integer here, it will store an error message. The trouble is that it's incredibly inefficient because it allocates 256 bytes for every `int` that it stores!

Imagine an array of 1000 `int_or_err_message_t` objects. Even if none of them make use of the `.err` field, the array will take up `256 * 1000 = 256,000` bytes of memory! An array of `int`s would have only taken `4,000` bytes (assuming 32-bit integers).

## Quiz Examples

_Assume the following_:

- `sizeof(int) = 4`
- `sizeof(char) = 1`
- `sizeof(long int) = 8`

```c  
union SensorData {
  long int temperature;
  long int humidity;
  long int pressure;
};
```

```c
union PacketPayload {
  char text[256];
  unsigned char binary[256];
  struct ImageData {
    int width;
    int height;
    unsigned char data[1024];
  } image;
};
```

```c
union Item {
  struct {
    int damage;
    int range;
    int size;
  } weapon;
  struct {
    int healingAmount;
    int duration;
  } potion;
  struct {
    int doorID;
  } key;
};
```