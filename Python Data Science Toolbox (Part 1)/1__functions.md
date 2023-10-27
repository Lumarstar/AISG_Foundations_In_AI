# Functions

## Built-in functions

Built-in functions are functions defined by Python. They can be used directly in a Python
program without importing any external libraries.

## User-defined functions

However, built-in functions cannot solve all our needs. Sometimes, we need to define 
user-specific functions.

### Defining a function

To define a function, we begin with the keyword `def`, then the function name followed by
`():`. This is called a function header. The function body will consist of code that executes
once the function is called and code within the function is indented to show 
that it is a code block within the function.

Within our parentheses, we can include (optional, if you have none it is alright too)
parameters that our function can use.

Take a look at an example! Let's say we want to create a function that squares some value.

```python
  def square():    # <- function header
      new_value = 4 ** 2    # <- function body
      print(new_value)
```

Our function will print out `new_value`, defined by `4 ** 2`. As of now, we do not
have parameters for the function yet.

To call the function, we do the same as we did for pre-built functions:

```python
  square()
```

This should yield the value of `4 ** 2`.

```console
  16
```

### Function parameters

What if we wanted to square any other number besides 4? To add that functionality, we add
a parameter to the function definition in between the parentheses.

```python
  def square(value):    # <- function with a parameter
      new_value = value ** 2    # <- 4 replaced by value
      print(new_value)
```

When we call a function with parameters, we have to add arguments.

```python
  python(4)
  python(5)
```

```console
  16
  25
```

> **Note:** There is a slight difference between parameters and arguments. When we define a
> function, we write parameters in the function header. When we call a function, we pass
> arguments into the function.

### Returning values

The function `square()` now accepts a single parameter and prints out its squared value.
However, what if we do not want to print that value directly and instead want to return
the squared value and assign it to some variable?

We can have the function return the new value by adding the `return` keyword, followed by
the value to return.

```python
  def square(value):
      new_value = value ** 2
      return new_value    # <- returns the value rather than printing it out
```

Now we can assign some variable to the result of the function call.

```python
  num = function(4)
  print(num)
```

```console
  16
```

If you assign a variable to a function without a return value, the variable will be of type
`NoneType`.

```python
  var = print(1)    # print() function does not return anything
  type(var)
```

```console
  NoneType
```

### Docstrings

Docstrings are essential to functions. They describe what the function does, and they serve
as documentation for your function so that others can understand what your function does
without having to trace through all the code in the function definition. Do remember to
define a docstring accurately and appropriately.

Function docstrings are placed in the immediate line after the function header within
triple quotation marks.

```python
  def square(value):
      """Returns the square of a value"""    # <- docstring (can also span multiple lines)
      new_value = value ** 2
      return new_value
```
