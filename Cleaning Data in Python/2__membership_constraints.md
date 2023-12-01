# Membership Constraints

## Categorical Data

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
