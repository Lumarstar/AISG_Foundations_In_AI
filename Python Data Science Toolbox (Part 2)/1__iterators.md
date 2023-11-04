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
