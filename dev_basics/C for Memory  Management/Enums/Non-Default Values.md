Sometimes, you don't just want to enumerate some _names_ (where the underlying integer constant values don't really matter)... you want to set those enumerations to _specific_ values. For example, you might want to define a program's [exit status codes](https://en.wikipedia.org/wiki/Exit_status):

```c
typedef enum {
  EXIT_SUCCESS = 0,
  EXIT_FAILURE = 1,
  EXIT_COMMAND_NOT_FOUND = 127,
} ExitStatus;
```

Alternatively, you can define the first value and let the compiler fill in the rest (incrementing by 1):

```c
typedef enum {
  LANE_WPM = 200,
  PRIME_WPM, // 201
  TEEJ_WPM,  // 202
} WordsPerMinute;
```