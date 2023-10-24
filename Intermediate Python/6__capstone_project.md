# Capstone Project - Winning a bet

## The Context

Your friend and you are climbing stairs, and you are very bored. (and tired.) Then, you had
a brilliant idea to spice things up - a bet! Here's how it will work:

- You throw a dice 100 times
  - If it is a 1 or 2, you go one step down
  - If it is a 3, 4 or 5, you go one step up
  - If it is a 6, you will throw the dice again and walk up the resulting number of steps
- You cannot go below step number 0
- There is a 0.1% chance of falling down the stairs, and you will restart at step 0 (because
you are clumsy oops)

With all of this in mind, you bet with your friend that you will reach 60 steps high. What
is the chance you will win this bet?

## Choosing an approach

How would we tackle this seemingly large task? One way to solve it will be to calculate the
chance analytically using equations. Another approach is to simulate the process thousands
of times and see in what fraction of the simulation you will reach 60 steps (there is a
statistics theory behind this!)

Let's try the second approach here! (Though if you are interested, you can try to work out
the analytical approach)

## Tools we need

### 1. Random Generators

We need to simulate throwing the dice. To do this, we make use of random generators!

We first need to import `numpy`, and inside `numpy`, there is the `random` package. Inside
that package, we find the `rand()` function.

```python
  import numpy as np
  np.random.rand()    # pseudo-random numbers
```

If we run this function, we get a number between 0 and 1.

> **How?**
> 
> Computers typically generate so-called pseudo-random numbers. These are random numbers
> that are generated using a mathematical formula, starting from a random seed. The seed
> was chosen by Python when we called the `rand()` function, but we can set it manually.
> 
> Say we set the seed to `123`, and called `rand()` twice to get two random numbers:
> 
> ```python
>   np.random.seed(123)
>   print(np.random.rand())
>   print(np.random.rand())
> ```
> 
> ```console
>   0.6964691855978616
>   0.28613933495037946
> ```
> 
> Now, if we call the `seed()` function again, set the seed back to `123`, and called
> `rand()` twice more:
>
> ```python
>   np.random.seed(123)
>   print(np.random.rand())
>   print(np.random.rand())
> ```
> 
> ```console
>   0.6964691855978616
>   0.28613933495037946
> ```
>
> we get the same exact random numbers! For the same seed, you are generating the same
> random numbers. This is why it is called "pseudo-random" - it is random but consistent
> between runs. This ensures *reproducibility as other people can reproduce our analysis.
>
> Do try this yourself!

#### Applying this randomness to coin tosses

Suppose we want to simulate a coin toss.

1. Set the seed (this can be anything)
2. Use the `randint(start, end)` function. It takes in two parameters, `start` (inclusive)
and `end` (exclusive), denoting the range the random integer can take.

```python
  import numpy as np
  coin = np.random.randint(0, 2)
```

3. To simulate a coin toss, let us assign one value to be heads, and one to be tails. Here,
we let `0` denote heads and `1` denote tails.
4. We use an `if` statement to print out the corresponding result depending on the result
of our "coin toss" using `randint()`!

```python
  # continued from the previous snippet of code
  # if you want to replicate this, copy both this snippet and the previous one
  if coin == 0:
      print('heads!')
  else:
      print('tails!')
```

### 2. Building a list using a for-loop

What better way to learn than through an example? First, have a look at this code.

```python
  # imports numpy and sets the seed
  import numpy as np
  np.random.seed(123)

  # initialise an empty list to store results
  outcomes = []

  # use a for loop to flip the coin 10 times, and store the result of each flip
  for i in range(10):
      # the random integer generated that simulates the coin flip
      coin = np.random.randint(0, 2)

      # append result based on coin flip
      if coin == 0:
          outcomes.append("heads")
      else:
          outcomes.append("tails")

  # outputs the result of the 10 coin flips
  print(outcomes)
```

The code simulates the coin flip 10 times, then stores the result and prints it. To run the for loop 10 times, we used the `range()` function that generates a list of numbers that we
can use to iterate over.

