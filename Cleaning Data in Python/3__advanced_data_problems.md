# Advanced Data Problems

## Uniformity

Unit uniformity is very important for accurate data analysis. Data can be represented by
different units. For instance, temperature can be recorded in both degrees Celsius and
degrees Fahrenheit.

### Example - Temperature Data

Let us take a look at an example. Assume that I have already imported temperature data into
a DataFrame `temperatures`. The dataset was collected from different sources with
temperature data in degrees Celsius and degrees Fahrenheit merged together. (If you are
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

To convert our temperature data, we isolate all rows of temperature column where it
is above 40 using the `.loc[]` method:

```python
  temp_fah = temperature.loc[temperature['Temperature'] > 40, 'Temperature']
  temp_cels = (temp_fah - 32) * (5/9)
  temperature.loc[temperature['Temperature'] > 40, 'Temperature'] = temp_cels
```

This snippet of code isolates all temperatures above 40 (why 40? Based on contextual
knowledge of NYC! 40 degrees Celsius is a common sense maximum for NYC.), and then
converts them into degrees Celsius by using the formula. Lastly, these values are
reassigned to their respective rows that contained Fahrenheit data.

Consistent with our previous approaches, we can always use the `assert` statement
to verify that conversion has been completed.

```python
  # if assertion passes, nothing is returned
  assert temperatures['Temperature'].max() < 40
```

### Example - Date Data
