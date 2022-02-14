# More With `matplotlib`

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

This lab covers how to generate `matplotlib` plots for data stored in a `pandas` `DataFrame`. It provides an overview of how to generate a variety of common plot types, including line plots, bar charts, histograms, box plots, area plots, scatter plots, and pie charts. It also covers how `pandas`'s plotting function handles missing data. It provides a comparison of the `matplotlib` and `seaborn` plotting packages and provides an introduction to `seaborn` with sample code. 

By the end of this lab, students will be able to:
- Understand how to generate `matplotlib` plots for data stored in a `pandas` `DataFrame`, for a variety of plot types
- Understand how to navigate the `pandas` documentation to troubleshoot and further explore `pandas`'s plotting functions
- Understand the basic distinctions and relationship between `matplotlib` and `seaborn`

## Acknowledgements

The author consulted the following resources when writing  this tutorial:
- Chapter 4.14 "Visualization With Seaborn" from Jake VanderPlas, [*Python Data Science Handbook: Essential Tools for Working with Data*](https://jakevdp.github.io/PythonDataScienceHandbook/04.14-visualization-with-seaborn.html ) (O'Reilly, 2016)
- `pandas`, [User Guide, "Visualization"](https://pandas.pydata.org/docs/user_guide/visualization.html)
- `pandas`, [Getting Started, "Plotting"](https://pandas.pydata.org/docs/getting_started/intro_tutorials/04_plotting.html)
- Chapter 9 "Plotting and Visualization" from Wes McKinney, [*Python for Data Analysis: Data Wrangling With pandas, Numpy, and IPython*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2017)
- Chapter 15 "Generating Data" from Eric Matthes, [*Python Crash Course: A Hands-On, Project-Based Introduction to Programming*](https://ehmatthes.github.io/pcc/) (No Starch Press, 2019).
- [`seaborn` package documentation](https://seaborn.pydata.org/introduction.html)

# Table of Contents

- [Lab notebook template](#lab-notebook-template)
- [Data](#data)
- [Setup and Environment](#setup-and-environment)
- [`pandas` and `matplotlib`](#pandas-and-matplotlib)
  * [Plotting in `pandas` Using `.plot()`](#plotting-in-pandas-using-plot)
    * [Time Series Data and Line Plots](#time-series-data-and-line-plots)
    * [Bar Charts](#bar-charts)
      * [Grouped Bar Charts](#grouped-bar-charts)
      * [Stacked Bar Charts](#stacked-bar-charts)
      * [Horizontal Bar Charts](#horizontal-bar-charts)
    * [Histograms](#histograms)
    * [Box Plots](#box-plots)
    * [Area Plots](#area-plots)
    * [Scatter Plots](#scatter-plots)
    * [Pie Charts](#pie-charts)
    * [Mapping](#mapping)
  * [Lab Notebook Question 1](#lab-notebook-question-1)
  * [`.plot()` and Missing Data](#plot-and-missing-data)
- [Working with `pandas` and `seaborn`](#working-with-pandas-and-seaborn)
  * [Lab Notebook Question 2](#lab-notebook-question-2)
- [Additional Resources for Statistical Analysis and Machine Learning](#additional-resources-for-statistical-analysis-and-machine-learning)
- [Lab Notebook Questions](#lab-notebook-questions)

[Link to lab procedure as a Jupyter Notebook](https://drive.google.com/file/d/1tq9fJX9oX_5cL1stEHRpMMmoNL573f9_/view?usp=sharing)

# Lab Notebook Template

[Link to lab notebook template (Jupyter Notebook)](https://drive.google.com/file/d/1qUT3e8D02oABf6DBjMEm6sb7thhk7wIR/view?usp=sharing)

# Data

Most of this lab will work with the `air_quality_no2.csv` dataset:
- [Download via Google Drive](https://drive.google.com/file/d/1q9np3jZycdHszWODH-MCQ4rfjkcH3g6F/view?usp=sharing)

To load this data in Python:

```Python
# import pandas
import pandas as pd

# load data from file
air_quality = pd.read_csv('air_quality_no2.csv', index_col=0, parse_dates=True)
```

```Python
# import pandas
import pandas as pd

# load data from url
air_quality = pd.read_csv('https://raw.githubusercontent.com/kwaldenphd/more-with-matplotlib/main/data/air_quality_no2.csv', index_col=0, parse_dates=True)
```

The mapping section of the lab will use georeferenced data on Notre Dame Football schedules, scraped from College Football Reference.
- [Download via Google Drive](https://drive.google.com/file/d/1zmEO_iDfGv1ciwTdQf7JKCZaCzx0Bk1C/view?usp=sharing)

To load this data in Python:

```Python
# import statements
import pandas as pd

# load data from file
football = pd.read_csv("nd_football.csv")
```

```Python
# import statements
import pandas as pd

# load data from url
football = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/more-with-matplotlib/main/data/nd_football.csv")
```

# Setup and Environment

## GeoPandas

Later in this lab, we'll be using a package called `GeoPandas` for plotting spatial data.

Installing and configuring `Geopandas` requires creating a new Python environment.

A few resources that can get folks started:
  
**Installing and Configuring `geopandas`**:
- Anaconda
  * Tanish Gupta, "[Fastest Way to Install Geopandas in Jupyter Notebooks](https://medium.com/analytics-vidhya/fastest-way-to-install-geopandas-in-jupyter-notebook-on-windows-8f734e11fa2b)" *Analytics Vidhya* (6 December 2020)
  * Anaconda, "[conda-forge packages, geopandas](https://anaconda.org/conda-forge/geopandas)" *Anaconda documentation*
  * GeoPandas, "[Installation](https://geopandas.org/getting_started/install.html)" *GeoPandas documentation*
- Google CoLab
  * Abdishakur Hassan, Jupyter notebook on using `geopandas` in Google CoLab, from "[Geographic data science tutorials with Python](https://github.com/shakasom/GDS)" *GitHub repository*
    * [Google CoLab](https://colab.research.google.com/github/shakasom/GDS/blob/master/Part1%20-%20Introduction.ipynb)
    * [GitHub](https://github.com/shakasom/GDS/blob/master/Part1%20-%20Introduction.ipynb)

Additional `GeoPandas` resources:
- Jonathan Soma, "[Mapping with geopandas](https://jonathansoma.com/lede/foundations-2017/classes/geopandas/mapping-with-geopandas/)" from 2017 "[Foundations of Computing](https://jonathansoma.com/lede/foundations-2017/)" course, Columbia Graduate School of Journalism
- CoderzColumn, "[Plotting Static Maps with geopandas](https://coderzcolumn.com/tutorials/data-science/plotting-static-maps-with-geopandas-working-with-geospatial-data)" *CoderzColumn* (11 March 2020)
- GeoPandas, "[Plotting with Geoplot and GeoPandas](https://geopandas.org/gallery/plotting_with_geoplot.html)" *GeoPandas documentation*

## Geocoding Data

Later in this lab we'll look at plotting geospatial data to generate maps.

But if you're wanting to work with your own data and it does not have geospatial attributes (latitude and longitude), you'll need to add that information via a process called geocoding.

Free online geocoding services:
- [LocalFocus data journalism batch geocoder](https://geocode.localfocus.nl/)
- [Texas A&M Geocoding Services](https://geoservices.tamu.edu/Services/Geocode/)
  * *Requires creating a free account*
  
There are also Python libraries that support geocoding.
- [`GeoPy`](https://geopy.readthedocs.io/en/stable/)
- [`Geocoder`](https://geocoder.readthedocs.io/providers/Mapbox.html) (requires a free Mapbox API key)
- Abdishakur, ["Geocode with Python"](https://towardsdatascience.com/geocode-with-python-161ec1e62b89) *Towards Data Science* (15 September 2019)

# `pandas` and `matplotlib`

1. Having to load data manually to build a visualization or plot gets cumbersome quickly.

2. In many situations, we might want to work with data in a `pandas` `DataFrame` when building a visualization.

3. `pandas` includes a `.plot()` attribute that interacts with the `matplotlib` API to generate plots.

4. The `pandas` `.plot()` attribute relies on the `matplotlib` API to generate plots, so our work with `matplotlib` will come in handy when we need to customize plots generated using `.plot()`.

5. And in many cases, the `.plot()` syntax is similar to `matplotlib` `OO` syntax.

## Plotting in `pandas` Using `.plot()`

6. Let's go back to the air quality data we were working with previously in `pandas`.

7. To load the data as a dataframe:
```Python
# import pandas
import pandas as pd

# load data from url to dataframe
# air_quality = pd.read_csv('https://raw.githubusercontent.com/kwaldenphd/more-with-matplotlib/main/data/air_quality_no2.csv', index_col=0, parse_dates=True)

# check data has loaded
air_quality.head()
```

8. We can do a quick visual check of the data by passing the entire data frame to `.plot()`.
```Python
# import matplotlib
import matplotlib.pyplot as plt

# generate plot
air_quality.plot()

# show plot
plt.show()
```

9. This isn't a particularly meaningful visualization, but it shows us how the default for `.plot()` creates a line for each column with numeric data.

10. The index is used for the `X` axis, and all numeric columns are plotted on the `Y` axis.

11. We can also see that `.plot()` pulls tick marks, tick labels, and axis titles from the underlying `dataframe`.

12. Let's say we only wanted to plot Paris data.

13. We can select that column in the `dataframe` before calling `.plot()`.
```Python
air_quality["station_paris"].plot()
```

14. We can plot a specific column in the `dataframe` using the `[" "]` selection method.

15. Let's say we want to visually compare NO<sub>2</sub> values measured in London and Paris.

16. We need to specify what column is going to be used for the `X` axis as well as what column is going to be used for the `Y` axis.

17. For this example, a scatterplot will be more effective than a lineplot.

18. We can create a scatterplot using `.plot.scatter()`.

```Python
air_quality.plot.scatter(x="station_london", y="station_paris", alpha=0.5)
```

19. This example generates a scatterplot with London data on the `X` axis and Paris data on the `Y` axis.

20. While the default for `.plot()` is a lineplot, there are a number of other methods we can use with `.plot()`.

21. Again, you will see some overlap with `matplotlib` syntax.

22. We can use these strings as as method in combination with `.plot()` or we can pass them to `.plot()` as a `kind` parameter.

Plot Type | Method Syntax | Parameter Syntax
--- | --- | ---
Default (line) | `.plot()` | NA
Area | `.plot.area()` | `.plot(kind='area')`
Bar | `.plot.bar()` | `.plot(kind='bar')`
Horizontal bar | `.plot.barh()` | `.plot(kind='barh')`
Box | `.plot.box()` | `.plot(kind='box')`
Density | `.plot.density()` | `.plot(kind='density')`
Hexbin | `.plot.hexbin()` | `.plot(kind='hexbin')`
Histogram | `.plot.hist()` | `.plot(kind='hist')`
Kernel density | `.plot.kde()` | `.plot(kind='hist')`
Line | `.plot.line()` | `.plot(kind='line')` *this would be redundant since the default for `.plot()` is line*
Pie | `.plot.pie()` | `.plot(kind='pie')`
Scatter | `.plot.scatter()` | `.plot(kind='scatter')`

23. In general, it's more effective to use the method syntax to note plot type, rather than treating plot type as a parameter.

24. Let's create a box plot using our air quality data.
```Python
air_quality.plot.box()
```

25. We can see each numeric column (each station) has its own box in the default boxplot.

26. Let's say we wanted to generate a lineplot with separate subplots for each of the numeric columns.

27. We can accomplish this by setting the `subplots` parameter to `True`.

```Python
axs = air_quality.plot.area(figsize=(12, 4), subplots=True)
```

28. Let's say we want to further customize this plot.

29. This is where we start to see more `matplotlib` syntax in play.

```Python
# create figure and axes
fig, axs = plt.subplots(figsize=(12, 4))

# build area plot
air_quality.plot.area(ax=axs)

# set y axis label
axs.set_ylabel("NO$_2$ concentration")

# save plot as png file
fig.savefig("no2_concentrations.png")
```

30. Each plot object created by `pandas` is also a `matplotlib` object.

31. Having a deep knowledge of `matplotlib` allows you to customize plots generated using `pandas` `.plot()` function.

32. That said, there are some parameters you can set as part of `.plot()` without having to have separate `matplotlib` syntax commands.

33. The plot `kind` is one parameter option, as is the `dataframe`, `X` axis data, and `Y` axis data.

34. Other parameters:

Parameter | Explanation
--- | ---
`ax` | `matplotlib` `axes` object; axes for the current figure
`subplots` | Default is `False`; set to `True` to enable multiple plots in the same figure
`layout` | Follows `(rows, columns)` syntax to set subplot layout
`figsize` | Follows `(width, height)` syntax; Sets figure object width and height in inches
`use_Index` | Default is `True`; uses dataframe index as ticks for `X` axis
`title` | Title to use for the plot; can take a list with titles for corresponding subplots
`grid` | Default is `False`; set to `True` to show axis grid lines
`legend` | Places legend on axis subplot
`xticks` | Values to use for `X` axis ticks
`yticks` | Values to use for `Y` axis ticks
`xlim` | Follows `(lower limit, upper limit)` syntax; Sets `X` axis limits
`ylim` | Follows `(lower limit, upper limit)` syntax; Sets `Y` axis limits
`xlabel` | Label for `X` axis; default uses index column name
`ylabel` | Label for `Y` axis; default uses `Y` column name
`fontsize` | Sets font size for tickmarks
`colormap` | Sets `matplotlib` colormap to select colors from
`table` | Default is `False`; set to `True` to draw a table from data in the `DataFrame`
`stacked` | Default is `False` in line and bar plots, `True` in area plot; if `True`, creates stacked plot

35. For more parameters that can be passed to `.plot()`: [`pandas.DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html)

36. Let's walk through a few more examples of using `.plot()` with a `pandas` `DataFrame`.

37. NOTE: The `.plot()` method can be used on `pandas` `Series` and `DataFrame`. These examples focus on `DataFrame`s.

### Time Series Data and Line Plots

38. Let's generate some random time series data.
```Python
# import pandas
import pandas as pd

# import matplotlib
import matplotlib.pyplot as plt

# import numpy
import numpy as np

# create series with random numbers organized over time series
ts = pd.Series(np.random.randn(1000), index=pd.date_range("1/1/2000", periods=1000))

# cumulative sum calculated by iterating over Series
ts = ts.cumsum()

# generate line plot
ts.plot()
```

39. In this example, our `Series` index labels are specified using `pd.date_range()`.

40. These are the `X` axis values.

41. The index values are the 1000 random values generated by `np.random.randn()`.

42. We use the cumulative sum `.cumsum` to determine the total value for each unique time in the series.

43. Let's modify this example to use a `DataFrame`.
```Python
# create new dataframe with random numbers, the original time series index, and four columns
df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index, columns=list("ABCD"))

# calculate cumulative sum
df = df.cumsum()

# create plot
df.plot()
```

44. For more on line plots:
- [`pandas`, "Basic plotting"](https://pandas.pydata.org/docs/user_guide/visualization.html#basic-plotting-plot)
- [`pandas.DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html)
- [`pandas.DataFrame.plot.line`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.line.html)

### Bar Charts

45. Let's say we want to create a bar chart with a single line of data from our `df` `DataFrame`.

46. We can select a specific row using `iloc`.

```Python
# create figure
plt.figure()

# create bar plot for single row of data
df.iloc[5].plot.bar()

# show plot
plt.show()
```

#### Grouped Bar Charts

47. Let's say we want to produce a grouped bar chart.

48. We can create a `DataFrame` with four columns and random values.

```Python
# create dataframe
df2 = pd.DataFrame(np.random.randn(10, 4), columns=['a', 'b', 'c', 'd'])

# generate bar chart
df2.plot.bar()
```

49. By default, `.plot.bar()` creates a separate bar for each of the numeric columns.

50. We could select a single column using `['']` to only show one bar or column value.

#### Stacked Bar Charts

51. If we wanted to show this data as a stacked bar chart, we would set the `stacked` parameter to `True`.
```Python
df2.plot.bar(stacked=True)
```

#### Horizontal Bar Charts

52. Remember `.plot.barh()` generates a horizontal bar chart, and we can also set `stacked` to `True` here to create a horizontal stacked bar chart.
```Python
df2.plot.barh(stacked=True)
```

53. For more on bar charts:
- [`pandas`, "Bar plots"](https://pandas.pydata.org/docs/user_guide/visualization.html#bar-plots)
- [`pandas.DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html)
- [`pandas.DataFrame.plot.bar`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.bar.html)
- [`pandas.DataFrame.plot.barh`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.barh.html)

### Histograms

54. The `.plot.hist()` method will generate a histogram.

55. We can also use `.hist()` to generate a histogram.

```Python
# create dataframe with three columns using dictionary with random number data
df = pd.DataFrame({"a": np.random.randn(1000) + 1, "b": np.random.randn(1000), "c": np.random.randn(1000) - 1,}, columns=["a", "b", "c"],)

# create figure object
plt.figure()

# create histogram
df4.plot.hist(alpha=0.5)
```

56. We can set `stacked` to `True` to create a stacked histogram.
```Python
df4.hist(stacked=True)
```

57. We can also specify the bin size using the `bins` keyword.
```Python
df4.plot.hist(bins=20)
```

58. We can use the `.hist()` method in `matplotlib` to further customize our histogram: [`matplotlib.axes.Axes.hist`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.hist.html#matplotlib.axes.Axes.hist)

59. For more on histograms:
- [`pandas`, "Visualization, Histograms"](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization-hist)
- [`pandas.DataFrame.plot.hist`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.hist.html)
- [`pandas.DataFrame.hist`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.hist.html)

### Box plots

60. We can generate box plots using `.plot.box()` or `.boxplot()`.

61. The default settings visualize the distribution of values within each column.

62. Back to our random number `DataFrame`, this time with five columns.

```Python
# create dataframe
df = pd.DataFrame(np.random.randn(10, 5), columns=['A', 'B', 'C', 'D', 'E'])

# create plot
df.plot.box()
```

63. We can add colors to our box plot using the `color` keyword.
```Python
df.plot.box(color='blue')
```

64. We can also use a dictionary with key-value pairs for each component of our box plot.
```Python
# set color dictionary
color = {"boxes": "DarkGreen", "whiskers": "DarkOrange", "medians": "DarkBlue", "caps": "Gray",}

# draw plot and specify colors and outlier symbol using keyword argument or kwarg
df.plot.box(color=color, sym="r+")
```

65. For more on box plots:
- [`pandas`, "Visualization, Box plots"](https://pandas.pydata.org/docs/user_guide/visualization.html#box-plots)
- [`pandas.DataFrame.plot.box`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.box.html)
- [`pandas.DataFrame.boxplot`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.boxplot.html)

### Area Plots

66. Basic syntax:
```Python
df = pd.DataFrame(np.random.rand(10, 4), columns=["a", "b", "c", "d"])

df.plot.area()
```

67. For more on area plots:
- [`pandas`, "Visualization, Area plot"](https://pandas.pydata.org/docs/user_guide/visualization.html#area-plot)
- [`pandas.DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html)
- [`pandas.DataFrame.plot.area`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.area.html)

### Scatter Plots

68. Basic syntax:
```Python
df = pd.DataFrame(np.random.rand(50, 4), columns=["a", "b", "c", "d"])

df.plot.scatter(x="a", y="b")
```

69. For more on scatter plots:
- [`pandas`, "Visualization, Scatter plot"](https://pandas.pydata.org/docs/user_guide/visualization.html#scatter-plot)
- [`pandas.DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html)
- [`pandas.DataFrame.plot.scatter`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.scatter.html)

### Pie Charts

70. Basic syntax:
```Python
series = pd.Series(3 * np.random.rand(4), index=["a", "b", "c", "d"], name="series")

series.plot.pie(figsize=(6, 6))
```

71. For more on pie plots:
- [`pandas`, "Visualization, Pie Plot"](https://pandas.pydata.org/docs/user_guide/visualization.html#pie-plot)
- [`pandas.DataFrame.plot.pie`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.pie.html#pandas.DataFrame.plot.pie)

### Mapping

72. Up to this point, we have been working with data plotted on a 2D cartesian coordinate system, with x and y axes.

73. For our purposes, it's most useful to think of maps in the same way--as data plotted on a coordinate system.

74. Except for maps, that coordinate system is typically some type of latitude or longitude based projection, and the data to be plotted includes explicit location information (rather than a numerical or categorical field that can be mapped to an axis).

75. We can use `GeoPandas` (a library based on `Pandas` that is geared toward geospatial data) in combination with `Shapely` and `Matplotlib` to generate static maps in Python.

76. First- we need data that has geospatial attributes. Most often, this is some kind of `Latitude` and `Longitude` field.

77. If our data includes addresses or city/state/country information, there are free online services that we can use to geocode data (add latitude/longitude information).

**Free online geocoding services:**
- [LocalFocus data journalism batch geocoder](https://geocode.localfocus.nl/)
- [Texas A&M Geocoding Services](https://geoservices.tamu.edu/Services/Geocode/)
  * *Requires creating a free account*
  
78. There are also Python libraries that support geocoding.
- [`GeoPy`](https://geopy.readthedocs.io/en/stable/)
- [`Geocoder`](https://geocoder.readthedocs.io/providers/Mapbox.html) (requires a free Mapbox API key)
- Abdishakur, ["Geocode with Python"](https://towardsdatascience.com/geocode-with-python-161ec1e62b89) *Towards Data Science* (15 September 2019)
  
79. Installing and configuring `Geopandas` requires creating a new Python environment.

80. A few resources that can get folks started:
  
**Installing and Configuring `geopandas`**:
- Anaconda
  * Tanish Gupta, "[Fastest Way to Install Geopandas in Jupyter Notebooks](https://medium.com/analytics-vidhya/fastest-way-to-install-geopandas-in-jupyter-notebook-on-windows-8f734e11fa2b)" *Analytics Vidhya* (6 December 2020)
  * Anaconda, "[conda-forge packages, geopandas](https://anaconda.org/conda-forge/geopandas)" *Anaconda documentation*
  * GeoPandas, "[Installation](https://geopandas.org/getting_started/install.html)" *GeoPandas documentation*
- Google CoLab
  * Abdishakur Hassan, Jupyter notebook on using `geopandas` in Google CoLab, from "[Geographic data science tutorials with Python](https://github.com/shakasom/GDS)" *GitHub repository*
    * [Google CoLab](https://colab.research.google.com/github/shakasom/GDS/blob/master/Part1%20-%20Introduction.ipynb)
    * [GitHub](https://github.com/shakasom/GDS/blob/master/Part1%20-%20Introduction.ipynb)

81. We'll spend more time with mapping when we work with interactive visualization.

82. But we can explore some static maps generated using `GeoPandas`, `Shapely`, and `Matplotlib`.

83. For these maps, we'll be using , but some sample code that works with `shapely`, `geopandas`, and `matplotlib`.

84. The data used for these maps is geocoded Notre Dame football schedule information scraped from College Football Reference.
- [Link to data scraping Jupyter Notebook](https://github.com/kwaldenphd/football-structured-data/blob/main/notebooks/sf-nd-schedules.ipynb)
- [Link to explore the data](https://github.com/kwaldenphd/more-with-matplotlib/blob/main/data/nd_football.csv)

85. We can create a scatterplot using the latitude and longitude data.

```Python
# import statements
import pandas as pd

# load data
# football = pd.read_csv("https://raw.githubusercontent.com/kwaldenphd/more-with-matplotlib/main/data/nd_football.csv")

# scatterplot of latitude and longitude data
football.plot(x= "Longitude", y="Latitude", kind='scatter')
```

86. We can also explore what types of basemap layers are available through `GeoPandas`.

```Python
# show available geopandas datasets (for basemaps)
geopandas.datasets.available

# world basemap from naturalearth_lowres geopandas dataset
world = gpd.read_file(geopandas.datasets.get_path("naturalearth_lowres"))

# show basemap head
world.head()
```

87. In order to create a map from the football data, we would need to create a point field from the latitude/longitude data.

```Python
# function that takes latitude and longitude columns from dataframe and creates Point field
def make_point(row):
    return Point(row.Longitude, row.Latitude)

points = football.apply(make_point, axis=1)

# create GeoDataFrame from football data and points geometry
football_map = gpd.GeoDataFrame(football, geometry=points)

# set GeoDataFrame coordinate system
football_map.crs = {'init': 'epsg:4326'}

# show head of GeoDataFrame
football_map.head()
```

88. Then, we could generate a preliminary Cartesian Coordinate plot of the GeoDataFrame.

```Python
# preliminary cartesian coordinate plot of GeoDataFrame
football_map.plot(figsize=(20,5))
```

89. From that point, we can add a basemap layer to create a static 2D map.

```Python
# create figure axes with world basemap
ax = world.plot(figsize=(15, 5), linewidth=0.25, edgecolor="white", color="lightgrey")

# set axes title
ax.set_title("Geography of Notre Dame Football")

# configure axes
ax.axis('off')

# plot football data with points colored by season
football_map.plot(markersize=10, column="Season", cmap='viridis', alpha=0.5, ax=ax, legend=True)
```

90. Additional `GeoPandas` resources:
- Jonathan Soma, "[Mapping with geopandas](https://jonathansoma.com/lede/foundations-2017/classes/geopandas/mapping-with-geopandas/)" from 2017 "[Foundations of Computing](https://jonathansoma.com/lede/foundations-2017/)" course, Columbia Graduate School of Journalism
- CoderzColumn, "[Plotting Static Maps with geopandas](https://coderzcolumn.com/tutorials/data-science/plotting-static-maps-with-geopandas-working-with-geospatial-data)" *CoderzColumn* (11 March 2020)
- GeoPandas, "[Plotting with Geoplot and GeoPandas](https://geopandas.org/gallery/plotting_with_geoplot.html)" *GeoPandas documentation*

## Lab Notebook Question 1

Q1: Build at least three different types of plots using matplotlib and data stored in a Pandas DataFrame. Include code + comments. 
- *I encourage folks to use this question to explore visualizations you might use for the final project.*

For each plot, include the following elements or components:
- Title
- Axis labels
- Legend
- Scale or tickmarks
- Data source
 
Plot types to choose from:
- Line plots
- Bar chart
- Grouped bar chart
- Horizontal bar chart
- Stacked bar chart
- Histogram
- Box plot
- Area plot
- Scatter plot
- Pie chart

## `.plot()` and Missing Data

91. Just like `pandas` has built-in settings for handling missing data or `NaN` values, the `.plot()` method also has default settings for how each type of plot handles missing data.

Plot Type | NaN Handling 
--- | ---
Line | Leaves gaps
Stacked line | Fills 0s
Bar | Fills 0s
Scatter | Drops missing values
Histogram | Drops missing values (column-wise)
Box | Drops missing values (column-wise)
Area | Fills 0s
Kernel density (KDE) | Drops missing values (column-wise)
Hexbin | Drops missing values
Pie | Fills 0s

92. To customize or alter these default settings, you would use `.fillna()` or `.dropna()` to filter the `DataFrame` before generating the plot.

## Additional Resources

93. For more on plotting with `pandas` and `matplotlib`:
- [`pandas`, Tutorials, "Plotting"](https://pandas.pydata.org/docs/getting_started/intro_tutorials/04_plotting.html)
- [`pandas`, "Plotting tools"](https://pandas.pydata.org/docs/user_guide/visualization.html#plotting-tools)
- [`pandas`, "Plot formatting"](https://pandas.pydata.org/docs/user_guide/visualization.html#plot-formatting)
- [`pandas`, "Plotting directly with matplotlib"](https://pandas.pydata.org/docs/user_guide/visualization.html#plotting-directly-with-matplotlib)

# Working with `pandas` and `seaborn`

94. As we've covered previously, `matplotlib` gives you a wide range of base components to work with when generating plots.

95. But, `matplotlib` does have limitations, and building a plot from the ground up, specifying each component can be cumbersome (and result in significant boilerplate code).

96. Advanced statistical analysis with `matplotlib` is possible, but cumbersome.

97. If you need to engage in advanced statistical analysis beyond what is easily accessible in `matplotlib`, `seaborn` is a statistical data visualization library that works well with `pandas`.

98. "`seaborn` is a Python data visualization library based on matplotlib. It provides a high-level interface for drawing attractive and informative statistical graphics" (["Seaborn"](https://seaborn.pydata.org/).

99. `seaborn` works with `numpy`, `scipy`, `pandas`, and `matplotlib` to simply high-level functions for common statistical plots.

100. Let's compare `seaborn` and `matplotlib` using random walk data and a line plot.

101. Sample line plot generated using only `matplotlib`:
```Python
# import necessary packages
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# create random walk data
rng = np.random.RandomState(0)
x = np.linspace(0, 10, 500)
y = np.cumsum(rng.randn(500, 6), 0)

# create figure
fig, ax = plt.subplots()

# generate line plot
ax.plot(x, y)

# create legend
ax.legend('ABCDEF', ncol=2, loc='upper left')

# show plot
plt.show()
```

102. An okay plot, but not particularly effective for this data.

103. Let's generate the same plot with `seaborn` running on top of `matplotlib`.
```Python
# import seaborn package
import seaborn as sns

# set seaborn  style
sns.set()

# create figure
fig, ax = plt.subplots()

# generate line plot
ax.plot(x, y)

# create legend
ax.legend('ABCDEF', ncol=2, loc='upper left')

# show plot
plt.show()
```

104. A simple way to incorporate `seaborn` with `matplotlib` is to use `seaborn`'s plot styling.

105. `matplotlib` also has a few different style sheets based on `seaborn`.

106. Let's look at a couple more sophisticated statistical plots built using `seaborn`.

107. We can create a plot that highlights the relationship between resturaunt bill amount, tip, and meal time.

108. This data comes from the `tips` example dataset already packaged in `seaborn`.
```Python
# import seaborn
import seaborn as sns

# apply default seaborn theme
sns.set_theme()

# load dataset already stored as dataframe
tips = sns.load_dataset("tips")

# create plot
sns.relplot(data=tips, x="total_bill", y="tip", col="time", hue="smoker", style="smoker", size="size",)
```

109. `seaborn` draws on `matplotlib`, but we don't need to directly load or import `matplotlib`.

110. We load the `tips` dataset as a `DataFrame`.

111. We then create a visualization that shows the relationships between the five dataset variables through the single call to the `.relplot()` function.

112. We can start to think through all of the work happening behind the scenes in `matplotlib` to generate this visualization:
- create figure/axes object with a 2, 1 subplot grid
- create subplots
- set tick values and labels for each axis for both subplots
- set axis labels and plot titles for both subplots
- set plot type for each subplot
- set symbol types, color, and size for each type of datapoint, for each subplot
- generate legend

113. And the list goes on.

114. `seaborn` handles all of those translations from the dataframe to `matplotlib` arguments.

115. This simplifies the work of writing code to generate this plot.

116. `seaborn`'s `.relplot()` function is designed to visualize statistical relationships.

117. Sometimes scatterplots are the most effective way to show these relationships.

118. But, in a relationship where one variable is a measure of time, a line can be a more effective representation.

119. We can use the `kind` parameter with the `.relplot()` function to make this change.

120. An example of `.relplot()` using a different sample dataset.

```Python
# load sample dataset as dataframe
dots = sns.load_dataset('dots')

# creat line plot showing relationships
sns.relplot(
    data=dots, kind="line",
    x="time", y="firing_rate", col="align",
    hue="choice", size="coherence", style="choice",
    facet_kws=dict(sharex=False),
)
```

121. In this example, the `style` parameter impacted line weight and style, rather than marker size as it did in the previous example.

122. A few other `seaborn` examples.

123. Relationship plot that presents average of one variable as a function of other variables.
```Python
fmri = sns.load_dataset("fmri")
sns.relplot(
    data=fmri, kind="line",
    x="timepoint", y="signal", col="region",
    hue="event", style="event",
)
```

124. `seaborn` estimates the statistical values using bootstrapping to compute confidence intervals and draw error bars to show uncertainty.

125. We could go back to our bill and tip data to generate a scatterplot that includes a linear regression model.
```Python
sns.lmplot(data=tips, x="total_bill", y="tip", col="time", hue="smoker")
```

126. We can visualize variable distribution with kernel density estimation.
```Python
sns.displot(data=tips, x="total_bill", col="time", kde=True)
```

127. `seaborn` can also calculate and plot the empirical cumulative distribution function (`ecdf`).
```Python
sns.displot(data=tips, kind="ecdf", x="total_bill", col="time", hue="smoker", rug=True)
```

128. We can also generate plots that are geared toward categorical data.

110. A `swarm` plot is a scatterplot with adjusted point positions on the categorical axis to minimize overlap
```Python
sns.catplot(data=tips, kind="swarm", x="day", y="total_bill", hue="smoker")
```

129. We could also display this categorical data using kernel density estimation and a violin plot.
```Python
sns.catplot(data=tips, kind="violin", x="day", y="total_bill", hue="smoker", split=True)
```

130. We could also display this data with a grouped bar chart that shows mean values and confidence intervals for each category.
```Python
sns.catplot(data=tips, kind="bar", x="day", y="total_bill", hue="smoker")
```

131. This is just a taste of how `seaborn` works to generate more advanced statistical plots.

132. Because the package integrates with `matplotlib`, customizing `seaborn` plots requires knowledge of `matplotlib` functionality and syntax.

133. Dropping down to the `matplotlib` layer is not always necessary (as shown in these examples), but a robust `matplotlib` foundation is knowledge that transfers when working with `seaborn`.

134. For more on `seaborn`:
- [`seaborn`, "seaborn: statistical data visualization"](http://seaborn.pydata.org/)
- [`seaborn`, "Installing and getting started"](https://seaborn.pydata.org/installing.html)
- [`seaborn`, "An introduction to seaborn"](http://seaborn.pydata.org/introduction.html)
- [`seaborn`, "User guide and tutorial"](http://seaborn.pydata.org/tutorial.html)
- [`seaborn`, "Example gallery"](https://seaborn.pydata.org/examples/index.html)
- [`seaborn`, "API reference"](https://seaborn.pydata.org/api.html)
- [Jake VanderPlas, "Visualization With Seaborn" from *Python Data Science Handbook*](https://jakevdp.github.io/PythonDataScienceHandbook/04.14-visualization-with-seaborn.html)

## Lab Notebook Question 2

Q2: Build at least two different types of plots using seaborn and data stored in a Pandas DataFrame. Include code + comments.
- *I encourage folks to use this question to explore visualizations you might use for the final project.*

For each plot, include the following elements or components:
- Title
- Axis labels
- Legend
- Scale or tickmarks
- Data source
 
Plot types to choose from:
- Line plots
- Bar chart
- Grouped bar chart
- Horizontal bar chart
- Stacked bar chart
- Histogram
- Box plot
- Area plot
- Scatter plot
- Pie chart

# Additional Resources for Statistical Analysis and Machine Learning

135. With the exception of some of the `Seaborn` plot types explored at the end of the lab, most of the plots focus on methods for plotting summary statistics.

136. Python can support a wide variety of statistical analysis methods and modelling techniques.

137. `statsmodels` is a Python module that supports a variety of regression and linear models, time series analysis, survival and duration analysis, and multivariate statistics.
- [Statsmodels documentation](https://www.statsmodels.org/stable/user-guide.html)

138. `scikit-learn` is a machine learning Python library that supports a variety of classification algorithms (nearest neighbors, random forest, etc), regression models (nearest neighbors, random forest, etc), and clustering algorithms (k-Means, spectral, mean-shift, etc). 

139. `scikit-learn` incorporates/is built on `matplotlib` and has robust support for plotting and visualization.
- [Scikit-learn documentation](https://scikit-learn.org/stable/)

# Lab Notebook Questions

[Link to lab notebook template (Jupyter Notebook)](https://drive.google.com/file/d/1qUT3e8D02oABf6DBjMEm6sb7thhk7wIR/view?usp=sharing)

Q1: Build at least three different types of plots using matplotlib and data stored in a Pandas DataFrame. Include code + comments. 
- *I encourage folks to use this question to explore visualizations you might use for the final project.*

For each plot, include the following elements or components:
- Title
- Axis labels
- Legend
- Scale or tickmarks
- Data source
 
Plot types to choose from:
- Line plots
- Bar chart
- Grouped bar chart
- Horizontal bar chart
- Stacked bar chart
- Histogram
- Box plot
- Area plot
- Scatter plot
- Pie chart

Q2: Build at least two different types of plots using seaborn and data stored in a Pandas DataFrame. Include code + comments.
- *I encourage folks to use this question to explore visualizations you might use for the final project.*

For each plot, include the following elements or components:
- Title
- Axis labels
- Legend
- Scale or tickmarks
- Data source
 
Plot types to choose from:
- Line plots
- Bar chart
- Grouped bar chart
- Horizontal bar chart
- Stacked bar chart
- Histogram
- Box plot
- Area plot
- Scatter plot
- Pie chart
