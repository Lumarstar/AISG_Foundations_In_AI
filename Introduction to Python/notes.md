# Introduction to Python

## References
This set of notes is made with reference to Datacamp's ["Introduction to Python" course](https://app.datacamp.com/learn/courses/intro-to-python-for-data-science).

## Python Basics

### `print()` function
The `print()` function allows you to output results through the terminal. 

If we type this into our Python interpreter:
```python
  print(7 + 10)
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

Comments are denoted by the `#` character. They are meant for us to read, and Python will ignore anything 
that comes after `#`.

```python
  # Division
  print(5 / 8)

  print(7 + 10)    # Addition
```
Python will also ignore `# Addition` even though it is on the same line as `print(7 + 10)`, meaning you can write
comments on the same line as your code.

### Arithmetic Operations

Python is able to handle arithmetic operations, like addition `+`, subtraction `-`, multiplication `*`, division `/`, 
and exponentiation `**`.

Here are some examples:
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

### Variables

Variables allow us to "save" values as we code. To do this, we define a variable with a specific, case-sensitive name, like this:
```python
  height = 1.79  # in metres
  weight = 68.7  # in kilograms
```
In Python, `=` means assignment, not equality!

We can also declare variables in the same row like this:
```python
  var1, var2 = val1, val2
```
`val1` is now assigned to `var1`, and `val2` is assigned to `var2`. Of course, ensure that `val1` and `val2` are valid values 
to be assigned to variables.

We can call up the value assigned to the variable by typing the variable name, as such:
```python
  print(height)
```
```console
  > 1.79
```

Defining variables through other variables, and doing calculations using variables, is also possible:
```python
  bmi = weight / height ** 2
```
If the values of `height` and `weight` change, then correspondingly the value of `bmi` will change as well.

### Types

To check the type of a variable, use the `type()` function.
```python
  type(<var name>)
```

The basic types in Python are:
1. `int`: integer. A number without a fractional part.
2. `float`: float, or a floating point. A number that has both an integer and a fractional part.
3. `str`: strings, a type to represent texts. Single or double quotes are both acceptable to construct a string.
4. `bool`: boolean, a type to represent logical values. `True` or `False`

Some examples of the above types:
```python
  #half is a float
  half = 0.5
  
  # intro is a string
  intro = 'Hello! How are you?'
  
  # is_good is a boolean
  is_good = True
```

**Note**: For different types, operators behave differently. For example,
```python
  2 + 3
  'ab' + 'cd'
```
```console
  > 5
  > 'abcd'
```
For integers (floats and booleans), operators behave like they do in Mathematics. For strings, however, `+` will *join the strings together*,
while `*` will *repeatedly join that string the number of times specified*. In general, how the code behaves depends on the types present.

#### Type Conversion

We can convert between `int`, `float`, `str` and `bool`, depending on what we need to use the variable for. For example,
let's say we have a variable `var`:
```python
  # var becomes an integer
  int(var)

  # var becomes a float
  float(var)

  # var becomes a string
  str(var)

  # var becomes a boolean
  bool(var)
```

### Errors

Whenever Python cannot handle an expression, it will throw an error which tells us what is wrong with that expression.

For example,
```python
  "the correct answer to this multiple choice exercise is answer number " + 2
```
```console
  > Traceback (most recent call last):
      File "<stdin>", line 72, in exceptionCatcher
        raise exception
      File "<stdin>", line 3361, in run_ast_nodes
        if (await self.run_code(code, result,  async_=asy)):
      File "<stdin>", line 3458, in run_code
        self.showtraceback(running_compiled_code=True)
      File "<stdin>", line 2066, in showtraceback
        self._showtraceback(etype, value, stb)
      File "<stdin>", line 72, in exceptionCatcher
        raise exception
      File "<stdin>", line 3441, in run_code
        exec(code_obj, self.user_global_ns, self.user_ns)
      File "<stdin>", line 1, in <module>
        "the correct answer to this multiple choice exercise is answer number " + 2
      TypeError: can only concatenate str (not "int") to str
```

