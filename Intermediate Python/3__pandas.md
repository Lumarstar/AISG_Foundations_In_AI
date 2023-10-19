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
  0        Brazil    Brasília   8.516       200.40
  1        Russia      Moscow  17.100       143.50
  2         India   New Delhi   3.286      1252.00
  3         China     Beijing   9.597      1357.00
  4  South Africa    Pretoria   1.221        52.98
```

### Manipulating DataFrames

#### Specifying Labels

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
