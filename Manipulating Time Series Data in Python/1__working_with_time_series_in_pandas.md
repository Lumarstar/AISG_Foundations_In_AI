# Working with Time Series in Pandas

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
