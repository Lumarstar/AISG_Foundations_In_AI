# Logic, Control Flow and Filtering

## Comparison Operators

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

**WARNING: `=` IS THE ASSIGNMENT OPERATOR. DO NOT GET THIS MIXED UP.**

Also since you're here, take a look at some cool stuff:

```python
  # what do you think this yields?
  print(True == 1)

  # how about this?
  print(False == 0)

  # this...?
  # especially this by the way, since `if 2` works but `2 != True` so this is kinda iffy
  print(True == 2)

  # and this...
  print(False == 2)
```

```console
  True
  True
  False
  False
```

These boolean comparisons worked fine because a boolean is a special kind of integer!
`True` corresponds to `1`, and `False` to `0`.
