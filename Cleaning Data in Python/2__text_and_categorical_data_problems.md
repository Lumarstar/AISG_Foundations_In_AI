# Text and Categorical Data Problems

## Membership Constraints

### Context

Categorical data represents variables that represent a predefined finite set of categories.
Examples are marriage status (unmarried vs married) and loan statuses (defaulted, paid, etc.).
They are often coded as numbers for machine learning models to recognise them.

Since categorical data represents variables that represent predefined finite sets of
categories, they cannot have values that go beyond these predefined categories. (ie. they
can **only** take up these predefined values)

#### A word on Joins

Joins are extremely useful in treating inconsistencies in data. We have two main types of
joins: 1. Anti Joins; 2. Inner Joins. We join DataFrames on common columns between them.

Anti Joins take two DataFrames `A` and `B` and return data from one of the DataFrames that
is *not contained in another*. *Left anti joins* of `A` and `B` will return data that exists
only in `A` (so intersections with `B` will be excluded too), while *right anti joins* of
`A` and `B` will return data that exists only in `B`.

Inner Joins take two DataFrames `A` and `B` and return data that is contained in *both*
DataFrames.

### Problems

We can have inconsistencies in categorical data for various reasons. Here are some potential
reasons:

- Data Entry Errors (Free text vs Dropdown)
- Parsing Errors

### Treatment

There are several approaches we can take, with increasingly specific solutions for different
types of inconsistencies.

1. Dropping data with incorrect categories
2. Remap incorrect categories to correct ones
3. Inferring categories based on data

#### Dropping Data - An Example

In this example, we will take a look at 2 DataFrames:

1. `study_data`, which contains a list of first names, birth dates and blood types.

```python
  study_data
```

```console
        name   birthday  blood_type
  1     Beth 2019-10-20          B-
  2 Ignatius 2020-07-08          A-
  3     Paul 2019-08-12          O+
  4    Helen 2019-03-17          O-
  5 Jennifer 2019-12-17          Z+
  6  Kennedy 2020-04-27          A+
  7    Keith 2019-04-19         AB+
```

2. `categories`, which contain the correct possible categories for the blood type column.

```python
  categories
```

```console
    blood_type
  1         O-
  2         O+
  3         A-
  4         A+
  5         B+
  6         B-
  7        AB+
  8        AB-
```

There is an inconsistency here! There is no blood type `Z+`. Thankfully, the `categories`
DataFrame will help us systematically spot all rows with these inconsistencies.

Remember joins? Here, if we took a left anti join on `study_data` and `categories`, all the
data in `study_data` with inconsistent blood types would be returned, and an inner join
returns all the rows containing consistent blood type signs.

Now we see how to do this in Python!

We first get all inconsistent categories in the `blood_type` column of the `study_data`
DataFrame by creating a *set (which stores unique values)* out of `blood_type`. Then, 
we use the `.difference()` method which takes the `blood_type` column as an argument from
the `categories` DataFrame. This returns all the categories in `blood_type` that are not in
`categories`.

```python
  inconsistent_cats = set(study_data['blood_type']).difference(categories['blood_type'])
  print(inconsistent_cats)
```

```console
  {'Z+'}
```

> Other than sets, we can also use the `.unique()` method of a DataFrame to get unique
> values of specific columns in a DataFrame, like so:
>
> ```python
>   dataframe[col].unique()
> ```

We then find the inconsistent rows by finding all the rows of `blood_type` that are equal
to the inconsistent categories by using the `.isin` method. This returns a Series of
boolean values that are `True` for inconsistent rows and `False` for consistent rows.
Finally, we subset the `study_data` DataFrame based on these boolean values.

```python
  inconsistent_rows = study_data['blood_type'].isin(inconsistent_cats)
  study_data[inconsistent_rows]
```

```console
        name   birthday blood_type
  5 Jennifer 2019-12-17         Z+
```

The very last step is to drop the values! We just use the tilde symbol `~` while subsetting
which returns everything except inconsistent rows.

```python
  inconsistent_data = study_data[inconsistent_rows]
  consistent_data = study_data[~inconsistent_rows]
```

```console
        name   birthday  blood_type
  1     Beth 2019-10-20          B-
  2 Ignatius 2020-07-08          A-
  3     Paul 2019-08-12          O+
  4    Helen 2019-03-17          O-
  6  Kennedy 2020-04-27          A+
  7    Keith 2019-04-19         AB+
```

## Categorical Variables

### More Errors

When cleaning categorical data, we can encounter other types of errors too. They
include:

1. Value Inconsistency

- Inconsistent Fields
- Trailing white spaces

2. Collapsing too many categories to few

- Creating new groups (eg. `0-20K`, `20-40K`, ... from continuous household
income data)
- Mapping groups to new ones (eg. Mapping household income categories to only two,
ie. `rich` and `poor`)

