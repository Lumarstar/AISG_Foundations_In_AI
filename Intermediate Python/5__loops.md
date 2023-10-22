# Loops

## `while` loop

Similar to the `if` statement, it executes the code inside if `condition` evaluates to `True`. However, for the `if` statement, Python only runs the statements once, and moves on. For the `while` loop, the code will be executed over and over again until `condition` evaluates `False`.

This means the `while` loop "repeats an action until a condition is met".

We follow this format:

```python
  while condition:
      expression
```

For example, say we have `error = 50.0`, and we divide `error` by 4 at every run, until `error` no longer exceeds 1.

```python
  error = 50.0

  while error > 1:
      error /= 4
      print(error)

  print('done!')
```

Let's take a look at a step by step analysis of what happens.

1. Initially, `error = 50.0`, so the condition `error > 1` is `True`. Thus, the statements in the `while` loop will be executed.
2. `error` will become 12.5, and is printed out.
3. Again, the condition is `True` and the code will be executed.
4. `error` becomes 3.125 and is printed out.
5. The condition is still `True`, so the code will be executed (again.)
6. `error` becomes 0.78125, and is printed out.
7. The condition finally evaluates to `False`, and Python will exit the `while` loop and moves on.
8. `'done!`' will be printed.

```console
  12.5
  3.125
  0.78125
  done!
```

Notice that the updating of our condition is extremely important in a `while` loop. If the condition is not updated, then we will have an "infinite loop" situation, where the `while` loop will keep being executed, and Python will crash.

Say we did not update the value of `error`:

```python
  error = 50.0

  while error > 1:    # always True
      # error /= 4
      print(error)
```

```console
  50
  50
  50
  50
  50
  ...
```

## `for` loop

The recipe for a `for` loop is:

```python
  # for each variable in a sequence,
  for var in seq:
      # execute the expression
      expression
```

### Loop over List

Let's say we have a list `fam`, and we want to print out each of the items in the list separately.

```python
  fam = [1.73, 1.68, 1.71, 1.89]

  # for each height in the list fam
  for height in fam:
      # print the height
      print(height)
```

We should analyse the code to help us better understand the `for` loop.

1. `for height in fam:`: this means we want to execute some code for each `height` in `fam`
2. `height` is an arbitrary variable name. It can be anything, like `h`, or `var`, or even something completely unrelated to what we are doing.
3. Inside the for loop, at every iteration, we print out the value of the current `height`.
4. When Python executes this code, it will first evaluate `seq`. In this case, it is the list `fam`. After this, the actual iteration starts.
5. At each iteration, Python will store the first value in the list, execute the code, then store the second value, execute the code... all the way until all the values have been read, stored, and code executed.

So, the output will be something like this:

```console
  1.73
  1.68
  1.71
  1.89
```

### `enumerate()` function

The `enumerate()` function is very useful to get the index of items in an iterable, such as a list.

Looking at our previous example, say we want to change our output to this:

```console
  index 0: 1.73
  index 1: 1.68
  index 2: 1.71
  index 3: 1.89
```

To achieve this indexing, we use the `enumerate()` function.

```python
  fam = [1.73, 1.68, 1.71, 1.89]
  for index, height in enumerate(fam):
      print("index " + str(index) + ": " + str(height))
```

We should probably explain what's happening.

1. `enumerate(fam)` produces two values on each iteration: the index of the value, and the value itself.
2. On each iteration, `index` will contain the index, and `height` will contain the height.

### Loop over String

The `for` loop does not only work on lists. It also works on strings! In the case of strings, the for loop will iterate through each character in the string, like in this example.

```python
  # for each character in the string
  for char in "family":
      # we capitalize the character
      print(c.capitalize())
```

```console
  F
  A
  M
  I
  L
  Y
```