## Python Lists

A list is a **data structure**. It can be declared like so:
```python
  my_list = ['a', 1, -0.5, False, ['hello', 'world']]
```
The variable `my_list` refers to the declared list. A list is a way to give a single name to a collection of values. 
These values, or elements, can have any type; they can be floats, integers, booleans, or strings, 
but also more advanced Python types, even lists (lists within lists are called "sublists"). It is therefore a **compound data type**.

### Creating lists

Lists are declared with a set of square brackets `[]`. Other than just including "pure" values, variables can also be used as
part of the declaration.
```python
  a = "is"
  b = "nice"

  # variables can also be used to declare a list!
  # the value in the list will refer to the value of the variable
  my_list = ["my", "list", a, b]

  # empty lists are fine too! declare them like so:
  empty_list = []

  # generic list syntax
  generic_list = [el1, el2, el3]
```

Printing `my_list` here gives:
```python
  print(my_list)
```
```console
  > ["my", "list", "is", "nice"]
```

### Accessing elements in lists

#### Subsetting lists

Python uses **indices** (singular: index) to access values in lists. Python follows **zero-indexing**, meaning the first index
is `0`, then the next index is `1`, and so on, until the last index is `(length of list) - 1`.

We access the $`k^{th}`$ element of a list like this:
```python
  list_name[k-1]    # Python follows zero-indexing, so the kth element has index k-1
```

We can also count backwards! An index of `-1` is the last element of the list, `-2` is the second last element, and so on.
So that means, if `n` is the length of the list, to access the $`k^{th}`$ element of the list via negative indices, we use:
```python
  # one way to check this is to try specific cases
  # if we want to access the last element in the list, k=n; -(n-k+1) == -1.
  list_name[-(n-k+1)]
```

These values can also be used to perform additional calculations. Take this example, where the second and fourth elements 
of a list `x` are extracted. The strings that result are pasted together using the + operator:
```python
  x = ["a", "b", "c", "d"]
  print(x[1] + x[3])
```
```console
  > "bd"
```

Variables can also be used to reference these extracted values, like so:
```python
  x = ["a", "b", "c", "d"]
  second_el = x[1]
  print(second_el)
```
```console
  > "b"
```

Python lists can contain lists within them, and we can also subset list of lists using square brackets! We show this by an example:
```python
  x = [["a", "b", "c"],
     ["d", "e", "f"],
     ["g", "h", "i"]]
  x[2][0]
  x[2][:2]
```
```console
  > "g"
  > ["g", "h]
```
`x[2]` results in a list that you can subset again by adding additional square brackets. Here is a step-by-step dissection of why it works:
```python
  inner_list = x[2]
  print(inner_list)
  print(inner_list[0])   # equivalent to x[2][0]
```
```console
  > ["g", "h", "i"]
  > g
```

#### List slicing

Slicing allows us to select multiple elements from a list, thus creating a new list. We do this by specifying a range, using a colon `:`.
It has the following syntax:
```python
  # start (inclusive), end (exclusive)
  list_name[start:end]
```
The elements selected will be from index `start` to index `end-1`. 

Leaving out the `start` index, like so:
```python
  list_name[:end]
```
will extract all the elements from the start of the list (ie index `0`) till the index `end-1`.

On the other hand, leaving out the `end` index, like so:
```python
  list_name[start:]
```
will extract all the elements from the index `start` to the end of the list.

### Manipulating lists

#### Changing elements in lists

To do this, we directly reference the element using its index, then update it using `=`.
```python
  # updating one value
  list_name[index] = new_value

  # updating through slicing
  # the length of `new_list` has to be exactly `end - start`
  list_name[start:end] = new_list
```
The value(s) will be updated, and any subsequent references of the list or the element(s) at the specific index (or indices) will now
contain the updated value(s).

#### Adding elements

