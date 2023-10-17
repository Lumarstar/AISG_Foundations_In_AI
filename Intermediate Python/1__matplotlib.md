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

![Scatter Plot of Population in billions against Year](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/14908006-6733-4969-8fe2-b99e4a53b965)

What Python will do here is to plot the data points only.

Scatter plots are useful to see correlations, whether they are positive or negative.
At this point, it is also useful to realise that we should not force correlations.
If there is no correlation between the data, then there simply is no correlation!

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
into how many bins the data should be divided. Based on `bins`, Matplotlib will
automatically find appropriate boundaries for all the bins in the histogram, and
calculate how many values are in each one.

Let's see an example:
```python
  values = [0, 0.6, 1.4, 1.6, 2.2, 2.5, 2.6, 3.2, 3.5, 3.9, 4.2, 6]
  plt.hist(values, bins=3)
  plt.show()
```

![Histogram of provided values](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/a4dfb670-aaba-4b8a-b795-7732ec839d1c)


Some real life applications of histograms are population pyramids! They "count" how many
people are within certain age ranges, so it fits the description of a histogram!


## Customisable Features

We need to call these customisation functions before `plt.show()`, if not these changes would not be reflected by the code.

### Axis Labels

```python
  plt.xlabel(x_label)    # x_label is a string which represents the x-axis label
  plt.ylabel(y_label)    # y_label is a string which represents the y-axis label
```

### Plot title

```python
  plt.title(title)    # title is a string which represents the plot title
```

### Plot Ticks

We can change the ticks for both the x-axis and the y-axis.

```python
  plt.xticks(list_of_ticks, list_of_display_names_of_ticks)
  plt.yticks(list_of_ticks, list_of_display_names_of_ticks)

  # example
  plt.xticks([1000, 10000, 100000], ['1k', '10k', '100k'])
  plt.yticks([0, 2, 4, 6, 8, 10], ["0", "2B", "4B". "6B". "8B", "10B"])
```

`list_of_ticks` and `list_of_display_names_of_ticks` should have the same length, as each index represents the same tick.

### Changing the sizes of data points

For **scatter plots**, we can change the size of the data points by specifying the `s` argument in `plt.scatter()`.

```python
  plt.scatter(values, bins=bins, s=s)
```

`values` and `bins` are explained as before. `s` is a list of the same length as `values`, with each index specifying
the size of the corresponding value in `values`.

To show the difference with and without specifying `s`, here is an example. `gdp_cap`, `pop`, and `life_exp` are available as lists.

1. Without any modification of `s`

```python
  plt.scatter(gdp_cap, life_exp)

  plt.xscale('log') 
  plt.xlabel('GDP per Capita [in USD]')
  plt.ylabel('Life Expectancy [in years]')
  plt.title('World Development in 2007')
  plt.xticks([1000, 10000, 100000],['1k', '10k', '100k'])
```

![Scatter Plot without modifying `s`](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/a53b3947-8ba9-4a3c-9c7f-fbb4fe685ad9)

2. Modifying `s`

```python
  plt.scatter(gdp_cap, life_exp, s=pop)

  plt.xscale('log') 
  plt.xlabel('GDP per Capita [in USD]')
  plt.ylabel('Life Expectancy [in years]')
  plt.title('World Development in 2007')
  plt.xticks([1000, 10000, 100000],['1k', '10k', '100k'])
```

![Scatter plot after modifying `s`](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/579c95f7-2677-4756-8e71-98d5f7b51853)

3. Modifying `s` more

```python
  # Import numpy as np
  import numpy as np

  # Store pop as a numpy array: np_pop
  np_pop = np.array(pop)
  
  # Double np_pop
  np_pop *= 2
  
  # Update: set s argument to np_pop
  plt.scatter(gdp_cap, life_exp, s = np_pop)
  
  # Previous customizations
  plt.xscale('log') 
  plt.xlabel('GDP per Capita [in USD]')
  plt.ylabel('Life Expectancy [in years]')
  plt.title('World Development in 2007')
  plt.xticks([1000, 10000, 100000],['1k', '10k', '100k'])

  # Display the plot
  plt.show()
```

![Scatter Plot with more obvious size changes](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/c523190b-d646-493f-9115-c11bf938f08b)

### Colours

We can change the colours of the plots by specifying the colour as a parameter.

Specifically, for scatter plots, we specify the parameter `c`, and provide a list of strings that denote what colour each data point should be.

To show an example, here we changed the colours of a previously shown scatter plot. `col` is a list which specifies colours.

```python
  plt.scatter(x = gdp_cap, y = life_exp, s = np.array(pop) * 2, c=col)

  plt.xscale('log') 
  plt.xlabel('GDP per Capita [in USD]')
  plt.ylabel('Life Expectancy [in years]')
  plt.title('World Development in 2007')
  plt.xticks([1000,10000,100000], ['1k','10k','100k'])
  
  # Show the plot
  plt.show()
```

![Scatter Plot with colours specified](https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/b72cb4c8-23ea-4188-ad79-a048f346e727)

### Transparency

To control the transparency of a plot, we specify the `alpha` parameter. `alpha` can take a value between 0 (completely transparent)
to 1 (completely opaque), inclusive of both endpoints.

### Text

We can also add text to the axes! We use the `plt.text()` function, like so:

```python
  plt.text(x, y, s)
```

This adds text `s` to the plot at location `(x, y)` *in the data coordinates*. (meaning, `x` and `y` are with reference to
the x-axis and y-axis.)

### Gridlines

So long as we specify, we can get gridlines to be drawn! We need to add `plt.grid(True)` for gridlines to be drawn on the plot.
We add this before `plt.show()` so that this change is reflected on the plot.

### Changing Scales

One thing we can do with plots is to change the scales they are on. Here are some examples
of different scales:

#### 1. Logarithmic Scales

We use `plt.xscale('log')` to put the x-axis on a logarithmic scale, and `plt.yscale('log')`
to put the y-axis on a logarithmic scale. As its name suggests, the x values will
first go through the logarithmic function, then be plotted. So rather than plotting
`y against x`, we are plotting `y against log(x)`.

### Miscellaneous functions

Matplotlib is an incredibly useful library! Here are some useful functions that will
likely help with data visualisation:

1. `plt.clf()`: cleans up the existing plot so we can start afresh with plots.

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

### Additional Questions to ponder

For each of these questions, either a line plot, scatter plot or histogram is most suitable. Think about why each plot is suitable

1. You're a professor teaching Data Science with Python, and you want to visually assess if the grades on your exam follow a particular 
distribution. Which plot do you use? (Answer: Histograms, because we are verifying a *distribution*)

2. You're a professor in Data Analytics with Python, and you want to visually assess if longer answers on exam questions lead to higher grades. 
Which plot do you use? (Answer: Scatter Plots, because we want to verify a *correlation*)
