# Advanced Data Problems

## Uniformity

Unit uniformity is very important for accurate data analysis. Data can be represented by
different units. For instance, temperature can be recorded in both degrees Celsius and
degrees Fahrenheit.

### Example - Temperature Data

Let us take a look at an example. Assume that I have already imported temperature data into
a DataFrame `temperatures`. The dataset was collected from different sources with
temperature data in degrees Celsius and degrees Fahrenheit merged. (If you are
wondering, the temperature data is taken from NYC in March 2019.)

```python
  temperatures.head()
```

```console
         Date     Temperature
  0  03.03.19            14.0
  1  04.03.19            15.0
  2  05.03.19            18.0
  3  06.03.19            16.0
  4  07.03.19            62.6
```

The temperature value on row 4 is... suspiciously high. Well, perhaps there could have
been a major climate event, but common sense tells us that this value is likely in
degrees Fahrenheit, not degrees Celsius.

We had a preview of our data using the `.head()` method, but we can visually confirm the
presence of such data (in the more macro scale) using plots via Matplotlib!

```python
  import matplotlib.pyplot as plt

  # use scatter plot
  # plt.scatter(x=<column name>, y=<column name>, data=<dataframe>)
  plt.scatter(x='Date', y='Temperature', data=temperatures)

  # create labels
  plt.title('Temperature in Celsius March 2019 - NYC')
  plt.xlabel('Dates')
  plt.ylabel('Temperature in Celsius')

  # show plot
  plt.show()
```

![Temperature in Celsius March 2019 - NYC](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/54497d91-0d78-4111-9401-c2a33570d5d2)

Notice the anomalous values with weirdly high temperature values? They must be in
Fahrenheit. By some magic (and I mean Google), we obtained the formula to convert
between degrees Fahrenheit and degrees Celsius:

$$ C = (F - 32) * \frac{5}{9} $$

where $C$ is temperature in degrees Celsius, and $F$ is temperature in degrees
Fahrenheit.

To convert our temperature data, we isolate all rows of temperature columns where it
is above 40 using the `.loc[]` method:

```python
  temp_fah = temperature.loc[temperature['Temperature'] > 40, 'Temperature']
  temp_cels = (temp_fah - 32) * (5/9)
  temperature.loc[temperature['Temperature'] > 40, 'Temperature'] = temp_cels
```

This snippet of code isolates all temperatures above 40 (why 40? Based on contextual
knowledge of NYC! 40 degrees Celsius is a common sense maximum for NYC), and then
converts them into degrees Celsius by using the formula. Lastly, these values are
reassigned to their respective rows that contain Fahrenheit data.

Consistent with our previous approaches, we can always use the `assert` statement
to verify that the conversion has been completed.

```python
  # if assertion passes, nothing is returned
  assert temperatures['Temperature'].max() < 40
```

### Example - Date Data

Date data is also problematic. Dates can be represented in different forms, (like in this
DataFrame `birthdays`) and even though they all mean the same thing, it might be misleading
if it is not standardised.

```python
  birthdays.head()
```

```console
            Birthday  First Name  Last Name
  0         27/27/19       Rowan      Nunez
  1         03-29-19       Brynn       Yang
  2  March 3rd, 2019      Sophia     Reilly
  3         24-03-19      Deacon     Prince
  4         06-03-19    Griffith       Neal
```

Notice the dates here? Row 0 has an obvious error, row 1 has an `mm-dd-yy` format, and row 2
has a `Month D, YYYY` format with the month spelt out. They are so different from each
other!

Not to worry, because we have a solution - **Datetime formatting**!

> Datetime objects represent dates in an idealised calendar. They accept different formats
> that help format dates.
>
> | Date | `datetime` format |
> |---|---|
> | 25-12-2019 | `%d-%m-%Y` |
> | December 25th 2019 | %c |
> | 12-25-2019 | `%m-%d-%Y` |
> | ... | ... |
>
> There is also the `pandas.to_datetime()` function, which automatically accepts most
> date formats, but could raise errors when certain formats are unrecognisable.

We can treat our data inconsistencies by converting our date column to `datetime` using the
`pandas.to_datetime()` function.

```python
  import pandas as pd
  birthdays['Birthday'] = pd.to_datetime(birthdays['Birthday'])
```

```console
  ValueError: month must be in 1..12
```

