# Python Functions

## Functions

A function is a piece of reusable code, aimed at solving a particular task. Even without knowing its implementation, passing in
inputs will return an output that you desire. The inputs of functions are called **arguments**, and Python will try to match our inputs
to the arguments. 

### 1. `max()` function

The `max()` function returns the largest item in an iterable. It can also be used to find the largest item 
between two or more parameters.

The max function has two forms:
```python
  # to find the largest item in an iterable, like a list
  max(iterable, [*iterables,] [key,] [default])
  
  # to find the largest item between two or more objects
  max(arg1, arg2, [*args,] [key])
```

### 2. `round()` function

As its name suggests, it will round a number to a specified precision. It follows this syntax:
```python
  round(number, [precision])
```
where `number` is the number we want to round, and `precision` is the number of digits after the decimal point we want to keep.
If `precision` is not specified (because it is optional), the function will automatically round to the closest integer.

### 3. `help()` function

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

### 4. `len()` function

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

### 5. `pow()` function

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

### 6. `sorted()` function

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

## Methods

Methods can be seen as "functions that belong to objects". What is an object? A few examples of objects are strings, floats and lists. Each object
has methods that are unique to their type.

- some objects will have methods with the same name, but they have different behaviours.
- some methods can change the object they are called on

### List Methods

#### 1. `.index()`

This method gets us the (first) index of the value that matches the argument. Take this scenario as an example:
```python
  fam = ['liz', 1.73, 'emma', 1.68, 'mom', 1.71, 'dad', 1.89]
  fam.index("mom")    # call method `index()` on fam
```
```console
  > 4
```

#### 2. `.count()`

This method returns the number of time a value appears in the list. Take this scenario as an example:
```python
  fam = ['liz', 1.73, 'emma', 1.68, 'mom', 1.71, 'dad', 1.89]
  fam.count("mom")    # call method `count()` on fam
```
```console
  > 1
```

#### 3. `.append()`

Appends/Adds a new element to the end of the list.
```python
  list_name.append(new_element)
```

#### 4. `.remove()`

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

#### 5. `.reverse()`

Reverses the list in-place. This method modifies the list. For example,
```python
  my_list = [0, 1, 2, 4, 6, 0]
  my_list.reverse()
  print(my_list)
```
```console
  > [0, 6, 4, 2, 1, 0]
```

### String Methods

#### 1. `.capitalise()`

This method returns the original string, but with only the first letter capitalised. See this example:
```python
  string_name = "hello WORLD!"
  string_name.capitalise()
```
```console
  > Hello world!
```

#### 2. `.replace()`

This method replaces all instances of specified old values of the string with new values. See this example:
```python
  sister = "Liz"
  sister.replace("z", "sa")    # string_name.replace(old_substr, new_substr)
  print(sister)
```
```console
  > Lisa
```

#### 3. `.upper()`

Returns an entirely capitalised version of the string, but does not modify the original string.
```python
  str_name.upper()
```

#### 4. `.count()`

Similar to the list method `.count()`, the string method `count()` returns the number of occurrences of a specific substring in the string.
```python
  str_name.count(substr)
```
