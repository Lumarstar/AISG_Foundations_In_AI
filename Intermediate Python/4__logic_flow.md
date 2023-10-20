# Logic, Control Flow and Filtering

## Comparison Operators

### The "normal" stuff

Comparison operators are operators that can tell how two Python values relate, and result
in a boolean.

Obviously, they work with numbers, meaning basic stuff like

```python
  3 > 5
  2 == 2
```

```console
  False
  True
```

follow the Laws of Mathematics.

They work with strings too! For strings, they compare character by character, from
left to right, and see which one starts first in alphabetical order.

```python
  # first characters are compared, and 'a' < 'b'
  "adam" < "ben"

  # first characters are the same, so we move on to the second character
  # 'd' < 'l', so 'adam' < 'alex'
  "adam" < "alex"
```

```console
  True
  True
```

However, you can't compare two items of different types. Doing so yields a TypeError.
*(Except between floats and integers. They work mathematically.)*

```python
  3 < 'chris'
```

```console
  TypeError: unorderable types: int() < str()
```

*(Another rule breaker is NumPy arrays, since you can directly apply comparison operators
between NumPy arrays and integers, floats, etc.)*

Here is a summary of all comparison operators.

| **comparator** | **meaning**              |
|----------------|--------------------------|
| `<`            | strictly less than       |
| `<=`           | less than or equal to    |
| `>`            | strictly greater than    |
| `>=`           | greater than or equal to |
| `==`           | equal                    |
| `!=`           | not equal                |

1. **WARNING: `=` IS THE ASSIGNMENT OPERATOR. DO NOT GET THIS MIXED UP.**
2. **SECOND WARNING: `=<` and `=>` are not valid syntaxes.**

Also since you're here, take a look at some cool stuff:

```python
  # what do you think this yields?
  print(True == 1)

  # how about this?
  print(False == 0)

  # this?
  print(True > False)

  # this...?
  # especially this by the way, since `if 2` works but `2 != True` so this is kinda iffy
  print(True == 2)

  # and this...
  print(False == 2)
```

```console
  True
  True
  True
  False
  False
```

These boolean comparisons worked fine because a boolean is a special kind of integer!
`True` corresponds to `1`, and `False` to `0`.

### Here comes the NumPy arrays...

As we already know, we can apply comparison operators on NumPy arrays too.

1. If we apply a comparison operator between a `np.array` and an `int` or `float`,
each of the elements in `np.array` will be compared individually with the provided
`int` or `float`. A boolean array will be returned.

For example,

```python
  import numpy as np
  my_array = np.array([0, 1, 2, 3, 4, 5])
  print(my_array < 3)
```

```console
  [True True True False False False]
```

2. If we apply a comparison operator between two `np.array`, their elements at the same
index will be compared. This means the two arrays must be of the same dimensions. A
boolean array will be returned.

Again, for example,

```python
  import numpy as np
  my_array = np.array([0, 1, 2, 3, 4, 5])
  other_array = np.array([5, 4, 3, 2, 1, 0])
  print(my_array >= other_array)
```

```console
  [False False False True True True]
```
