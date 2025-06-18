To illustrate the usefulness of pointers, let's pretend we want to pass a collection of data into a function. Within that function, we want to modify the data. In Python, we could use a class to store the data, and pass an instance of that class into the function:

```python
class Coordinate:
    def __init__(self, x, y, z):
        self.x = x
        self.y = y
        self.z = z

def update_coordinate_x(coord, new_x):
    coord.x = new_x

c = Coordinate(1, 2, 3)
print(c.x)  # 1
update_coordinate_x(c, 4)
print(c.x)  # 4
```


Example:
Now let's do the same thing, but using a struct in C.

**Complete the `coordinate_update_x` and `coordinate_update_and_return_x` functions.**

1. [ ] `coordinate_update_x` should update the `x` field with the provided `new_x` value. It returns `void`.
2. [ ] `coordinate_update_and_return_x` should update the `x` field with the provided `new_x` value, and then return the updated coordinate struct.

Remember, you can access the field of a struct with a `.` operator, like so:

```c
car.tires = 4;
```

## What Happened?

After passing the assignment, open up `main.c` and take a look at the test cases. You'll notice that `coordinate_update_x` doesn't update anything, but `coordinate_update_and_return_x` does because it returns a new copy of the struct.

- In C, structs are passed by _value_. That's why updating a field in the struct does _not_ change the original struct from the `main` function.
- To get the change to "persist", we needed to return the updated struct from the function (a new copy).
- The memory address of the struct that went _in_ to `coordinate_update_and_return_x` was not the same as the address of the struct that was returned. Again, because we created a copy.