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

We use the `plt.plot()` function to plot a
line graph. Here's an example:
```python
  import matplotlib.pyplot as plt
  
  year = [1950, 1970, 1990, 2010]
  pop_in_billions = [2.519, 3.692, 5.623, 6.972]

  # plot(x-axis, y-axis)
  plt.plot(year, pop)

  # plot customisation can go here!

  # this is to show the plot
  plt.show()
```

In this scenario, there are 4 data points. Using Python, it drew a line through!

## Scatter plot

We use the `plt.scatter()` function to plot a scatter plot. Using the same data as before:
```python
  import matplotlib.pyplot as plt
  
  year = [1950, 1970, 1990, 2010]
  pop_in_billions = [2.519, 3.692, 5.623, 6.972]

  # scatter(x-axis, y-axis)
  plt.scatter(year, pop)

  # plot customisation can go here!

  # this is to show the plot
  plt.show()
```

What Python will do here is to plot the data points only.