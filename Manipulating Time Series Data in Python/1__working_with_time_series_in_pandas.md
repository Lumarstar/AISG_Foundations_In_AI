<img width="97" alt="image" src="https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/a09a0765-7dcc-411c-8717-6e8d3dc68246"><img width="261" alt="image" src="https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/366bca57-391d-4568-b4fe-05e466821984"># Working with Time Series in Pandas

## Using Dates and Times with Pandas

### Date and Time Series Functionality in Pandas

The key to date and time series functionality in Pandas - which is extremely useful to analyse
data that come as time series - are data types tailored to managing date and time information.

These data types represent either *points* in time or *periods* in time. They have attributes
and methods that allow you to access and manipulate the time dimension of your data.

Any column can contain date or time information, but it is the most important as a *DataFrame
index*, because this converts the entire DataFrame into a time series.

### Basic Building Block - `pd.Timestamp`

`pd.Timestamp` is one such data type. We can use the `pandas` library and the (built-in) 
`datetime` class to create a pandas `Timestamp`. We can also use a date string instead of
a datetime object and get the same result.

```python
  import pandas as pd
  from datetime import datetime

  time_stamp = pd.Timestamp(datetime(2017, 1, 1))    # using datetime object
  pd.Timestamp('2017-01-01') == time_stamp    # using date string
```

```console
  True
```

If we display the timestamp, we can see that the time has been automatically set to midnight.

```python
  time_stamp    # data type: pandas.islib.Timestamp
```

```console
  Timestamp('2017-01-01 00:00:00')
```

#### Attributes

The `Timestamp` object has many attributes to store time-specific information. For instance,
we can retrieve the year or the name of the weekday.

```python
  time_stamp.year
```

```console
  2017
```

```python
  time_stamp.day_name()
```

```console
  'Sunday'
```

### More building blocks - `pd.Period` and `.freq`

Pandas also has a data type for time periods - `pd.Period`. The `Period` object always has
a *frequency* attribute `.freq` to store frequency information, with months as the default.

```python
  period = pd.Period('2017-01')
  period    # default: month-end
```

```console
  Period('2017-01', 'M')
```

We can use the `.asfreq()` method to convert between frequencies, for instance from monthly
to daily frequency.

```python
  period.asfreq('D')    # convert to daily
```

```console
  Period('2017-01-31, 'D')
```

We can also convert between a `Timestamp` object and a `Period` object!

```python
  # to_timestamp() to convert to Timestamp object
  # to_period() to convert to Period object
  period.to_timestamp().to_period('M')
```

```console
  Period('2017-01', 'M')
```

### Basic Date Arithmetic

We can also do basic date arithmetic! Let us look at an example:

We start with a `Period` object for January 2017 at monthly frequency.

```python
  period = pd.Period('2017-01', 'M')
```

If we add 2 to `period`, we get a monthly period for March 2017.

```python
  period + 2
```

```console
  Period('2017-03', 'M')
```

This means that frequency information enables basic date arithmetic.

Time stamps can also have frequency information. If we create the timestamp for Jan 31 2017
with monthly frequency and add 1, we get a timestamp for February 28th.

```python
  pd.Timestamp('2017-01-31', 'M') + 1
```
  
```console
  Timestamp('2017-02-28 00:00:00', freq='M')
```

### Time Series

To create a time series, we need a *sequence of dates*. To create a sequence of Timestamps,
we can use the pandas function `pd.date_range()`, and specify:

1. a start date
2. either an end date or a number of periods
3. frequency (defaults to daily)

```python
  index = pd.date_range(start='2017-1-1', periods=12, freq='M')
```

The function returns the sequence of dates as a `pd.DatetimeIndex` object - a sequence of
`Timestamp` objects with frequency information.

```console
  ﻿DatetimeIndex(['2017-01-31', '2017-02-28', '2017-03-31', ...,
                  '2017-09-30', '2017-10-31', '2017-11-30', '2017-12-31'],
                  dtype='datetime64[ns]', freq='M')
```

The elements are pandas `Timestamp` objects!

```python
  index[0]
```

```console
  ﻿Timestamp ('2017-01-31 00:00:00', freq='M')
```

We can convert this index into a `PeriodIndex`, just like how we could convert between
`Timestamp` and `Period` objects.

```python
  index.to_period()
```

```console
  ﻿Period Index(['2017-01', '2017-02', '2017-03', '2017-04', ... '2017-11', '2017-12'],
  dtype='period [M]', freq='M')
```

Now we can create a time series by setting the `DateTimeIndex` as the index of our
DataFrame.

DataFrame columns containing dates will be assigned the `datetime64` datatype. (*ns* means
nanoseconds.)

```python
  pd.DataFrame({'data': index}).info()
```

```console
  ﻿RangeIndex: 12 entries, 0 to 11
  Data columns (total 1 columns):
  data    12 non-null datetime64[ns]
  dtypes: datetime64[ns](1)
```

Let us now create 12 rows with two columns of random data to match the `DateTimeIndex` using
`np.random.random()`. We also provide the dates to the DataFrame constructor (under `index`)
and we have our first time series with 12 monthly timestamps!

```python
  data = np.random.random((size=12, 2))
  pd.DataFrame(data=data, index=index).info()
```

```console
  ﻿DatetimeIndex: 12 entries, 2017-01-31 to 2017-12-31
  Freq: M
  Data columns (total 2 columns):
  0 12 non-null float64
  1 12 non-null float64
  dtypes: float64(2)
```

### Frequency alias and time information

Pandas allow us to create and convert between many different frequencies. Here are the most
important ones!

| Period  | Alias |
|---------|-------|
| Hour    | H     |
| Day     | D     |
| Week    | W     |
| Month   | M     |
| Quarter | Q     |
| Year    | Y     |