An error? It is to be expected though, since our dates are in multiple formats, especially
the weird `day/day/year` format which triggers an error with months.

To "solve" (in quotation marks because the data is still funky) this issue, we need to
specify values for two parameters - `infer_datetime_format=True` and `errors='coerce'`.
This will infer the format and return missing value for dates that could not be identified
and converted instead of a value error.

```python
  import pandas as pd

  # yay this works!
  birthdays['Birthday'] = pd.to_datetime(birthdays['Birthday'],
                                          # attempt to infer format of each date
                                          infer_datetime_format=True,
                                          # Return NA for rows where conversion failed
                                          errors='coerce')
```

Let us check the dates in the DataFrame!

```python
  birthdays.head()
```

```console
            Birthday  First Name  Last Name
  0              NaT       Rowan      Nunez
  1       2019-03-29       Brynn       Yang
  2       2019-03-03      Sophia     Reilly
  3       2019-03-24      Deacon     Prince
  4       2019-06-03    Griffith       Neal
```

This returns the `'Birthday'` column with aligned formats, with the initial ambiguous format
of `DD-DD-YY` being set to `NaT`, which represents missing values in Pandas for datetime
objects.

An alternative method to convert the format of a datetime column is to use the
`dt.strftime()` method, which accepts a datetime format as an argument. For instance, here
we convert the `'Birthday'` column to `DD-MM-YYYY`, instead of `YYYY-MM-DD`.

```python
  birthdays['Birthday'] = birthdays['Birthday'].dt.strftime('%d-%m-%Y')
  birthdays.head()
```

```console
            Birthday  First Name  Last Name
  0              NaT       Rowan      Nunez
  1       29-03-2019       Brynn       Yang
  2       03-03-2019      Sophia     Reilly
  3       24-03-2019      Deacon     Prince
  4       03-06-2019    Griffith       Neal
```

#### Ambiguous date data...?

A common (sub) problem is having ambiguous dates with vague formats. For example, is
`2019-03-08` in August or March?

Unfortunately, there is no clear-cut way to spot this inconsistency or to treat it.
Depending on the size of the dataset and suspected ambiguities, we can either:

- convert these dates into `NA` and deal with them accordingly,
- infer format by understanding the data source (ie. context of data), or
- infer format by understanding previous and subsequent data in DataFrame.

In general, it is extremely helpful to properly understand where your data comes from
before trying to treat it, as it will make making these decisions much easier.

## Cross Field Validation

### Motivation

More often than not, our data would have been collected and merged from different data
sources. A common challenge when merging data from different sources is data integrity, or
more broadly making sure that our data is consistent and correct.

### What Cross Field Validation is

Cross field validation is the use of *multiple* fields in a dataset to sanity check data
integrity.

Let us take a look at an example. In the DataFrame `users`, information about the user ID,
their age and their birthdays are stored.

```python
  users.head()
```

```console
      user_id  Age    Birthday
  0     32985   22  1998-03-02
  1     94387   27  1993-12-04
  2     34236   42  1978-11-24
  3     12551   31  1989-01-03
  4     55212   18  2002-07-02
```

We can, for example, make sure that the age and birthday columns are correct by subtracting
the number of years between today's date and each birthday.

To do this, we need to first make sure the `'Birthday'` column is converted to datetime with
`pd.to_datetime()` function.

```python
  import pandas as pd
  import datetime as dt

  users['Birthday'] = pd.to_datetime(users['Birthday'])
```

We then create an object storing today's date using the datetime package's
`dt.date.today()` function.

```python
  today = dt.date.today()
```

We then calculate the difference in years between today's date's year and the year of each
birthday by using the `.dt.year` attribute of the user's Birthday column.

```python
  age_manual = today.year - users['Birthday'].dt.year
```

We then find instances where the calculated ages are equal to the actual age column in
`users`. Lastly, we find and filter out the instances were we have inconsistencies through
subsetting with brackets and the tilde symbol on the equality Series we created.

```python
  age_equ = age_manual == users['Age']

  inconsistent_age = users[~age_equ]
  consistent_age = users[age_equ]
```

### After catching inconsistencies

So how should we deal with inconsistencies? Just like other data cleaning problems, the best
solution requires us to have an in-depth understanding of our dataset. We can decide to:

- drop inconsistent data
- set it to missing and impute it
- apply rules from domain knowledge
