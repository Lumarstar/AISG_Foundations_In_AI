# NumPy

Numeric Python, better known as NumPy, is most potent when the NumPy array is used, as it allows calculations to be performed
across the entire array.

## Configuring NumPy

1. Use `pip` to install numpy
```console
  > pip install numpy
```
2. Import numpy before using it in scripts
```python
  import numpy as np
```

## Creating NumPy arrays

We create numpy arrays on the basis of regular Python lists, like so:
```python
  numpy.array(my_list)
```

Using the example provided by DataCamp, let's say we have two Python lists - `height`, and `weight`.
```python
  height = [1.73, 1.68, 1.71, 1.89, 1.79]
  weight = [65.4, 59.2, 63.6, 88.4, 68.7]
```

We use the `numpy.array()` function to create numpy arrays:
```python
  import numpy as np
  np_height = np.array(height)
  np_height
```
```console
  > array([1.73, 1.68, 1.71, 1.89, 1.79])
```
```python
  np_weight = np.array(weight)
  np_weight
```
```console
  > array([65.4, 59.2, 63.6, 88.4, 68.7])
```

## Performing calculations

The power of numpy arrays can be shown here! If we used our regular Python lists `height` and `weight` like so,
```python
  weight / height ** 2
```
the following error will be thrown:
```console
  > TypeError: unsupported operand type(s) for ** or pow(): 'list' and 'int'
```

But after converting the lists into numpy arrays, if we perform the same calculation directly on the arrays, they work!
```python
  np_weight / np_height ** 2
```
```console
  > array([21.85171573, 20.97505669, 21.75028214, 24.7473475, 21.44127836])
```

There are still a few things to note:
- NumPy assumes that its arrays only contain one type. If there is more than one type, the resulting Numpy array will **contain only a single type**. This is known as **type coersion**.
```python
  np.array([1.0, "is", True])
```
```console
  > array(['1.0', 'is', 'True'], dtype='<U32')
```
- A NumPy array is a **new Python type**, meaning operators and methods will behave differently!

## Subsetting NumPy arrays

1. Use square brackets `[]`, like how we do for Python Lists.
2. Using **an array of booleans**

We will elaborate more on the second method below.

Say we have a numpy array `bmi`:
```python
  bmi = array([21.85171573, 20.97505669, 21.75028214, 24.7473475, 21.44127836])
```
and we want to get all the BMI values that are above 23.

The first step is to use `>`, like so:
```python
  bmi > 23
```
This returns an array of boolean values, `True` if the corresponding value fulfils the specified condition, and `False` otherwise.
```console
  array([False, False, False, True, False])
```

The next step is to use this boolean array in square brackets like normal subsetting!
```python
  bmi[bmi > 23]
```
This too is subsetting! This will select elements with a corresponding boolean value of `True`.
```console
  array([24.7473475])
```

## 2D (and other multi-dimensional) NumPy Arrays

Just like with Python lists, we can also create multi-dimensional arrays in NumPy! Say for instance, we have a 2D Python List, and we use that to create our 2D NumPy array:
```python
  np_2d = numpy.array([[1.73, 1.68, 1.71, 1.89, 1.79], [65.4, 59.2, 63.6, 88.4, 68.7]])
  np_2d
```
```console
  array([[1.73, 1.68, 1.71, 1.89, 1.79], [65.4, 59.2, 63.6, 88.4, 68.7]])
```
This is nothing new! It still is a regular data structure. Now using the `.shape` attribute, we can see how many rows and columns `np_2d` has!
```python
  np_2d.shape
```
```console
  > (2, 5)    # 2 rows, 5 columns
```

For these N-D arrays, the arrays can still only contain a single type!

**Note:** To access methods, we have round brackets at the end `()`, but we don't use them when accessing attributes

### Subsetting multi-dimensional arrays

The normal vanilla subsetting used with Python N-D arrays work perfectly fine here. But! We can also do more advanced stuff too!

Say for instance, we want to access the element at the 1st row and the 3rd column. Here are two ways we can do this:
```python
  my_np_arr[0][2]    # the normal way
  my_np_arr[0, 2]    # using commas! [row, col]
```

We can do this as well:
```python
  my_np_arr[:, 1:3]    # using colons to access values by slicing works too!
```

This comma method finds the intersection of the specified row(s) and column(s), and thus provides more flexibility.

### Arithmetic

Similar to 1D NumPy arrays, we can combine numbers, vectors (ie 1D arrays) and even matrices (2D and above) with other matrices! We show an example here:
```python
  import numpy as np
  np_mat = np.array([[1, 2],
                     [3, 4],
                     [5, 6]])
  np_mat * 2    # single number
  np_mat + np.array([10, 10])    # vector
  np_mat + np_mat    # matrix
  np_mat + 2
```
As you can see below, if we use single numbers or vectors, they will be applied to all the elements in the matrix accordingly.
```console
  > array([[2, 4],
           [6, 8],
           [10, 12]])

  > array([[11, 12],
           [13, 14],
           [15, 16]])

  > array([[2, 4],
           [6, 8],
           [10, 12]])

  > array([[3, 4],
           [5, 6],
           [7, 8]])
```

## NumPy and Statistics

We need ways to visualise large amounts of data! Because NumPy enforces single types within its data structures, it is a lot faster than normal Python functions! NumPy can help us in such ways:

### 1. `np.mean()`

This NumPy function, when given a NumPy array, will return the mean of all values.

### 2. `np.median()`

This NumPy function, when given a NumPy array, will return the median value.

### 3. `np.corrcoef()`

This NumPy function takes in two series, and sees whether they are related.

### 4. `np.std()`

This NumPy function, when given a series, returns the standard deviation of the series.

### 5. `np.sum()`

As the name suggests, this returns the sum of all the elements in the iterable.

### 6. `np.sort()`

As the name suggests, this sorts an iterable!

### 7. `np.random.normal()`

This function takes in three arguments:

- Distribution Mean
- Distribution Standard Deviation
- Number of samples

and churns out random values based on these inputs!