3. Making sure data is of type `category` (refer to first set of notes in this
series)

### Troubleshooting!

#### (Ensuring) Value Consistency

A common data categorisation problem is having categorical values with different
types of *capitalisation*.

Here are some examples:

- `married` vs `Married`
- `UNMARRIED` vs `unmarried`

If we do not treat this, subsequent data analysis will be misleading. Say we have
a DataFrame `demographics` which stores the marriage status of a population, and
the categorical values are `unmarried`, `married`, `MARRIED` and `UNMARRIED` (ie.
inconsistent values)

```python
  marriage_status = demographics['marriage_status']
```

```python
  # this only works on Series.
  marriage_status.value_counts()
```

```console
  unmarried    352
  married      268
  MARRIED      204
  UNMARRIED    176
```

```python
  # alternative method for DataFrame: use groupby
  marriage_status.groupby("marriage_status").count()
```

```console
                  household_income  gender
  marriage_status
  MARRIED                      204     204
  UNMARRIED                    176     176
  married                      268     268
  unmarried                    352     352
```

To fix this specific problem, we can either capitalise or lowercase the entire
`marriage_status` column so the values can be consistent.

Another common problem is *trailing spaces*. Using `demographics` from before, here are
some possible inconsistencies we can see in categorical values:

- ` unmarried` vs `unmarried`
- `married ` vs `married`

Again, not treating this issue can lead to misleading data analysis as data are not grouped
together correctly.

Looking at the `marriage_status` column of `demographics`:

```python
  marriage_status = demographics['marriage_status']
  marriage_status.value_counts()
```

```console
   unmarried    352
  unmarried     268
  married       204
  married       176
```

*Note that there is a trailing space on the right for `married `, which makes it hard to
spot on the output, as opposed to ` unmarried`.*

To fix this specific problem, we can use the `str.strip()` method to strip off all leading
and trailing white spaces (given no input into the method).

#### Collapsing Data into Categories

*Creating categories* out of our data is very useful for grouping data.

As an example, let us create an `income_group` column from the `income` column in the
`demographics` DataFrame. We can do this in two ways:

1. `pd.qcut()` function from `pandas`.

The `qcut()` method automatically divides our data based on its distribution into the
number of categories we set in the `q` argument.

In our example, we created the category names in the `group_names` list and fed it to the
`labels` argument.

```python
  import pandas as pd
  group_names = ['0-200K', '200K-500K', '500K+']
  demographics['income_group'] = pd.qcut(demographics['household_income'], q=3, labels=group_names)

  demographics[['income_group', 'household_income']]
```

```console
        category  household_income
  0    200K-500K  189243
  1        500K+  778533
```

From our return values, we see that the first row misrepresents the actual income
of the income group, as we did not instruct `qcut()` where our ranges lie.

2. `pd.cut()` function from `pnadas`.

Instead of using `pd.qcut()`, we can opt to use `pd.cut()`, which lets us define category
cutoff ranges with the `bins` argument. It takes in a list of cutoff points for each
category, with the final one being infinity represented with `np.inf`.

```python
  import numpy as np
  import pandas as pd
  ranges = [0, 200000, 500000, np.inf]
  group_names = ['0-200K', '200K-500K', '500K+']
  demographics['income_group'] = pd.cut(demographic['household_income'], bins=ranges, labels=group_names)

  demographics[['income_group', 'household_income']]
```

```console
        category  household_income
  0       0-200K  189243
  1        500K+  778533
```

This looks more correct now!

The main difference between `pd.cut()` and `pd.qcut()` is:

- `pd.cut()` creates **equispaced bins** but the frequency of samples is
**unequal in each bin**
- `pd.qcut()` creates **unequal size bins** but the frequency of samples is
**equal in each bin**

