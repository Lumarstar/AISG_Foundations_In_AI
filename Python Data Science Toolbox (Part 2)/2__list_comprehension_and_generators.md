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

## Conditionals in comprehensions

We can also use (more than one, if needed) conditionals in comprehensions.

```python
  even_squares = [num ** 2 for num in range(10) if num % 2 == 0]
  print(even_squares)
```

```console
  [0, 4, 16, 36, 64]
```

This simple list comprehension results in a list which contains the square of values in
`range(10)` under the condition that the value itself is even.

Other than the `if` clause, we can use the `else` clause too!

```python
  even_squares = [num ** 2 if num % 2 == 0 else 0 for num in range(10)]
  print(even_squares)
```

```console
  [0, 0, 4, 0, 16, 0, 36, 0, 64, 0]
```

The `else` clause allows us to specify that for odd integers, we output 0. If there is no
`else` clause, the `if` statement is behind the `for` clause. If there is an `else` clause,
the `if`-`else` statement will go before the `for` loop.

## Conditionals in dictionaries

We can use comprehension to create new dictionaries! As per the definition, dictionaries are
defined with curly brackets `{}` and key=value pairs are separated by colons `:`.

Take a look at this example!

```python
  pos_neg = {num: -num for num in range(5)}
  print(pos_neg)
```

```console
  {0: 0, 1: -1, 2: -2, 3: -3, 4: -4}
```

Here, we created a dictionary that creates key-value pairs by assigning the negative of
the key as our value.
