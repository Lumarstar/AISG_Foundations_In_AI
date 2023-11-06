# List Comprehension and Generators

## List comprehensions

We have previously learnt to use `for` loops to populate lists. In this scenario, we
want to create a new list from an old list of numbers, and increase their values by 1.

```python
  nums = [1, 2, 3]
  new_nums = []

  for num in nums:
      new_nums.append(num + 1)

  print(new_nums)
```

```console
  [2, 3, 4]
```

However, `for` loops are "inefficient" as compared to using list comprehension, which
is faster and neater.

Here's how to use it:

1. Set up square brackets `[]`
2. Within the square brackets, add the values we want to create, ie the *output expression*
3. Lastly, the `for` clause which references the original list

Referring to the previous example,

```python
  nums = [1, 2, 3]
  new_nums = [nums + 1 for num in nums]
  print(new_nums)
```

```console
  [2, 3, 4]
```

List comprehension is not just limited to lists. It can be used over any iterable!

```python
  # used with range object
  nums = [num for num in range(10)]

  # used with dictionary
  items = [(key, value) for key, value in my_dict.items()]

  # the list goes on
```

In summary, list comprehensions collapse `for` loops for building lists into a single line.
They require:

1. an iterable
2. an iterator variable that represents the members of the iterable
3. an output expression

### Nested `for` loops

List comprehension can also be used in place of nested `for` loops. Compare these two code
snippets.

Without list comprehension:

```python
  pairs = []

  for num1 in range(0, 2):
      for num2 in range(6, 8):
          pairs.append((num1, num2))

  print(pairs)
```

```console
  [(0, 6), (0, 7), (1, 6), (1, 7)]
```

With list comprehension:

```python
  pairs = [(nums1, nums2) for nums1 in range(0, 2) for nums2 in range(6, 8)]
  print(pairs)
```

```console
  [(0, 6), (0, 7), (1, 6), (1, 7)]
```

YES! We do this by putting both of the `for` loops in the list comprehension. This
compromises on readability, however, so there needs to be a fine line drawn between squeezing
everything and writing readable code.

More information about list comprehension can be found
[here](https://pythonexamples.org/python-list-comprehension-multiple-if-conditions/).
