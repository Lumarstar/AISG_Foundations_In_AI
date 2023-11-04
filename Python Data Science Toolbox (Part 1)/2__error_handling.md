# Error Handling

## Errors

When we pass an incorrect argument into functions, an error would be thrown back at us. For
example, if we tried to pass the string `"hello"` to the `float()` function, a `ValueError` will
occur.

```python
  float("hello")
```

```console
  ValueError: could not convert string to float: 'hello'
```

Other than `ValueError`, there are many other types of errors.

Now, this concept does not only apply to built-in functions. When we write our own functions,
we should also make sure that arguments passed into our functions are valid. This means we
should catch specific problems and write specific error messages.

As an example, let us check out this function that computes the square root of a number.

```python
  def sqrt(x):
      """Returns the square root of a number."""
      return x ** 0.5
```

If we pass integer (and even float) arguments into the function, it works as expected!

```python
  print(sqrt(4))
  print(sqrt(10))
  print(sqrt(1.5))
```

```console
  2.0
  3.1622776601683795
  1.224744871391589
```

But what if we passed it a string, such as `"hello"`?

```python
  sqrt("hello")
```

```console
  TypeError: unsupported operand type(s) for ** or pow(): 'str' and 'float'
```

We see that an error corresponding to a line of code within the function definition is
thrown. However, such messages may not be very useful to the user, who likely has no idea
what's going on within our function. We should thus strive to provide useful error messages
for the functions we write.

## Handling Errors

### Exceptions

Exceptions are errors caught during execution. The main way we catch exceptions in Python
is through the `try-except` clause. Let's try applying this concept in `sqrt()`.

```python
  def sqrt(x):
      """Returns the square root of a number."""
      try:
          return x ** 0.5
      except:
          print("x must be an int or float")
```

What would happen here is that Python will attempt to execute the code block within `try`.
If it works without any errors, it will exit the `try-except` clause and continue on with
the rest of the code. On the other hand, if any errors are caught, the `except` clause will
be run, and then it just continues with the rest of the code after the `try-except` clause.

```python
  print(sqrt(4))
  print(sqrt(1.5))
  sqrt("hello")
```

```console
  2.0
  1.224744871391589
  x must be an int or float
```

Now, we see that the function behaves well for integers and floats, and prints out what we
want for strings.

We can also catch specific errors, by specifying them in the `except` clause. Like so:

```python
  def sqrt(x):
      """Returns the square root of a number."""
      try:
          return x ** 0.5
      except TypeError:
          print("x must be an int or float")
```

This will only catch `TypeError`, and allow other errors to pass through the clause. If you
are interested in what types of errors there are, do look up the Python documentation.

### Raising Errors

We can also raise errors by using the keyword `raise`. Expanding on our `sqrt()` example,
even though negative integers yield valid results, the results are complex numbers. In our
case, we want the result to be real. So, we will look to raise an error if the argument is
negative.

```python
  def sqrt(x):
      """Returns the square root of a number."""
      if x < 0:
          raise ValueError("x must be non-negative")
      try:
          return x ** 0.5
      except:
          print("x must be an int or float")
```

```python
  sqrt(-9)
```

```console
  ValueError: x must be non-negative
```

Here, we used an `if` clause to raise the error. We can also raise errors in the `except`
clause! In general, raising errors follows this format: `raise Error(error_msg)`.
