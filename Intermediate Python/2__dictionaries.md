# Dictionaries

Imagine we have a list of countries `countries`, and a list of their capitals, `capitals`. Each index corresponds to one country and
their respective capital. To access the capital of a specific country, we would have to do the following:

```python
  countries = ['spain', 'france', 'germany', 'norway']
  capitals = ['madrid', 'paris', 'berlin', 'oslo']

  # say for instance we want to find the capital of germany
  
  # step 1: find the index of the country we want
  ind_country = countries.index("germany")

  # step 2: use the index we found to print out the capital
  print(capitals[ind_country])
```

This parallel array technique works. But it is not efficient. Lo and behold, I present to you: dictionaries.
Dictionaries provide a way to connect a *key* to a *value* directly. We can use a key to access a value within the dictionary!
Here is (a preview of) how it would work if we used dictionaries instead of lists:

```python
  capitals = {'spain': 'madrid', 'france': 'paris', 'germany': 'berlin', 'norway': 'oslo'}
  print(capitals['germany'])
```

## Creating dictionaries

To create a dictionary, we enclose key-value pairs using curly brackets `{}`. A key-value pair is represented by `key: value`.

```python
  my_dict = {key1: value1, key2: value2, ... }
```

## Accessing values

To access values stored in the dictionary, we enclose its key within square brackets `[]`. This is quite similar to lists!

```python
  my_dict[key]    # returns the value associated with this key
```

To access all the keys in the dictionary, we use the `.keys()` method.

```python
  my_dict.keys()
```

To check if a key is present in a dictionary, we can use this:

```python
  key in my_dict
```

This returns a boolean. `True` if `key` is a key in `my_dict`, and `False` if otherwise.

## Creating Key-Value Pairs

Keys should be unique. If we add a new key-value pair using an existing key, the old value will be overridden, like so:

```python
  world = {'a': 1, 'b': 2, 'c': 3, 'b': 10}
  print(world)
```
```console
  > {world = {'a': 1, 'b': 10, 'c': 3}
```

These keys should also be immutable objects, ie. they cannot be changed after they are created. For example, strings, booleans,
integers and floats are immutable objects! If we use a mutable object as a key, we will get thrown an error, like so:

```python
  {['just', 'to', 'test']: 'value'}
```
```console
  > TypeError: unhashable type: list
```

## Assigning/Adding Values

We can also add new values to the dictionary! Like this:

```python
  my_dict[new_key] = new_value
```

With the same syntax, we can also change the values that are assigned to the keys:

```python
  my_dict[existing_key] = new_value
```

The old value will be overridden, and the dictionary will be updated! This works because each key in the dictionary is unique, so
Python knows we are trying to update values, rather than create a new key-value pair entry.

## Deleting Key-Value Pairs

To delete an entry from the dictionary, we use the `del()` function:

```python
  del(my_dict[key])
```

## Dictionaries... in dictionaries?

That's right! Just like how lists can contain lists, dictionaries can, too, contain anything. For a key-value pair, the value can be anything:
a list, a dictionary... it's all within bounds!

Here's an example of a multi-leveled dictionary:

```python
  europe = {'spain': { 'capital':'madrid', 'population':46.77 },
           'france': { 'capital':'paris', 'population':66.03 },
           'germany': { 'capital':'berlin', 'population':80.62 },
           'norway': { 'capital':'oslo', 'population':5.084 }}
```

It's perfectly possible to chain square brackets to select elements. This should look familiar... you've dealt with this in lists too.
To fetch the population for Spain from europe, for example, you need:

```python
  europe['spain']['population']
```

## Lists vs Dictionaries

| **Lists**                                                          | **Dictionaries**                                                      |
|--------------------------------------------------------------------|-----------------------------------------------------------------------|
| Select, update, and remove with `[]`                               | Select, update, and remove with `[]`                                  |
| Indexed by a range of numbers                                      | Indexed by unique, immutable keys                                     |
| Collection of values - order matters, for selecting entire subsets | Lookup table with unqiue keys (yep dictionaries are unordered)        |