> #### `range()` function
>
> The `range()` function can be used in two ways.
>
> 1. `range(stop)`: This returns an object that produces a sequence of integers from 0
> (inclusive) to `stop` (exclusive) by increments of 1.
> 2. `range(start, stop, step)`: This returns an object that produces a sequence of
> integers from `start` (inclusive) to `stop` (exclusive) by increments of `step`. which
> can be positive or negative.
>
> This means that `range(i, j)` produces `i, i+1, i+2, ..., j-1`; `range(k)` produces
> `1, 2, ..., k-1`. Then, `range(i+5, i, -1)` produces `i+5, i+4, i+3, i+2, i+1`.

Well, this is nice, but what if I wanted you to track the total number of tails at each
instance of the game? How would we modify our code?

```python
  # this part is still the same
  import numpy as np
  np.random.seed(123)

  # to track number of tails seen at each instant
  tails = [0]

  # for loop concept is still the same - we want to flip the coin 10 times
  for i in range(10):
      # flipping the coin is still the same
      coin = np.random.randint(0, 2)

      # instead of an if statement, we append the result of the coin flip each time
      # by adding the value of `coin` to the previous seen instant since we defined
      # `coin == 1` to be tails
      tails.append(tails[x] + coin)
```

This edited version will do the trick! I'll explain it, not to worry.

1. `tails` is initialised as a list with one element - 0, since before any coins are
flipped, we would not have seen any tails.
2. instead of an if-else structure, we make use of our definition of `coin` - `0` meaning
heads, and `1` meaning tails. Meaning, we can just add the value of `coin` to the most
recent entry to `tails` - since `0` would not change the sum, and `1` increments it - just
what we wanted!

## The Execution

### 1. Movement mechanism

We simulate dice rolls using the `randint()` function in `np.random`. Here, we tested two
dice rolls using the seed `123`:

```python
  import numpy as np
  np.random.seed(123)

  print(np.random.randint(1, 7))
  print(np.random.randint(1, 7))
```

```console
  6
  3
```

Now, to determine our next move on our long flight of stairs, we use the result of the
dice roll.

```python
  # imports numpy and sets the seed
  import numpy as np
  np.random.seed(100)

  # starting step
  step = 50

  # roll the dice
  dice = np.random.randint(1, 7)

  # movement based on rules
  if dice <= 2:
    step -= 1
  elif 3 <= dice <= 5:
    step += 1
  else:
    step += np.random.randint(1, 7)

  # print out dice and step
  print(dice)
  print(step)
```

### 2. Random Walk

If we use a dice to determine our next step, we can call this a random step. what if we
repeated this 100 times? We would have a succession of random steps - a random walk.

```python
  # imports numpy and sets the seed
  import numpy as np
  np.random.seed(100)

  # initialise random_walk list, which tracks the step we are on at each instant of time
  # since the first step starts on step 0, the list will contain one value - 0
  random_walk = [0]

  # the 100 dice throws
  for x in range(100):
      # set step: the last element in random_walk (ie where we are right now)
      step = random_walk[-1]

      # dice roll
      dice = np.random.randint(1, 7)

      # determine next step
      if dice <= 2:
        step -= 1
      elif dice <= 5:
        step += 1
      else:
        step += np.random.randint(1, 7)

      # append the new (ie current) step to random_walk
      random_walk.append(step)

  # print random_walk to check results
  print(random_walk)
```

When we print out `random_walk`, we should do a check on the values. Our rules stated that
`step` cannot be negative. This means that all our values in `random_walk` need to be
positive! If there is a negative value, then something is wrong. Currently, this set of
code allows for negative `step`. So we have to fix that!

A typical way to solve such issues is by using `max()` to make sure that the minimum
value (*yes I know this is not as intuitive*) that is stored is at least a certain value.
Let's apply it here!

```python
  import numpy as np
  np.random.seed(100)

  random_walk = [0]

  for x in range(100):

      step = random_walk[-1]
      dice = np.random.randint(1, 7)

      # used max() here to make sure step cannot go below 0
      # since step can only possibly go negative from 0 on a decrement step
      if dice <= 2:
        step = max(step - 1, 0)
      elif dice <= 5:
        step += 1
      else:
        step += np.random.randint(1, 7)

      random_walk.append(step)

  print(random_walk)
```

### 3. Visualising the walk

