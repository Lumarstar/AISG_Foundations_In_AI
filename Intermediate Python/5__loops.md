# Loops

## `while` loop

Similar to the `if` statement, it executes the code inside if `condition` evaluates to
`True`. However, for the `if` statement, Python only runs the statements once, and moves
on. For the `while` loop, the code will be executed over and over again until `condition`
evaluates `False`.

This means the `while` loop "repeats an action until a condition is met".

We follow this format:

```python
  while condition:
      expression
```

For example, say we have `error = 50.0`, and we divide `error` by 4 at every run, until
`error` no longer exceeds 1.

```python
  error = 50.0

  while error > 1:
      error /= 4
      print(error)

  print('done!')
```

Let's take a look at a step-by-step analysis of what happens.

1. Initially, `error = 50.0`, so the condition `error > 1` is `True`. Thus, the statements
in the `while` loop will be executed.
2. `error` will become 12.5, and is printed out.
3. Again, the condition is `True` and the code will be executed.
4. `error` becomes 3.125 and is printed out.
5. The condition is still `True`, so the code will be executed (again.)
6. `error` becomes 0.78125, and is printed out.
7. The condition finally evaluates to `False`, and Python will exit the `while` loop and
moves on.
8. `'done!`' will be printed.

```console
  12.5
  3.125
  0.78125
  done!
```

Notice that the updating of our condition is extremely important in a `while` loop. If the
condition is not updated, then we will have an "infinite loop" situation, where the `while`
loop will keep being executed, and Python will crash.

Say we did not update the value of `error`:

```python
  error = 50.0

  while error > 1:    # always True
      # error /= 4
      print(error)
```

```console
  50
  50
  50
  50
  50
  ...
```

## `for` loop

The recipe for a `for` loop is:

```python
  # for each variable in a sequence,
  for var in seq:
      # execute the expression
      expression
```

### Loop over List

Let's say we have a list `fam`, and we want to print out each of the items in the list
separately.

```python
  fam = [1.73, 1.68, 1.71, 1.89]

  # for each height in the list fam
  for height in fam:
      # print the height
      print(height)
```

We should analyse the code to help us better understand the `for` loop.

1. `for height in fam:`: this means we want to execute some code for each `height` in `fam`
2. `height` is an arbitrary variable name. It can be anything, like `h`, or `var`, or even
something completely unrelated to what we are doing.
3. Inside the for loop, at every iteration, we print out the value of the current `height`.
4. When Python executes this code, it will first evaluate `seq`. In this case, it is the
list `fam`. After this, the actual iteration starts.
5. At each iteration, Python will store the first value in the list, execute the code, then
store the second value, execute the code... all the way until all the values have been
read, stored, and code executed.

So, the output will be something like this:

```console
  1.73
  1.68
  1.71
  1.89
```

#### Looping through list of lists

It is possible to loop through lists of lists! We can have multiple `var`s that will store
each of the values in each iteration.

For example, take a look at this list of lists:

```python
  house = [["hallway", 11.25], 
           ["kitchen", 18.0], 
           ["living room", 20.0], 
           ["bedroom", 10.75], 
           ["bathroom", 9.50]]
           
  for place, area in house:
      print("the " + str(place) + " is " + str(area) + " sqm")
```

`house` is a list of lists, and in each sublist there are two values. We used `place`
and `area` to store the two values in each sublist (ie. in each iteration).

### `enumerate()` function

The `enumerate()` function is very useful to get the index of items in an iterable, such as
a list.

Looking at our previous example, say we want to change our output to this:

```console
  index 0: 1.73
  index 1: 1.68
  index 2: 1.71
  index 3: 1.89
```

To achieve this indexing, we use the `enumerate()` function.

```python
  fam = [1.73, 1.68, 1.71, 1.89]
  for index, height in enumerate(fam):
      print("index " + str(index) + ": " + str(height))
```

We should probably explain what's happening.

1. `enumerate(fam)` produces two values on each iteration: the index of the value, and the
value itself.
2. On each iteration, `index` will contain the index, and `height` will contain the height.

### Loop over String

The `for` loop does not only work on lists. It also works on strings! In the case of
strings, the for loop will iterate through each character in the string, like in this
example.

```python
  # for each character in the string
  for char in "family":
      # we capitalize the character
      print(c.capitalize())
```

```console
  F
  A
  M
  I
  L
  Y
```

### Loop over Dictionary

We have a dictionary `world` prepared for you. Look!

```python
  world = {
      "afghanistan": 30.55,
      "albania": 2.77,
      "algeria": 39.21
  }
```

We want to use a for loop to loop through the key-value pairs of the dictionary. Hmm...
Ah! I have an idea! Let's try it this way, and hope `key` and `value` are correctly set...

```python
  # idea 1: simply using the dictionary as the seq
  for key, value in world:
      print(key + " -- " + str(value))
```

```console
  ValueError: too many values to unpack (expected 2)
```

Oh no... Python expected 2 values (because `key, value`). But the current method we have
is not able to provide it 2 values. What to do...

Not to worry, because we have methods at our disposal! We can use the `.items()` method
to generate a key and value in each iteration of the `for` loop:

```python
  for key, value in world.items():
      print(key + " -- " + str(value))
```

```console
  algeria -- 39.21
  afghanistan -- 30.55
  albania -- 2.77
```

Yay it works! But something else looks amiss... wasn't `afghanistan` first in the
dictionary? Why is `algeria` first now? That is because dictionaries are inherently
unordered - the order in which they are iterated over is not fixed.

By the way, if you did not already know from the previous section (where we looped through
lists), the variable names `key` and `value` are totally arbitrary. We can call them
anything we want, so long as we remember them and are able to use them properly. **The order does matter though,** because the first variable gets the key, and the second gets
the value.