as explained [here](https://stackoverflow.com/questions/30211923/what-is-the-difference-between-pandas-qcut-and-pandas-cut).

Sometimes, we may want to reduce the number of categories, ie. *map categories to fewer
ones.*

For example, assume we have a DataFrame `devices` that contains the operating system of
different devices and contains some unique values in the `operating_system` column. As of
now, `operating_system` is: `'Microsoft'`, `'MacOS'`, `'IOS'`, `'Android'`, `'Linux'`.
We want to collapse the categories to become: `'DesktopOS'`, `'MobileOS'`.

To do this, we can use the `.replace()` method. It takes in a dictionary that maps each
existing category to the category name you desire (`mapping` in the code below).

```python
  mapping = {
    'Microsoft': 'DesktopOS',
    'MacOS': 'DesktopOS',
    'Linux': 'DesktopOS',
    'IOS': 'MobileOS',
    'Android': 'MobileOS'
  }
  devices['operating_system'] = devices['operating_system'].replace(mapping)

  devices['operating_system'].unique()
```

```console
  array(['DesktopOS', 'MobileOS'], dtype=object)
```

After a quick print of the unique values in `operating_system`, we see that the mapping
has been done!

## Cleaning Text Data

### Context

Text data is one of the most common data types. It is information that is stored and written
in a text format. It can represent a name, a phone number, an email, a password, and so
much more.

### Common Problems

As with all data, we could encounter problems. Here are some common text data problems:

1. Data Inconsistency
2. Fixed length violations
3. Typographical errors (typos)

Let us take a look at an example of the above violations. We have already imported data from
a CSV file into a DataFrame called `phones`. Let us take a look at this DataFrame. *Please
do not try calling these numbers. They are randomly generated by me, and they could actually
work.*

```python
  print(phones)
```

```console
          Full Name      Phone Number
  0        John Doe          81234567
  1        Mary Sue          81000001
  2     Bart Dimple          99102345
  3       Joe Daddy       +6581092475
  4      Tan Ah Kaw              2345
  5      Tansi Medo          99120934
```

Acceptable phone numbers in Singapore start with the digit `8` or `9`, and are 8 digits
long. In our data, however, we have one entry (row 4) where the phone number is 4 digits,
which is invalid. There is also one phone number with `+65` in front (row 3), which makes
our data inconsistent.

Ideally, we want a consistent `'Phone Number'` column, where all the phone numbers are
aligned to begin with `8` or `9`, and any numbers shorter (or longer) than 8 digits will
be replaced with `NaN` ("Not a Number") to represent a missing value.

```console
          Full Name      Phone Number
  0        John Doe          81234567
  1        Mary Sue          81000001
  2     Bart Dimple          99102345
  3       Joe Daddy          81092475
  4      Tan Ah Kaw               NaN
  5      Tansi Medo          99120934
```

Let us see how this is done!

### Fixing Problems

#### Fixing Simple Problems - Phone Numbers

Let us begin by removing `'+65'`. To do this, we use the `.str.replace()` method which takes
in two values - the string being replaced (`'+65'`) and the string to replace it (an empty
string, `''`).

```python
  phones['Phone Number'] = phones['Phone Number'].str.replace('+65', '')
  phones
```

```console
          Full Name      Phone Number
  0        John Doe          81234567
  1        Mary Sue          81000001
  2     Bart Dimple          99102345
  3       Joe Daddy          81092475
  4      Tan Ah Kaw              2345
  5      Tansi Medo          99120934
```

Finally, we are going to replace all phone numbers that are not 8 digits long with `NaN`. We
can do this by chaining the `'Phone Number'` column with the `.str.len()` method, which
returns the string length of each row in the column. We can then use the `.loc[]` method to
index rows where `digits` are not 8, and replace the value of `'Phone Number'` with
NumPy's `NaN` object.

```python
  import numpy as np
  digits = phones['Phone Number'].str.len()
  phones.loc[digits != 8, 'Phone Number'] = np.nan
  phones
```

```console
          Full Name      Phone Number
  0        John Doe          81234567
  1        Mary Sue          81000001
  2     Bart Dimple          99102345
  3       Joe Daddy          81092475
  4      Tan Ah Kaw               NaN
  5      Tansi Medo          99120934
```

To test whether the column is corrected, we can write assert statements.

```python
  # assert phone number length is 8
  assert phones['Phone Number'].str.len() == 8

  # assert phone number does not contain +65
  assert phones['Phone Number'].str.contains('+65') == False
```

*Remember, `assert` returns nothing if the condition passes. So no outputs = good!*

#### Fixing Complicated Problems - Using Regular Expressions

Sometimes, we are faced with more complicated problems that cannot be solved simply by
applying methods. For example, how would we clean `phones`?

```python
  phones.head()
```

```console
          Full Name      Phone Number
  0        John Doe       +6581234567
  1        Mary Sue              0001
  2     Bart Dimple          +6502345
  3       Joe Daddy     +65-8109-2475
  4      Tan Ah Kaw      +65(80001000)
  5      Tansi Medo          9912-0934
```

`phones` contain a range of symbols, and have inconsistent formats. Not to worry though,
this is where **regular expressions** come in. They give us the ability to search for any
pattern in text data.

```python
  # replace letters with nothing
  phones['Phone Number'] = phones['Phone Number'].str.replace(r'\D+', '')
  phones.head()
```

In our example, we attempt to extract only digits from the phone number column. (Further
treatment is required, but this is the first step.) To do this, we use the `.str.replace()`
method with the regex pattern we want to replace with an empty string. Notice the pattern
fed into the method. This is essentially us telling Pandas to replace anything that is not
a digit with nothing!

```console
          Full Name      Phone Number
  0        John Doe          81234567
  1        Mary Sue              0001
  2     Bart Dimple           6502345
  3       Joe Daddy        6581092475
  4      Tan Ah Kaw          80001000
  5      Tansi Medo          99120934
```

Feel free to read more about regular expressions!
