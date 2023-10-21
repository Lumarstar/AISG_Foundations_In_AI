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

Needless to say too, operands can be complicated expressions, like these:

```python
  2 * my_kitchen < 3 * your_kitchen
```

so do what you desire and go wild! Python knows how to process them yes.

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

## Boolean operators

These operators allow us to combine booleans!

### `and`

Takes two booleans, and returns `True` only if both booleans are True.

| **operation**         | **result** |
|-----------------------|------------|
| `True` `and` `True`   | `True`     |
| `True` `and` `False`  | `False`    |
| `False` `and` `False` | `False`    |

Here's an example of this in action:

```python
  x = 12

  # find out if x is between 5 and 15
  print(x > 5 and x < 15)
```

```console
  True
```

### `or`

This operator just needs at least one of the operands to be `True` to evaluate `True`.

| **operation**        | **result** |
|----------------------|------------|
| `True` `or` `True`   | `True`     |
| `True` `or` `False`  | `True`     |
| `False` `or` `True`  | `True`     |
| `False` `or` `False` | `False`    |

Take a look at a simple example to see this operator applied on some variables:

```python
  y = 5

  # find out if y is less than 7 or larger
  # than 13
  print(y < 7 or y > 13)
```

```console
  True
```

### `not`

This operator is useful to negate results.

| **operation**  | **result** |
|----------------|------------|
| `not` `True`   | `False`    |
| `not` `False`  | `True`     |

*Notice that `not` has a higher priority than `and` and `or`. It is executed first.*

### Combining them together

A short question here for you to see if you get the hang of `and`, `or`, `not`.

```python
  x = 8
  y = 9

  # what would this expression return?
  not(not(x < 3) and not(y > 14 or y > 10))
```

We can solve this by breaking the expression down into its constituent parts.

```python
  # part 1
  not(x < 3) == not(False) == True

  # part 2
  not(y > 14 or y > 10) == not(False) == True
```

So this now simplifies down to:

```python
  not(True and True) == not(True) == False
```

Thus, the original expression evaluates to `False`.

### NumPy Array operations

If we just simply use these operators on NumPy arrays, they return ValueErrors.

Assume `bmi` is a NumPy array.

```python
  # we just want to find BMIs between 21 and 22
  bmi > 21 and bmi < 22
```

```console
  ValueError: The truth value of an array with more than one element is ambiguous.
  Use a.any() or a.all()
```

Instead of using the normal boolean operators, we now use the following functions instead:

| **numpy operation** | **meaning** |
|---------------------|-------------|
| `np.logical_and()`  | `and`       |
| `np.logical_or()`   | `or`        |
| `np.logical_not()`  | `not`       |

```python
  np.logical_and(bmi < 22, bmi > 21)
```

```console
  array([True, False, True, False, True], dtype=bool)
```

Now the logical operators work as expected - they are applied on each element of the array!

## Conditional Statements

Conditional statements allow us to control the flow of our code.

### `if`

The `if` statement is applied this way:

```python
  if condition:
    expression
```

If `condition` evaluates to `True`, then the `expression` within the code block will be executed.

After `condition`, a colon `:` is used to denote a code block, and `expression`, being within the code block, will have to be indented with tabs or spaces.

To exit the `if` statement, simply write code without the indentation to show that subsequent code is not part of the `if` code block.

For instance, let's say we have the following program:

```python
  z = 4

  if z % 2 == 0:
    # there can be more than one expression within the code block
    print('hello!')
    print('z is even')

  print('done!')    # outside of the code block
```

```console
  hello!
  z is even
  done!
```

Now, if the `condition` evaluates to `False`, then the `expression` within the `if` code block will not be executed.

Watch what happens when `z = 5`:

```python
  z = 5

  if z % 2 == 0:
    print('hello!')
    print('z is even')

  print('done!')
```

```console
  done!
```

### `else`

### `elif`