Some may also be set to the beginning or end of the period, or use business instead of
calendar periods.

There are also numerous Timestamp (`pd.Timestamp()`) attributes!

| Attribute                     |
|-------------------------------|
| .second, .minute, .hour       |
| .day, .month, .quarter, .year |
| .weekday                      |
| .dayofweek                    |
| .weekofyear                   |
| .dayofyear                    |

## Indexing and Resampling Time Series

### Time Series Transformation

We have many basic time series methods at our disposal to transform time series. These basic
methods include:

- Parsing string dates and converting them into `datetime64` data type
- selecting and slicing for specific subperiods of the time series
- setting or changing the `DateTimeIndex` frequencies.
  - upsampling: increasing the time-frequency. requires generating more data.
  - downsampling: decreasing the time-frequency. requires aggregating the data

### Example - Google Stock Prices

We don't have access to the source of the data here, but our data source supposedly has two
years worth of daily Google stock prices.

```python
  import pandas as pd
  google = pd.read_csv('google.csv')
  google.info()
```

```console
  ﻿<class 'pandas.core.frame.DataFrame'>
  RangeIndex: 504 entries, 0 to 503 Data columns (total 2 columns):
  date 584 non-null object
  price 504 non-null float64
  dtypes: float64(1), object(1)
```

Date data in real life are often of type `object`. Strings. We see this in our example here!
Our `date` column is of data type `object`. Yet, when we print a few rows of the data, we
see it contains dates.

```python
  google.head()
```

```console
  ﻿        date  price
  0 2015-01-02 524.81
  1 2015-01-05 513.87
  2 2015-01-06 501.96
  3 2015-01-07 501.10
  4 2015-01-08 502.68
```

To convert the strings to the correct data type, we use the `.to_datetime()` function.

```python
  google.date = pd.to_datetime(google.date)
  google.info()
```

```console
  
```

We can now set the typecasted column as the index using `.set_index()`.

```python
  google.set_index('date', inplace=True)
  google.info()
```

```console
  <class 'pandas.core.frame.DataFrame'>
  DatetimeIndex: 504 entries, 2015-01-02 to 2016-12-30
  Data columns (total 1 columns):
  price 504 non-null float64
  dtypes: float64(1)
```

The resulting `DateTimeIndex` lets us treat the entire DataFrame as time series data!

If we plot the stock price, it shows that Google has been doing well. More importantly, it
also shows that with a DateTimeIndex, pandas automatically creates reasonably spaced date
labels for the x-axis!

```python
  google.price.plot(title='Google Stock Price')
  plt.tight_layout()
  plt.show()
```

<img width="487" alt="image" src="https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/799e05bf-b142-4f17-8d25-ccb08854eae8">

#### Partial String Indexing

To select subsets of our time series, we can use strings that represent a complete date or
relevant parts of a date. This is what we call *partial string indexing*.

If we pass a string representing a year, pandas will return all dates within this year.

```python
  google['2015'].info()
```

```console
  ﻿DatetimeIndex: 252 entries, 2015-01-02 to 2015-12-31
  Data columns (total 1 columns):
  price     252 non-null float64
  dtypes: float64(1)
```

If we pass a slice that starts with one month and ends at another, we get all dates within
that range.

```python
  google['2015-3':'2016-2'].info()    # date slicing includes both endpoints
```

```console
  ﻿DatetimeIndex: 252 entries, 2015-03-02 to 2016-02-29
  Data columns (total 1 columns):
  price252     non-null float64
  dtypes: float64(1)
```

*We just need to note that the date range will be inclusive of the end dates, which is
different from other intervals in Python (like list slicing).*

We can also use `.loc[]` with a complete date and a column label to select a specific stock
price.

```python
  google.loc['2016-6-1', 'price']    # use full date with .loc[]
```

```console
  734.15
```

#### Setting frequencies

In our example, our DateTimeIndex did not have frequency information. We can (manually) set
it using `.asfreq()`.

```python
  google.asfreq('D').info()    # we set to calendar day frequency
```

```console
  ﻿DatetimeIndex: 729 entries, 2015-01-02 to 2016-12-30
  Freq: D
  Data columns (total 1 columns):
  price   504 non-null float64
  dtypes: float64(1)
```

As a result of us using the alias 'D' to set the frequency as calendar day frequency, our
DateTimeIndex now contains all the calendar dates between the two dates stated, even on all
the days where stock was not bought or sold. (*This sounds like upsampling is going to
happen... hmmm*)

These new dates have missing values. As we suspected before, this is what we call
*upsampling*, since the new DataFrame is of higher frequency than the original version.
We can verify that there is missing data by printing the first few rows of the DataFrame.

```python
  google.asfreq('D').head()
```

```console
             ﻿price
date
2015-01-02  524.81
2015-01-03  NaN
2015-01-04  NaN
2015-01-05  513.87
2015-01-06  501.96
```

We can also convert the `DateTimeIndex` to business day frequency using `.asfreq('B')`. Yes,
the alias for business day frequency is 'B'.

```python
  google = google.asfreq('B')
  google.info()
```

```console
  ﻿DatetimeIndex: 521 entries, 2015-01-02 to 2016-12-30
  Freq: B
  Data columns (total 1 columns):
  price    504 non-null float64
  dtypes: float64(1)
```

A smaller number of additional dates are created!

Additionally, we can use the `.isnull()` method to select the missing values and check which
dates are considered business days but have no stock prices because no stocks were traded.

```python
  google[google.price.isnull()]    # select all missing `price` values
```

```console
          ﻿price
date
2015-01-19  NaN
2015-02-16  NaN
2016-11-24  NaN
2016-12-26  NaN
```
