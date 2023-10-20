# Pandas

Pandas is very useful to deal with tabular data. While 2D NumPy arrays seem to be able to do the trick, they only allow a single type for every array.
Pandas, built on NumPy, is more flexible and therefore useful!

## Configuring Pandas

To install pandas, we use this command in the console:

```console
  > pip install pandas
```

or if you want to play safe and specify that it is Python3 you are using,

```console
  > pip3 install pandas
```

To use it in your code, you need to import it first, like so:

```python
  import pandas as pd
```

By convention, Pandas is imported and `pd` is the alias we use.

## Series

*This section is not included in the DataCamp course, but I felt it was right to
include this because there are some important things that should be said.*

A series is usually a result of trying to retrieve one column or row of values
from a DataFrame.

To retrieve values from a Series is similar to retrieving values from dictionaries!
We use square brackets `[]` to retrieve values that are associated with its label.

```python
  value = my_series[label]
```

## DataFrame

DataFrames are tables of data. Let's say we have a table that contains information about countries:

| **country**  | **capital** | **area** | **population** |
|--------------|-------------|----------|----------------|
| Brazil       | Brasília    | 8.516    | 200.4          |
| Russia       | Moscow      | 17.10    | 143.5          |
| India        | New Delhi   | 3.286    | 1252           |
| China        | Beijing     | 9.597    | 1357           |
| South Africa | Pretoria    | 1.221    | 52.98          |
|     *str*    |    *str*    |  *float* |     *float*    |

We can use a DataFrame to represent this data, like this:

```console
           country     capital    area   population
  BR        Brazil    Brasília   8.516       200.40
  RU        Russia      Moscow  17.100       143.50
  IN         India   New Delhi   3.286      1252.00
  CH         China     Beijing   9.597      1357.00
  SA  South Africa    Pretoria   1.221        52.98
```

The rows represent the observations, and the columns represent the variables. Also, notice that each row has a unique row label: 
BR for Brazil, RU for Russia, and so on. The columns, or variables, also have labels: country, population, and so on. 
Notice that the values in the different columns have different types.

### Creating DataFrames

#### Creating from Dictionaries

When we create a DataFrame from a dictionary, the keys of the dictionary would become
the column labels, and the values of the dictionary would become the main bulk of the data.

In order to create a DataFrame from a dictionary, we use the `.DataFrame()` class.
To show you what this means, take a look at this example, which uses data from the
previous table:

```python
  my_dict = {
    'country': ['Brazil', 'Russia', 'India', 'China', 'South Africa'],
    'capital': ['Brasilia', 'Moscow', 'New Delhi', 'Beijing', 'Pretoria'],
    'area': [8.516, 17.10, 3.286, 9.597, 1.221],
    'population': [200.4, 143.5, 1252, 1357, 52.98]
  }

  import pandas as pd
  brics = pd.DataFrame(my_dict)
  brics
```

```console
          country     capital    area   population
  0        Brazil    Brasília   8.516       200.40
  1        Russia      Moscow  17.100       143.50
  2         India   New Delhi   3.286      1252.00
  3         China     Beijing   9.597      1357.00
  4  South Africa    Pretoria   1.221        52.98
```

There we go! We created our first DataFrame using a dictionary. You may see that there
are numbers on the left - they are automatic row labels assigned by Pandas. We can
specify these labels too! The how-tos will be shown in a later section.

#### Creating from csv

A `.csv` file is a file that contains *comma separated values*. To import data from a
csv file to a DataFrame, we use the `.read_csv()` function, with the csv file path
passed in as an argument.

```
brics.csv

  ,country,capital,area,population
  BR,Brazil,Brasília,8.516,200.40
  RU,Russia,Moscow,17.100,143.50
  IN,India,New Delhi,3.286,1252.00
  CH,China,Beijing,9.597,1357.00
  SA,South Africa,Pretoria,1.221,52.98
```

```python
  brics = pd.read_csv("path/to/brics.csv")
  brics
```

