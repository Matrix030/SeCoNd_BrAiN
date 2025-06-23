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
