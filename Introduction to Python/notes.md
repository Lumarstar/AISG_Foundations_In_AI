# Introduction to Python

## References
This set of notes is made with reference to Datacamp's ["Introduction to Python" course](https://app.datacamp.com/learn/courses/intro-to-python-for-data-science).

## Python Basics

### `print()` function
The `print()` function allows you to output results through the terminal. 

For example, if we type this into our Python interpreter:
```python
  print (7 + 10)
```
we will get this as our output:
```console
  > 17
```

Here's another example. If we key this into our Python interface:
```python
  print(5 / 8)
```
we will get this as our output:
```console
  > 0.625
```

### Comments
Comments are denoted by the `#` character. They are meant for us to read, and Python will ignore anything that comes after `#`.

Example:
```python
  # Division
  print(5 / 8)

  print(7 + 10)    # Addition
```
Python will also ignore `# Addition` even though it is on the same line as `print(7 + 10)`, meaning you can write
comments on the same line as your code.

### Arithmetic Operations
Python is able to handle basic arithmetic operations, like addition `+`, subtraction `-`, multiplication `*`, and division `/`.

Examples:
```python
  # Addition
  print(4 + 5)

  # Subtraction
  print(5 - 5)

  # Multiplication
  print(3 * 5)
  
  # Division
  print(10 / 2)
```
```console
  > 9
  > 0
  > 15
  > 5.0
```