```console
   Unnamed: 0        country    capital    area  population
  0         BR        Brazil   Brasília   8.516      200.40
  1         RU        Russia     Moscow  17.100      143.50
  2         IN         India  New Delhi   3.286     1252.00
  3         CH         China    Beijing   9.597     1357.00
  4         SA  South Africa   Pretoria   1.221       52.98
```

The row labels weren't recognised... but it's okay! We can fix this by specifying the
`index_col` argument, like this:

```python
  brics = pd.read_csv("path/to/brics.csv", index_col=0)
  brics
```

```console
           country     capital    area   population
  BR        Brazil    Brasília   8.516       200.40
  RU        Russia      Moscow  17.100       143.50
  IN         India   New Delhi   3.286      1252.00
  CH         China     Beijing   9.597      1357.00
  SA  South Africa    Pretoria   1.221        52.98
```

### Manipulating DataFrames

#### Specifying Labels

To refresh your memory, here is `brics` without customised row labels:

```python
  brics
```

```console
          country     capital    area   population
  0        Brazil    Brasília   8.516       200.40
  1        Russia      Moscow  17.100       143.50
  2         India   New Delhi   3.286      1252.00
  3         China     Beijing   9.597      1357.00
  4  South Africa    Pretoria   1.221        52.98
```

To specify labels, we use the `.index` attribute. Each element in the list corresponds
to the label of that row.

```python
  brics.index = ["BR", "RU", "IN", "CH", "SA"]
  brics
```

```console
           country     capital    area   population
  BR        Brazil    Brasília   8.516       200.40
  RU        Russia      Moscow  17.100       143.50
  IN         India   New Delhi   3.286      1252.00
  CH         China     Beijing   9.597      1357.00
  SA  South Africa    Pretoria   1.221        52.98
```

### Accessing DataFrames

All the ways shown here will refer to the previously created DataFrame `brics`.

#### Column Access `[]`

In general, to access a single column, we use square brackets to access it:

```python
  dataframe[col_name]
```

It is what we do to access the column `country`:

```python
  brics['country']
```

```console
  BR        Brazil
  RU        Russia
  IN         India
  CH         China
  SA  South Africa
  Name: country, dtype: object
```

If we use the `type()` function to find out more about this object that is returned,

```python
  type(brics['country'])
```

```console
  pandas.core.series.Series
```

we can see that we actually get a **Pandas Series**. A Series is like a one dimensional
array that can be labelled, and by pasting together a bunch of Series, we get a
DataFrame.

In order for the data to be returned in a DataFrame, we have to use double square brackets.

```python
  brics[['country']]
```

```console
  BR        Brazil
  RU        Russia
  IN         India
  CH         China
  SA  South Africa
```

Checking its type,

```python
  type(brics[['country']])
```

```console
  pandas.core.frame.DataFrame
```

we see that it is a DataFrame, but with only one column.

To retrieve multiple columns, we give it a list of column labels, like so:

```python
  dataframe[list_of_labels]
```

Continuing with our previous example,

```python
  brics[['country', 'capital']]
```

```console
           country     capital
  BR        Brazil    Brasília
  RU        Russia      Moscow
  IN         India   New Delhi
  CH         China     Beijing
  SA  South Africa    Pretoria 
```

This returns us a "sub-DataFrame".

#### Row Access `[]`

We access rows through slicing. Yes! Like Python Lists!

```python
  dataframe[start:end]
```

Similar to Python Lists, we retrive the rows corresponding to the `start` index (inclusive)
to the `end` index (exclusive).

Applying it on our convenient `brics` DataFrame,

```python
  brics[1:4]
```

```console
           country     capital    area   population
  RU        Russia      Moscow  17.100       143.50
  IN         India   New Delhi   3.286      1252.00
  CH         China     Beijing   9.597      1357.00
```

#### `loc[]` function

Previously, we looked at our old friend `[]`. They work, but they offer limited
functionality. Ideally, having something similar to 2D NumPy arrays would be good,
where we used square brackets, but also the comma notation, which allowed us to slice
across rows and columns together.

