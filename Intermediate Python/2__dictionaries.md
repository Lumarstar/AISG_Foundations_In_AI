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
