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

### `global` keyword

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

## Nested functions

We can define functions within functions! This allows us to efficiently repeat processes
within functions without having to repeat ourselves. For example, say we want to obtain
the remainder plus five of three values:

```python
  def mod2plus5(x1, x2, x3):
      """Returns the remainder plus 5 of three values."""
      x1_new = x1 % 2 + 5
      x2_new = x2 % 2 + 5
      x3_new = x3 % 2 + 5

      return (x1_new, x2_new, x3_new)
```

The actual content of the function is repeated multiple times... Instead of this, we can
define a function within `mod2plus5()` (ie a nested function) to aid us with this process:

```python
  def mod2plus5(x1, x2, x3):
      """Returns the remainder plus 5 of three values."""

      def inner(x):
          """Returns the remainder plus 5 of a value."""
          return x % 2 + 5

      return (inner(x1), inner(x2), inner(x3))
```

Now, we can instead call the `inner()` function thrice and our job is done. The syntax for
the inner function is the same as that for any other function. Both these approaches
will return the same result. The main difference is, that using an inner (helper) function
allows our processes to be scaled up with relative ease. Meaning that we can repeat the
process many times without having to repeat ourselves for every instance.

### Returning functions

Nesting functions also allow us to simply return (the inner) function. Here is an example:

```python
  def raise_val(n):
      """Return the inner function."""

      def inner(x):
          """Raise x to the power of n."""
          raised = x ** n    # n is part of the enclosing function
          return raised

      return inner
```

The function `raise_val` takes in an argument `n`, and returns a function that takes in
another argument `x` and raises it to the power of `n`. Let's break the steps down and show
it as Python code.

```python
  # with this line, we can see square defined as:
  # def square(x):
  #    raised = x ** 2
  #    return raised
  square = raise_val(2)

  # similarly, with this line, we can see cube defined as:
  # def cube(x):
  #    raised = x ** 3
  #    return raised
  cube = raise_val(3)

  print(square(2), cube(3))
```

```console
  4 64
```

Let's try to digest our findings:

1. Passing the number 2 to `raise_vals` creates a function that squares any number. 
Similarly, passing the number 3 creates a function that cubes any number.
2. `square` and `cube` both remember the values of `n` assigned to them, even though
`n` is local to `raise_val`, and it has finished its execution. This is known as *closure*.
More can be found [here](https://en.wikipedia.org/wiki/Closure_(computer_programming)).

### `nonlocal` keyword

Another cool thing - similar to how we can use `global` to change global variables defined
in the global scope, we can use the `nonlocal` keyword to create and change names in an
enclosing scope.

```python
    def outer():
        """Prints the value of n."""
        n = 1

        def inner():
            nonlocal n    # access n defined outside
            n = 2    # this changes the value of n, even outside this function
            print(n)

        print(n)
        inner()
        print(n)
```

```console
  1
  2
  2
```

### Scope (again)

To summarise, Python will search for the variable/name in this order: 1. Locally;
2. Any enclosing/outer functions; 3. Globally; 4. Built-in. Let's take a look at an example.

```python
  global_var = "hello world!"

  def unhappy(sad_text):
      # sad_text is an enclosing variable

      # feelings is an enclosing variable here
      feelings = "sad"
      
      def cry():
        shouts = "I am sad because " + sad_text
        feelings = "horrible"    # feelings here is local to cry
        print(shouts)
        print("I feel " + feelings)    # this accesses local feelings
      
      # this will not work here as shouts is local to cry()
      # print(shouts)
      
      # this should work
      print(global_var)
      cry()

      # this will access the enclosing feelings variable
      print("I feel " + feelings)
  
  # this will not work
  # print(sad_text)
  
  # but this will
  # list() is a built-in name
  print(list())
  
  # and this will work too
  unhappy("I broke up.")
```

```console
  []
  hello world!
  I am sad because I broke up.
  I feel horrible
  I feel sad
```

## Default arguments

Default arguments are values that parameters will take by default if an argument is not
provided when the function is called. To add a default argument, we declare a parameter
with `=value` in the function definition.

```python
  def power(number, pow=1):    # <- default value of pow is 1
      """Rause number to the power of pow"""
      return number ** pow
```

We can still call the function with 2 arguments, as we'd expect.

```python
  power(9, 2)
```

```console
  81
```

```python
  power(9, 1)
```

```console
  9
```

However, if we only use one argument, the function call will use the default argument of 1
for `pow`!

```python
  power(9)
```

```console
  9
```

## Flexible arguments

For more information about flexible arguments, refer
[here](https://realpython.com/python-kwargs-and-args/).

### `*args`

To illustrate the power of this keyword, it's best we look at an example. Let us create a
simple function to add variables up.

```python
  def sum_all(a, b):
      return a + b
```

This is a simple function that works as intended - it takes sums! But it seems a little
limited. After all, our function is called `sum_all`, but as of now we only can sum 2
variables. How about using a list?

```python
  def sum_all(list_of_vals):
      result = 0

      for num in list_of_vals:
          result += num

      return result

  my_list = [1, 2, 3]
  sum_all(my_list)
```

```console
  6
```

This works as intended too! Now, we just need to create a list to use this function. But,
it'd be nice if we did not need to define the list just to use this function... Enter the
`*args` argument.

This keyword allows any number of arguments to be specified if it is defined as a parameter
in the function definition. We denote this using `*`. Using this in our `sum_all` function:

```python
  def sum_all(*args):
      result = 0

      for num in args:
          result += num

      return result
```

`args` is a tuple. We can now call `sum_all` with any number of arguments to add them
all up!

> What is important here is the single star `*` that precedes the parameter name, not the
> name `args`. In fact, we can use any name we desire, like `*integers`, so long as `*` is
> in front.

### `**kwargs`

Similar to `*`, we can use the double star `**` to pass in an arbitrary number of
keyword functions (also known as kwargs), which are arguments preceded by identifiers.

To write such a function, we use the parameter `kwargs` preceded by a double star
(`**kwargs`). This turns the identifier-keyword pairs that we will give as arguments
into a dictionary within the function body.

```python
  def print_all(**kwargs):
      """Print out key-value pairs in **kwargs"""
      for key, value in kwargs.items():
          print(key + ": " + value)
```

Using our newly defined `print_all` function, we now can print out the identifier-keyword
(ie parameter-value) pairs we gave!

```python
  print_all(name="En Hao", job="SCH V Trainer", is_happy="No")
```

```console
  name: En Hao
  job: SCH V Trainer
  is_happy: No
```

> What is important is the double star `**`, not the name `kwargs`. So long as `**` is at
> the front, we can use any name we want.
