# Iterators

## Iterators? Iterables?

Remember `for` loops? We have been using them to loop through lists, strings, dictionaries
and a sequence of numbers produced by a special `range` object.

The reason that we can loop over such objects is that they are special objects
called **iterables**. An iterable is an object that has an associated `iter()` method. Once
the `iter()` method is applied to an iterable, an **iterator object** is created.

> Actually, this is what the `for` loop does behind the scenes! It takes an iterable, creates
> the associated iterator object, and iterates over it.

An **iterator** is an object that has an associated `next()` method that produces the
consecutive values.

To create an iterator from an iterable, we pass the iterable into the `iter()` function.

```python
  word = 'Da'    # strings are iterables
  it = iter(word)
```

## Iterating over iterables

### `next()`

One way to iterate through an iterator is the `next()` function. We call the function and
pass in the iterator. The first time this is done, the first value is returned. Subsequent
times will return subsequent values (in order) until no more values are left. The function
will then throw a `StopIteration` error.

Continuing the example from above,

```python
  next(it)
```

```console
  'D'
```

```python
  next(it)
```

```console
  'a'
```

```python
  next(it)
```

```console
  StopIteration
```

### Iterating at once with `*`

The splat operator `*` unpacks all elements of an iterator or an iterable. This means we
can, for example, print all values of an iterator in one fell swoop:

```python
  word = 'Data'
  it = iter(word)
  print(*it)
```

```console
  D a t a
```

However, once we use the splat operator, we cannot do any iteration with the iterator anymore
because all values have been iterated through. We would have to redefine our iterator.
Continuing from the code snippet above,

```python
  # no more values to unpack...
  next(it)
```

```console
  StopIteration
```

### Iterating over dictionaries

As mentioned before, we have to use the `.items()` method to unpack the dictionary.

### Iterating over file connections

We can also use `iter()` and `next()` to return lines from a text file. Take a look:

```
short_song.txt

  This is a short song.
  This is the last line.
```

```python
  file = open('short_song.txt')
  it = iter(file)
  print(next(it))
  print(next(it))
```

```console
  This is a short song.
  This is the last line.
```

## Useful functions with iterators

### `enumerate()`

`enumerate()` takes in any *iterable* as an argument, such as a list, and returns a special
enumerate object, which consists of pairs containing the elements of the original iterable,
along with their index within the iterable.

We can use `list()` to typecast this object into a list of tuples. Not only that, the
special enumerate object is also an iterable and we can unpack its values using a `for`
loop.

```python
  hello = ['hi', 'nihao', 'konnichiwa']
  for index, value in enumerate(hello):
      print(index, value)
```

```console
  0 hi
  1 nihao
  2 konnichiwa
```

`enumerate()` actually takes in 2 arguments - `enumerate(iterable[, start=0])`. If we change
the value assigned to `start`, the indexing will start from that value.

```python
  for index, value in enumerate(hello, start=10):
      print(index, value)
```

```console
  10 hi
  11 nihao
  12 konnichiwa
```

### `zip()`

`zip()` accepts an arbitrary number of iterables and returns an iterator of tuples. The
tuple at index `i` will contain all the elements in each of the iterables at index `i`.

Zipping the iterables together creates a zip object which is an iterator of tuples.

Say we have two lists, `one` and `two`.

```python
  one = [1, 2, 3]
  two = [4, 5, 6]
```

```python
  z = zip(one, two)
  print(type(z))
```

```console
  <class 'zip'>
```

By zipping the two lists `one` and `two` together, we have created a zip object. We can
typecast this into a list and print it to see its contents:

```python
  z_list = list(z)
  print(z_list)
```

```console
  [(1, 4), (2, 5), (3, 6)]
```

The first element is a tuple containing the first elements of each list that was zipped.
The second element is a tuple containing the second elements, and so on. This aligns nicely
with what we discussed before - The tuple at index `i` will contain all the elements in
each of the iterables at index `i`.

Alternatively, we can use a `for` loop to loop through the zip object.

```python
  for z1, z2 in zip(one, two):
      print(z1, z2)
```

```console
  1 4
  2 5
  3 6
```

We could also use the splat operator `*` to print out all the elements, just like any other
iterator!

```python
  z = zip(one, two)
  print(*z)
```

```console
  (1, 4) (2, 5) (3, 6)
```

We can think of `*` as unzipping what has been previously `zip`ped. `*` unpacks an iterable
such as a list or a tuple into positional arguments in a function call.