This is where the `loc[]` function comes in. The `loc[]` function is label-based.
We put the label of the row of interest in square brackets.

```python
  dataframe.loc[my_label]
```

Applying it on `brics`, say we want to retrieve information about Russia:

```python
  brics.loc['RU']
```

```console
  country       Russia
  capital       Moscow
  area            17.1
  population     143.5
  Name:   RU, dtype: object
```

We get a Pandas Series which contains all the information in the row!

For the result returned to be a DataFrame, we need to give the `loc[]` function a list
of labels.

```python
  # the list of labels can be either row or column labels, but not a mix of both
  dataframe.loc[list_of_labels]
```

Applying it on `brics`, say we want to also include India and China:

```python
  brics.loc[['RU', 'IN', 'CH']]
```

```console
     country    capital    area  population
  RU  Russia     Moscow  17.100       143.5
  IN   India  New Delhi   3.286      1252.0
  CH   China    Beijing   9.597      1357.0
```

Up till now, we have only selected entire rows. However, `.loc[]` is capable of so
much more! We can **extend our selection with a comma** and thus specify the columns
we are interested in.

In general, we specify the row and columns we want like so:

```python
  dataframe[list_of_row_labels, list_of_column_labels]
```

This returns the intersection of the specified rows and columns.

Let's trying using this to retrieve the country names and capitals of Russia,
India and China.

```python
  brics.loc[['RU', 'IN', 'CH'], ['country', 'capital']]
```

```console
     country    capital                      
  RU  Russia     Moscow
  IN   India  New Delhi
  CH   China    Beijing
```

Here's a neat trick if we want to specify all the rows (or columns). Instead of
painstakingly writing every label down, we can use `:` in place of the list of labels.

Using `brics` as an example, say we want to retrieve all the countries' names and capitals:

```python
  brics.loc[:, ['country', 'capital']]
```

```console
           country    capital                  
  BR        Brazil   Brasília
  RU        Russia     Moscow
  IN         India  New Delhi
  CH         China    Beijing
  SA  South Africa   Pretoria
```

#### `iloc[]` function

Unlike its `loc[]` counterpart, `iloc[]` is integer position-based. Meaning, instead of
using labels, we use indices.

```python
  # if only a scalar integer is given, it will retrieve that row as a Series
  dataframe.iloc[index]

  # if only one list of indices is given, it will assume it refers to rows
  dataframe.iloc[list_of_indices]

  # specifying for both rows and columns
  dataframe.iloc[list_of_row_indices, list_of_col_indices]

  # you can also specify the row and column indices by slicing
  dataframe.iloc[[row_start:row_end, col_start:col_end]]

  # selecting all rows (this method works for columns too)
  dataframe.iloc[:, list_of_col_indices]
```

Applying all of these on `brics`,

```python
  # retrieves Russian information
  print("Information on Russia:")
  print(brics.iloc[1])

  # retrieves Russia, India and China
  print("\nInformation on Russia, India, China:")
  print(brics.iloc[[1, 2, 3]])

  # retrieves the name and capitals of Russia, India and China
  print("\nInformation on Russia, India, China's names and capitals:")
  print(brics.iloc[[1, 2, 3], [0, 1]])

  # retrieves the names and capitals of all countries
  print("\nInformation on all countries' names and capitals:")
  print(brics.iloc[:, [0, 1]])
```

```console
  Information on Russia:
  country       Russia
  capital       Moscow
  area            17.1
  population     143.5
  Name: RU, dtype: object
  
  Information on Russia, India, China:
     country    capital    area  population
  RU  Russia     Moscow  17.100       143.5
  IN   India  New Delhi   3.286      1252.0
  CH   China    Beijing   9.597      1357.0
  
  Information on Russia, India, China's names and capitals:
     country    capital
  RU  Russia     Moscow
  IN   India  New Delhi
  CH   China    Beijing
  
  Information on all countries' names and capitals:
           country    capital
  BR        Brazil   Brasilia
  RU        Russia     Moscow
  IN         India  New Delhi
  CH         China    Beijing
  SA  South Africa   Pretoria
```