The `+` operator pastes the contents of two lists together. This is called "extending a list". Here is an example to illustrate that:
```python
  list_one = ['a', 'b']
  list_two = ['c', 'd']

  # the combined lists can be assigned to a new variable, or just be on their own
  # this means `list_one + list_two` on its own is perfectly fine!
  combined_list = list_one + list_two
  print(combined_list)
```
```console
  > ['a', 'b', 'c', 'd']
```

#### Removing elements

The `del()` function removes elements from the list at a specified index. To do this, we use:
```python
  del(list_name[index])
```
This will cause the elements in subsequent indices to move one index to the left to "fill up the gap".

We can also remove elements via slicing too, like so:
```python
  del(list_name[start:end])
```
This will remove all elements starting from index `start` to index `end-1`.

### `list()` type

A list is also a Python data type. A new (empty) list can also be declared like this:
```python
  my_empty_list = list()
```

To see the type of a list, we simply use the `type()` function:
```python
  type(my_empty_list)
  type(my_list)
```
```console
  list
  list
```

### Inner workings of a list

What actually happens when we create a new list, `x`, like this:
```python
  x = ['a', 'b', 'c']
```
Well, in a simplified sense, you're storing a list in your computer memory, and storing the 'address' of that list, so
where the list is in your computer memory, in x. This means that x does not actually contain all the list elements, 
it rather contains a reference to the list.

This becomes important when we copy lists! For example, let:
```python
  y = x    # = ['a', 'b', 'c']

  # changing one element in the list `y`...
  y[1] = 'z'
  y

  # also changed `x`!
  x
```
```console
  > ['a', 'z', 'c']
  > ['a', 'z', 'c']
```
When we copied `x` to `y`, we only copied the **reference** of the list and not the actual values. When you're updating 
an element in the list, it is one and the same list in the computer memory that is being changed. Both x and y point to this 
list, so the update is visible from both variables.

To create a list `y` that points to a new list and have the same values, we have two options:
```python
  # first: use the `list()` function
  y = list(x)

  # second: use slicing to get all the elements explicitly
  y = x[:]
```
Making changes now to `y` will not alter `x`!

## Python Functions

### Functions

A function is a piece of reusable code, aimed at solving a particular task. Even without knowing its implementation, passing in
inputs will return an output that you desire. The inputs of functions are called **arguments**, and Python will try to match our inputs
to the arguments. 

#### 1. `max()` function

The `max()` function returns the largest item in an iterable. It can also be used to find the largest item 
between two or more parameters.

The max function has two forms:
```python
  # to find the largest item in an iterable, like a list
  max(iterable, [*iterables,] [key,] [default])
  
  # to find the largest item between two or more objects
  max(arg1, arg2, [*args,] [key])
```

#### 2. `round()` function

As its name suggests, it will round a number to a specified precision. It follows this syntax:
```python
  round(number, [precision])
```
where `number` is the number we want to round, and `precision` is the number of digits after the decimal point we want to keep.
If `precision` is not specified (because it is optional), the function will automatically round to the closest integer.

#### 3. `help()` function

The `help()` function retrieves the documentation for functions! Using the `round()` function as an example:
```python
  help(round)
```
```console
  round(number, ndigits=None)
    Round a number to a given precision in decimal digits.
    
    The return value is an integer if ndigits is omitted or None.  Otherwise
    the return value has the same type as the number.  ndigits may be negative.
```

#### 4. `len()` function

This function takes in an iterable and returns the length of the iterable (ie how many items it has). An example of an iterable would be
a list, or a dictionary! For example, to find the length of a list `var1`,
```python
  var1 = [1, 2, 3, 4]
  print(len(var1))
```
```console
  > 4
```

The `len()` function can also be used to find the number of characters in a string!

#### 5. `pow()` function

This function does the same as the exponentiation operator `**`, but has an option to take modulo `mod`. Using the `help()` function,
we see that it has the following syntax:
```python
  help(pow)
```
```console
  pow(base, exp, mod=None)
    Equivalent to base**exp with 2 arguments or base**exp % mod with 3 arguments
    
    Some types, such as ints, are able to use a more efficient algorithm when
    invoked using the three-argument form.
```

