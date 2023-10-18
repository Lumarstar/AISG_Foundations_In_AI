# Pandas

Pandas is very useful to deal with tabular data. While 2D NumPy arrays seem to be able to do the trick, they only allow a single type for every array.
Pandas, built on NumPy, is more flexible and therefore useful!

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