Here, take a look at an example where different variable names are used.

```python
  europe = {'spain':'madrid', 'france':'paris', 'germany':'berlin',
            'norway':'oslo', 'italy':'rome', 'poland':'warsaw', 'austria':'vienna' }

  # country refers to the key, and capital refers to the value
  for country, capital in europe.items():
      print(f"the capital of {country} is {capital}")
```

### Loop over NumPy Array

The method we used to iterate through a normal Python List would work for 1D arrays, but
for 2D arrays and above, it wouldn't get the individual values within those subarrays.

So instead, we use a NumPy function `np.nditer()`. If we break the name down, this function
means "N-Dimension Iteration". Let's see this function in action:

```python
  import numpy as np
  np_height = np.array([1.73, 1.68, 1.71, 1.89, 1.79])
  np_weight = np.array([65.4, 59.2, 63.6, 88.4, 68.7])
  meas = np.array([np_height, np_weight])

  print("this gets the entire arrays printed out")
  for val in meas:
      print(val)

  # the heights will be printed out first
  # followed by the weights
  # because np_height comes before np_weight
  print("this gets the values printed out one by one")
  for val in np.nditer(meas):
      print(val)
```

```console
  this gets the entire arrays printed out
  [1.73, 1.68, 1.71, 1.89, 1.79]
  [65.4, 59.2, 63.6, 88.4, 68.7]
  this gets the values printed out one by one
  1.73
  1.68
  1.71
  1.89
  1.79
  65.4
  59.2
  63.6
  88.4
  68.7
```

An additional tip is to add an additional argument `end` to the `print()` call. This adds
the specified string to the end of the printed outputs. Here are a few types of strings you
can specify:

| **String** | **Function**                    |
|------------|---------------------------------|
| `" "`      | Adds a space behind             |
| `"\t"`     | Adds a tab/indent behind        |
| `"\n"`     | Starts the cursor on a new line |

### Loop over Pandas DataFrame

For the last time, we will refer to our dear friend, `brics` (a Pandas DataFrame).

```console
           country     capital    area   population
  BR        Brazil    Brasília   8.516       200.40
  RU        Russia      Moscow  17.100       143.50
  IN         India   New Delhi   3.286      1252.00
  CH         China     Beijing   9.597      1357.00
  SA  South Africa    Pretoria   1.221        52.98
```

So how would we iterate through the DataFrame? Hmm for a first try, let's try this:

```python
  for val in brics:
      print(val)
```

```console
  country
  capital
  area
  population
```

It seems we got the column names in return. Interesting! But not quite iterating through
all the values.

In Pandas, we have to be explicit and mention that we want to iterate over the rows. To do
this, we call the `iterrows()` method on the DataFrame.

```python
  for label, row in brics.iterrows():
      print(label)
      print(row)
```

The `iterrows()` method looks at the DataFrame, and on each iteration generates two pieces
of data: the label of the row and then the actual data in the row as a Pandas Series. Since
`row` is a Pandas Series, we can easily select additional information from it using the
subsetting techniques we covered earlier.

```console
  BR
  country      Brazil
  capital    Brasilia
  area          8.516
  population    200.4
  Name: BR, dtype: object
  ...
```

As we can clearly see here, the `label` is first printed out, then the `row` is printed
out.

If we want to selectively print certain data from the Series, we can just select them:

```python
  for label, row in brics.iterrows():
      # select capital
      print(lab + ": " + row["capital"])
```

```console
  BR: Brasilia
  RU: Moscow
  IN: New Delhi
  CH: Beijing
  SA: Pretoria
```

#### Adding new columns

To do this, we have to make use of many of the ideas we touched on previously! Let us break
this down into steps.

1. We use... (no surprises here) a for loop! The specification for the for loop can be the
same, as we will need both the row label and the row data.

```python
  for lab, row in brics.iterrows():
      # fill in content here
```

2. We can calculate the length of each country name by selecting each country name from
`row`, and apply the `len()` function.
3. This is arguably the most important step: storing the data in the correct place! Here,
we used the `loc[]` method (which is label-based), and we stored the data at the
intersection between the row label `row`, and the column label `"name_length"`.

```python
  for lab, row in brics.iterrows():
      # creating one entry of the new Series on every iteration
      brics.loc[lab, "name_length"] = len(row["country"])

  # print(brics)
```

To check if our code worked, uncomment the `print(brics)` statement when you run this code!
*(remember to first import pandas and declare brics properly)* Here's the print out for you:

```console
           country     capital    area   population   name_length
  BR        Brazil    Brasília   8.516       200.40             6
  RU        Russia      Moscow  17.100       143.50             6
  IN         India   New Delhi   3.286      1252.00             5
  CH         China     Beijing   9.597      1357.00             5
  SA  South Africa    Pretoria   1.221        52.98            12
```

It worked! (Kind of.) It isn't very efficient though, because we're creating a Series object
(for each row) during each iteration of the for loop. It would not matter for small
DataFrames like `brics`, but for larger datasets... well we would not recommend this. What
should we use then?

#### `apply()` method

Lo and behold, the `apply()` method. This method allows you to apply a function on a
particular column of a DataFrame in an element-wise fashion. In our `brics` example, having
this in our toolkit means we don't even have to use for loops anymore!

```python
  brics["name_length"] = brics["country"].apply(len)
```

What we are doing here, is we first select the entire `"country"` column, and then apply
the `len` function on this column, and finally we store it in the `"name_length"` column
in `brics`. Since this new column did not exist beforehand, similar to a dictionary,
this new column would be created and added to the DataFrame!