#### 6. `sorted()` function

As its name suggests, `sorted()` will sort an iterable it is given, and return a *new* list with its elements sorted. Using the `help()`
function, we see that it follows this syntax:
```python
  help(sorted)
```
```console
  sorted(iterable, /, *, key=None, reverse=False)
    Return a new list containing all items from the iterable in ascending order.
    
    A custom key function can be supplied to customize the sort order, and the
    reverse flag can be set to request the result in descending order.
```
`sorted()` takes in three arguments - `iterable`, `key`, and `reverse` `key=None` means that if you don't specify the `key` argument, 
it will be `None`. `reverse=False` means that if you don't specify the `reverse` argument, it will be `False`, by default. (For now, we can 
understand an iterable as being any collection of objects, e.g., a List.)

For example, we have a list `full` that we want to sort in descending order. If we do not want to change `key`, we have to specify `reverse=True`,
like so:
```python
  full = [11.25, 18.0, 20.0, 10.75, 9.50]
  sorted(full, reverse=True)
```
```console
  > [20.0, 18.0, 11.25, 10.75, 9.5]
```
The reason we have to specify `reverse=True` instead of just putting `True` is that Python will try to match the arguments from left to right.
Meaning, if we did not specify, Python will assume that we meant `key=True`, not `reverse=True`, which is not what we want.

### Methods

Methods can be seen as "functions that belong to objects". What is an object? A few examples of objects are strings, floats and lists. Each object
has methods that are unique to their type.

- some objects will have methods with the same name, but they have different behaviours.
- some methods can change the object they are called on

#### List Methods

##### 1. `.index()`

This method gets us the (first) index of the value that matches the argument. Take this scenario as an example:
```python
  fam = ['liz', 1.73, 'emma', 1.68, 'mom', 1.71, 'dad', 1.89]
  fam.index("mom")    # call method `index()` on fam
```
```console
  > 4
```

##### 2. `.count()`

This method returns the number of time a value appears in the list. Take this scenario as an example:
```python
  fam = ['liz', 1.73, 'emma', 1.68, 'mom', 1.71, 'dad', 1.89]
  fam.count("mom")    # call method `count()` on fam
```
```console
  > 1
```

##### 3. `.append()`

Appends/Adds a new element to the end of the list.
```python
  list_name.append(new_element)
```

##### 4. `.remove()`

Removes the first element of a list that matches the input. For example,
```python
  my_list = [0, 1, 2, 4, 6, 0]
  my_list.remove(1)    # removes element at index 1
  print(my_list)

  my_list.remove(0)    # removes the first instance of 0 at index 0
  print(my_list)
```
```console
  > [0, 2, 4, 6, 0]
  > [2, 4, 6, 0]
```

##### 5. `.reverse()`

Reverses the list in-place. This method modifies the list. For example,
```python
  my_list = [0, 1, 2, 4, 6, 0]
  my_list.reverse()
  print(my_list)
```
```console
  > [0, 6, 4, 2, 1, 0]
```

#### String Methods

##### 1. `.capitalise()`

This method returns the original string, but with only the first letter capitalised. See this example:
```python
  string_name = "hello WORLD!"
  string_name.capitalise()
```
```console
  > Hello world!
```

##### 2. `.replace()`

This method replaces all instances of specified old values of the string with new values. See this example:
```python
  sister = "Liz"
  sister.replace("z", "sa")    # string_name.replace(old_substr, new_substr)
  print(sister)
```
```console
  > Lisa
```

##### 3. `.upper()`

Returns an entirely capitalised version of the string, but does not modify the original string.
```python
  str_name.upper()
```

##### 4. `.count()`

Similar to the list method `.count()`, the string method `count()` returns the number of occurrences of a specific substring in the string.
```python
  str_name.count(substr)
```
