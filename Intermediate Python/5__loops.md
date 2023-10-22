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