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

#### Single parameters

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

#### Multiple parameters

Our functions can be made to accept more than one parameter. Suppose that, instead of
just simply squaring a value like we did with `square()`, we wanted to raise a value to
the power of another. We can change our function definition to reflect that!

```python
  def raise_to_power(value1, value2):    # <- should change function name too
      """Raise value1 to the power of value2."""    # <- docstring should reflect change
      new_value = value1 ** value2
      return new_value
```

Now, when we call this function, we need to provide it with two arguments. Essentially,
the number of arguments when calling a function = the number of parameters it has.

```python
  result = raise_to_power(2, 3)    # 2 ** 3
  print(result)
```

```console
  8
```

### Returning values

#### Single return values

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

#### Tuples

Before we touch on multiple return values, we have to first learn tuples.

Tuples are a data type in Python. They are like a list as they can contain multiple values.
They are also immutable - their values cannot be modified. Lastly, they are constructed
using parentheses `()` and commas.

```python
  my_tuple = (1, 2, 3)
  my_list = [1, 2, 3]
  print(type(my_tuple))
  print(type(my_list))
```

```console
  <class 'tuple'>
  <class 'list'>
```

We can also unpack our tuple values into multiple variables in the same line.

```python
  my_tuple = (1, 2, 3)
  a, b, c = my_tuple
```

This assigns `a`, `b` and `c` values in order of how they appear in the tuple.

```python
  print(a)
  print(b)
  print(c)
```

```console
  2
  4
  6
```

To access tuple elements, we do the same indexing magic as we did with lists. Tuples also
use zero-indexing!

```python
  my_tuple = (1, 2, 3)
  print(my_tuple[0])
```

```console
  1
```

#### Multiple return values

To illustrate this concept, let us modify our `raise_to_power` function.

```python
  def raise_both(val1, val2):
      """Raise val1 to the power of val2 and vice versa"""
      new_val1 = val1 ** val2
      new_val2 = val2 ** val1

      new_tuple = (new_val1, new_val2)    # a tuple is used to return multiple values
      return new_tuple
```

```python
  result = raise_both(2, 3)
  print(result)
```

```console
  (8, 9)
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

## Scope

"There is a (time and) place for everything." Sounds familiar? Surprisingly,
this applies to Python too! Every object has its "place" in a Python script, and thus
not all of them are accessible everywhere in a script.

Enter the idea of a *scope* - which part of a program an object or name may be accessed.
Names refer to the variables, or more generally, objects such as functions that are
defined in the program.

There are 3 types of scope:

1. Global scope: a name defined in the main body of a script.
2. Local scope: a name defined within a function. Once the execution of the function
is done, the name defined within the function ceases to exist.
3. Built-in scope: names in pre-defined built-in modules

Sounds confusing? Not to worry, here are a couple of examples that hopefully will make this
clearer for you!

Take a look at our function `square()`.

```python
  def square(val):
      """Squares val and returns it"""
      new_val = val ** 2
      return new_val
```

Let's call the function we just defined.

```python
  square(3)
```

```console
  9
```

Everything works so far! The function `square()` is defined in the global scope and
could be accessed.

Let's try accessing `new_val`, which is defined within the function.

```python
  new_val
```

```console
  NameError: name 'new_val' is not defined
```

We see that it is not accessible! This is because it was only defined within the local scope
of `square()`, so accessing `new_val` outside of `square()` does not make sense.

Now, what if we defined `new_val` once outside (and before) the function?

```python
  new_val = 10

  def square(val):
      """Squares val and returns it"""
      new_val = val ** 2
      return new_val

  square(3)
```

```console
  9
```

Now, let us try to access `new_val`.

```python
  new_val
```

```console
  10
```

Now we see, anytime we call the name in the global scope, Python accesses the name in
the global scope; anytime we call the name in the local scope, Python accesses the name in
the local scope.

If Python cannot find the name in the local scope, then it will access the (same) name
in the global scope.

```python
  new_val = 10    # global scope

  def square(val):
      """Returns the square of a number."""
      new_val2 = new_val ** 2
      return new_val2
```

```python
  square(3)
```

```console
  100
```

As we can see here, we accessed `new_val` defined globally within the function `square()`.
Note that the global value accessed is the value at the time the function is *called*, not
the value when the function is defined.

```python
  new_val = 10
  square(3)
  square(5)
  new_val = 1
  square(10)
```

```console
  100
  100
  1
```

To recap, when we reference a name, first the local scope is searched, then the global.
If the name is in neither, then the built-in scope is searched. By the way, Python's
built-in scope is just a built-in module called `builtins`. To query this module, we have
to import the module.

How about altering the values of global names within a function call? This is where the
`global` keyword comes in. Let's take a look at an example:

```python
  new_val = 10

  def square(value):
      """Returns the square of a number."""
      global new_val
      new_val **= 2
      return new_val
```

Within the function definition, we use the `global` keyword, followed by the name of the
global variable we wish to access and alter. In this case, we changed `new_val` to its
square.

Let's call the function and take a look.

```python
  square(3)
```

```console
  100
```

The function call works as one would expect. Now calling `new_val`:

```python
  new_val
```

```console
  100
```

We see that the global value has indeed been squared by running `square()`.
