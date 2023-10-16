# Matplotlib

## Configuring Matplotlib

### Install Matplotlib

To install Matplotlib, we go to our terminal and key in this command:
```console
  > pip install matplotlib
```

### Using Matplotlib

To use Matplotlib to visualise data, we need its subpackage `pyplot`. By convension, this
subpackage is imported as `plt`, like so:
```python
  import matplotlib.pyplot as plt
```

## Line Plots

We use the `plt.plot()` function to plot a line graph. Here's an example:
```python
  import matplotlib.pyplot as plt
  
  year = [1950, 1970, 1990, 2010]
  pop_in_billions = [2.519, 3.692, 5.623, 6.972]

  # plot(x-axis, y-axis)
  plt.plot(year, pop_in_billions)

  # plot customisation can go here!

  # this is to show the plot
  plt.show()
```

![Line Plot of Population in billions against Year](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/28e24d65-b03b-48b3-b581-d3b29d5bed2f)

In this scenario, there are 4 data points. Using Python, it drew a line through!

## Scatter plot

We use the `plt.scatter()` function to plot a scatter plot. Using the same data as before:
```python
  import matplotlib.pyplot as plt
  
  year = [1950, 1970, 1990, 2010]
  pop_in_billions = [2.519, 3.692, 5.623, 6.972]

  # scatter(x-axis, y-axis)
  plt.scatter(year, pop_in_billions)

  # plot customisation can go here!

  # this is to show the plot
  plt.show()
```

Scatter plots are useful to see correlations, whether they are positive or negative.
At this point, it is also useful to realise that we should not force correlations.
If there is no correlation between the data, then there simply is no correlation!

## Customisable Features

### Changing Scales

One thing we can do with plots is to change the scales they are on. Here are some examples
of different scales:

#### 1. Logarithmic Scales

We use `plt.xscale('log')` to put the x-axis on a logarithmic scale, and `plt.yscale('log')`
to put the y-axis on a logarithmic scale. As its name suggests, the x values will
first go through the logarithmic function, then be plotted. So rather than plotting
`y against x`, we are plotting `y against log(x)`.

![Scatter Plot of Population in billions against Year](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/14908006-6733-4969-8fe2-b99e4a53b965)

What Python will do here is to plot the data points only.

## Histogram

Histograms are visualisations that are useful for exploring datasets. It gives an idea
about the **distribution of the data**.

To use Matplotlib to plot histograms, we follow these steps:
```python
  import matplotlib.pyplot as plt

  # bins is an optional argument. if not provided, `bins = 10` by default.
  plt.hist(x[, bins])
```
`x` is the list of values we want to build a histogram for; `bins` is to tell Python
into how mnay bins the data should be divided. Based on `bins`, Matplotlib will
automatically find appropriate boundaries for all the bins in the histogram, and
calculate how mnay values are in each one.

Let's see an example:
```python
  values = [0, 0.6, 1.4, 1.6, 2.2, 2.5, 2.6, 3.2, 3.5, 3.9, 4.2, 6]
  plt.hist(values, bins=3)
  plt.show()
```

![Histogram of provided values](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/a4dfb670-aaba-4b8a-b795-7732ec839d1c)


Some real life applications of histograms are population pyramids! They "count" how many
people are within certain age ranges, so it fits the description of a histogram!

## Food for thought

We need to know when to use which type of plots! Some data are suited for line plots,
others... not so much. For example, when we plot 2 lists `gdp_cap` and `life_exp` against
each other using a line plot, we get... very interesting results:
```python
  # for the sake of keeping things succinct, not all data is shown here.
  # the complete set of data can be found in DataCamp.
  gdp_cap = [974.5803384, 5937.029525999999, 6223.367465, ... , 469.70929810000007]
  life_exp = [43.828, 76.423, 72.301, ... , 43.487]

  import matplotlib.pyplot as plt
  plt.plot(gdp_cap, life_exp)
  plt.show()
```

![Line Plot of Life Expectancy for each country against their GDP per capita](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/f51da1f7-83b1-4de5-9ed0-3f9d28e2ae40)

as compared to when we use a scatter plot (and change the scale of the plot):
```python
  plt.scatter(gdp_cap, life_exp)
  plt.xscale('log')
  plt.show()
```

![Scatter plot of Life Expectancy against GDP per capita (with log scale)](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/48f62d75-d24a-4192-a5c4-7d5389bff5cd)
