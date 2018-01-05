---
title: Making Plots With ggplot
teaching: 40
exercises: 50
questions:
    - " How can I visualize data in Python?"
    - " What is 'grammar of graphics'?"
objectives:
    - "Create a matplotlib object."
    - "Set universal plot settings."
    - "Modify an existing matplotlib object."
    - "Change the aesthetics of a plot such as colour."
    - "Edit the axis labels."
    - "Build complex plots using a step-by-step approach."
    - "Create scatter plots, box plots, and time series plots." 
    - "Use the facet_wrap and facet_grid commands to create a collection of plots splitting the data by a factor variable."
    - "Create customized plot styles to meet their needs." 


<!--
## Disclaimer ##
 Python has powerful built-in plotting capabilities such as `matplotlib`, but for this exercise, we will be using the [`ggplot`](http://ggplot.yhathq.com/) package, which facilitates the creation of highly-informative plots of structured data based on the R implementation of [`ggplot2`](http://ggplot2.org/) and [The Grammar of Graphics](http://link.springer.com/book/10.1007%2F0-387-28695-0) by Leland Wilkinson.-->


<!--

```python
import pandas as pd


surveys_complete = pd.read_csv('data_output/surveys_complete.csv')
surveys_complete
```


```python
%matplotlib inline
from ggplot import *
```
-->

# Plotting with matplotlib

Python has powerful plotting capabilities with its built-in  `matplotlib` library. We will make a plot using this library, its `pyplot` module and our pandas dataframe.

Before we begin let's import the `pandas` module and load in our data.

```python
import pandas as pd

rainfall_data = pd.read_csv('data/raingauge_data.csv')
rainfall_data
```
Now let's import the `pyplot` module from the `matplotlib` library. 

```python
%matplotlib inline
import matplotlib.pyplot as plt
```

`matplotlib`'s `pyplot` module is a powerful plotting tool that makes it simple to create complex plots
from data in a dataframe. It uses default settings, which help creating
publication quality plots with a minimal amount of settings and tweaking.

matplotlib graphics are built step by step by adding new elements.

To build a matplotlib plot we need to:

- generate an empty figure object with no axes and assign it a _variable_ name.

- bind **_one_** axes object to the figure, assign the axes object a _variable_ name.

- decide what type of graph you want to plot e.g. *bar, line, scatter*. These plots are functions that belong to the axes object we generated above.

- plot the data on the axis and descide on aesthetics such as plotting size, shape color, etc.,




```python
fig = pyplot.figure()                                          # generate figure
ax = fig.add_subplot(111)                                      # bind one axis to the figure
points = ax.scatter(rainfall_data['UT'],rainfall_data['data']) #plotting the data as scatter points
```


Notes:

- In order to use the `scatter` function we **must** call it with _x_ and _y_ arguments as ```scatter(x,y)```, otherwise we will generate an error.
- A link to useful informtion on how to use the `scatter` plot function can be found [here](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.scatter.html#matplotlib.pyplot.scatter)

- By binding the `scatter` plot object to the variable ```points``` we can perform "real time" operations on the scatter plot. This allows us to manipulate the plot until we are happy with its aesthetics.
- Once happy with the aesthetics we can pass them all as keyword arguments into the ```scatter``` function in **one** call.


# Building your plots iteratively

A great feature of the pyplot module is that we can build plots iteritively. We start by
passing the dataset to the `scatter` function




```python
points = ax.scatter(rainfall_data['UT'],rainfall_data['data]')
```


Then, we start modifying this plot to extract more information from it. For
instance, we can add transparency (alpha) to avoid overplotting.




```python
points.set_alpha(0.1)
```


We can also add colours for all the points. This is a little more complex since the scatter _points_ are stored _inside_ the variable `points` as a collection. Therefore we must pass an array of length *number of points* with the colour of each _point_ within the collection to the function `set_color()`.

First we must loop over all the points and assign colours to them:

```
import numpy as np                           # we will need Numpy to do this
colourList = []                              # create an empty list
for i in range(len(rainfall_data['data']):   # loop over points and append the colour red to the list
	colourList.append('red')
colourList = np.asarray(colourList)          # use Numpy to comvert the list to an array
```

Now we can set the colours in our plot



```python
points.set_color(colourList)
```


Or to color each raingauge in the plot differently: This is quite advanced so let's go through it line-by-line

First let's use the `panda`'s `groupby` function to group all our rainfall data by rangauge ID.


```python
g = rainfall_data.groupby(by='raingauges_id') # group the raingauges
num_grps = len(g)                             # get the total number of raingauges
num_grps
```
Now let's get a colour map that the `scatter` function uses from the `pyplot` module to assign colour to its points. More on colourmaps can be found [here](https://matplotlib.org/examples/color/colormaps_reference.html).

```
ccm = plt.get_cmap('jet')     # NOTE: there are 256 colours in a colourmap. We will use the jet colourmap.

```

Create an empty list to store colour values

```
colourList = []
```
Loop over the data and assign colours based on the raingage ID

```
for i in range(len(rainfall_data['data'])):
    r_id = rainfall_data['raingauges_id'][i]       # raingauge id as number
    indx = r_id*(float(r_id)/num_grps))            # index in colour array normalized to the number of rainguages
    colourList.append(ccm.colors[indx])
    
colourList = np.asarray(colourList)                # convert to an array
points.set_color(colourList)                       # set the colours
```


# Boxplot

The scatter plot we have just genrated doesn't provide much detail on the characteristics of the dataset. So let's visualise the distribution of rainfall within each raingauge.


In order to plot all the boxes on one axis we must store the data for each group in a list.

```python

groups = [] # create and empty list to storre the data

for i in range(len(g)):                # loop over the groups
	if i != 38:
		continue
	data = g.get_group(i+1)            # get the dataframe for each group... remember python indexing starts at 0 but our groups start at 1
	groups.append(data['data'])        # append the 'data' column to our empty list 
```
Now we can pass this info to our `boxplot` function

```python
ax.cla() # clear the previous axes object or create a new instance... it's up to you :)

ax.boxplot(groups)
```
What do you see? Can you make sense of this?

#Challenge
>Can you change the axes limits to see the boxes?

<!--Answer

ax.set_ylim(0,3)
-->

By default python adds outliers to the plot. By adding points to boxplot, we can have a better idea of the number of
measurements and of their distribution:

```python
# In order to display points spread out in the horizontal 
# direction around each group we will need to add some horizontal 
# noise to the ID's.

jitter = rainfall_data['raingauges_id']+np.random.normal(loc=0,scale=0.04,size = len(rainfall_data['raingauges_id'])

# the loc and scale keywords are just the mean and standard deviation.           
            
ax.scatter(jitter,rainfall_data['data'],s=1)
```

## Challenges

> Boxplots are useful summaries, but hide the *shape* of the distribution. For
> example, if there is a bimodal distribution, this would not be observed with a
> boxplot. An alternative to the boxplot is the violin plot (sometimes known as a
> beanplot), where the shape (of the density of points) is drawn.
>
> - Replace the box plot with a violin plot; see [`violinplot()`](https://matplotlib.org/gallery/statistics/violinplot.html)
>
> In many types of data, it is important to consider the *scale* of the
> observations.  For example, it may be worth changing the scale of the axis to
> better distribute the observations in the space of the plot.  Changing the scale
> of the axes is done similarly to adding/modifying other components (i.e., by
> incrementally adding commands).
>
> - Change the yscale to Log see more [here](https://matplotlib.org/devdocs/api/_as_gen/matplotlib.pyplot.yscale.html)
>


<!-- Answers

```python
## Challenges:
##  Clear the boxplot we created:
ax.cla()

```


```python
##  1. Replace the box plot with a violin plot; see `geom_violin()`.

ax.violinplot(groups)
```


```python
##  2. Represent weight on the log10 scale; see `scale_y_log10()`.
ax.set_yscale('log')

## what do the other scales look like?
```
-->


# Plotting time series data

Let's calculate number of rainfall records per day for each raingauge. To do that we need
to create a day column, group the data first and count records within each group.




```python
import datetime as dt

days = []                               # create and empty day list
dats = []										# store dates for later
for ut in rainfall_data['UT']:
	dat = dt.datetime.fromtimestamp(ut) # convert UT to datetime object
	days.append(dat.day)
	dats.append(dat)

DAYS = pd.Series(days)                 # create a Series object
rainfall_data['day'] = DAYS            # add column to dataframe
rainfall_data['datetime'] = pd.Series(dats)

```
 Now let's group the data and add:
 
 ```python
day_counts = rainfall_data[['day','raingauges_id']].groupby(['day','raingauges_id']).size().reset_index()
 
 ```

Timeseries data can be visualised as a line plot with days on x axis and counts
on y axis.




```python
ax.cla()
ax.plot(day_counts['datetime'],day_counts['data'])
```


Unfortunately this does not work, because we plot data for all the species
together. We need to tell pyplot to draw a line for each raingauge by grouping the data by raingauges_id. 
We will be able to distinguish gauges in the plot if we add colors.




```python
gaugeGrps = day_counts.groupby('raingauges_id')

for i in range(len(gaugeGrps)):
	if i == 38 or i == 39:
		continue
	ax.plot(gaugeGrps.get_group(i+1)['day'],gaugeGrps.get_group(i+1)[0])

```
##Challenge

With all those colours how do we distinguish the different raingauges?
Can you figure it out [here](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.legend.html)?



# Subplots

As we've already seen an `axes` object is drawn on a `figure` instance using the `add_subplot()` function. 

Subplotting in python is a useful way to represent complex datasets and 'de-clutter' our plot. 

The `add_subplot` function takes the *num_rows*, *num_cols* and an *index* as arguments. More on the `add_subplot` function can be found [here](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.subplot.html#matplotlib.pyplot.subplot). This is great but what if we have a large number of plots to draw? Would we have to assign a variable to each subplot?? Can we predefine all the subplots and then refer to specific axes within our plot?

For that let's use `pyplot`'s `subplots()` function. Have a look at the documentation [here](https://matplotlib.org/examples/pylab_examples/subplots_demo.html).

Let's re-draw the graphs from the previous exercise and neaten up our plot.

```python
# let's clear the figure

fig.clear()

num_rows = 6
num_cols = 7
fig2,axArr = plt.subplots(num_rows,num_cols,sharey=True,sharex=True)  # add another axes instance with num_rows*num_cols = number of raingauge IDs

for i in range(num_rows):
	for j in range(num_cols):
		if i*j ==38 or i*j == 39:
			contine
		ind = (i+1)*(j+1)
		axArr[i,j].plot(gaugeGrps.get_group(ind)['day'],gaugeGrps.get_group(ind)[0])
	

```


# Challenge


## Customization

Take a look at the matplotlib `text` documentation [here](https://matplotlib.org/users/text_intro.html), and
think of ways to improve the plot labels. You can write down some of your ideas as
comments in the Etherpad.

Now, let's change names of axes to something more informative add a title to this figure:




```python
title = fig2.suptitle('Rainfall Intensity over the past 7 days',fontsize=14, fontweight='bold')

xLabel = fig2.text(0.5,0.04,'Day',ha='center',fontsize=5)
yLabel = fig2.text(0.04,0.5,'Rainfall Itensity mm/5min',va='center',rotation='vertical',fontsize=5)
```


Can you think why we have assigned variable names to the text?

The axes have more informative names, but their readability can be improved by
increasing the font size. While we are at it, we'll also change the font family:




```python
yLabel.set_fontsize(10)
xLabel.set_fontsize(10)
```


If you like the changes you created to the default theme, you can save them as
a python script to easily apply them to other plots you may create:



With all of this information in hand, please take another five minutes to either
improve one of the plots generated in this exercise or create a beautiful graph
of your own.

Here are some ideas:

* See if you can change thickness of the lines.
* Can you find a way to change the name of the legend? What about its labels?
* Use a different color palette (see [https://matplotlib.org/examples/color/colormaps_reference.html](https://matplotlib.org/examples/color/colormaps_reference.html))

After creating your plot, you can save it to a file in your favourite format.
You can easily change the dimension (and its resolution) of your plot by
adjusting the appropriate arguments (`name` and `dpi`):




```python
fig.savefig('my_first_plot.png',dpi = 120)
```
see more [here](https://matplotlib.org/devdocs/api/_as_gen/matplotlib.pyplot.savefig.html)


## Final plotting challenge:
With all of this information in hand, please take another five
minutes to either improve one of the plots generated in this
exercise or create a beautiful graph of your own. Use the Matplotlib gallery
for inspiration:
[https://matplotlib.org/gallery/index.html](https://matplotlib.org/gallery/index.html)


