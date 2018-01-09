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

Python has powerful plotting capabilities with its built-in  `matplotlib` library. We will make a plot using this library, its `pyplot` module and our `pandas` dataframe.

Before we begin let's import the `pandas` module and load in our data.

```python
import pandas as pd

rainfall_data = pd.read_csv('data/raingauge_data.csv')
rainfall_data
```
There is no need to import the `pyplot` module because the `pandas` library can access it directly.

<!--
Now let's import the `pyplot` module from the `matplotlib` library. 

```python
%matplotlib inline
import matplotlib.pyplot as plt
```
-->

`matplotlib`'s `pyplot` module is a powerful plotting tool that makes it simple to create complex plots
from data in a dataframe. It uses default settings, which help creating
publication quality plots with a minimal amount of settings and tweaking.

We can use our `pandas` Dataframe to plot matplotlib graphics. The plot is built step by step by adding new elements using *keyword* arguments.

To build a matplotlib plot we need to:

- decide what type of graph you want to plot e.g. *bar, line, scatter*. <!--These plots are functions that belong to the axes object we generated above.-->

- plot the data and decide on aesthetics such as plotting size, shape color, etc.

<!-- - generate an empty figure object with no axes and assign it a _variable_ name. -->

<!--- bind **_one_** axes object to the figure, assign the axes object a _variable_ name. -->




<!--

```python
fig = pyplot.figure()                                          # generate figure
ax = fig.add_subplot(111)                                      # bind one axis to the figure
points = ax.scatter(rainfall_data['UT'],rainfall_data['data']) #plotting the data as scatter points
```
-->
Let's start with a simple scatter plot:

```python
rainfall_data.plot(x='UT',y='data',kind='scatter')

```
What happens if you call the `plot` function without any variables?

Notes:

- The key word argument *kind* tells `pandas` what type of plot to use.
- In order to use the `scatter` function we **must** call it with _x_ and _y_ otherwise we will generate an error.
- A link to useful informtion on how to use the `scatter` plot function can be found [here](https://pandas.pydata.org/pandas-docs/stable/visualization.html)
- The `matplotlib` documentation is found at [https://matplotlib.org/api/_as_gen/matplotlib.pyplot.scatter.html#matplotlib.pyplot.scatter](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.scatter.html#matplotlib.pyplot.scatter)

<!--- By binding the `scatter` plot object to the variable ```points``` we can perform "real time" operations on the scatter plot. This allows us to manipulate the plot until we are happy with its aesthetics.-->
- Once happy with the aesthetics we can create a function to generate similar figures in the future.


# Building your plots iteratively

A great feature of the pyplot module is that we can build plots iteritively. We start by
passing the dataset to the `scatter` function




```python
rainfall_data.plot(x='UT',y='data',kind='scatter')
```


Then, we start modifying this plot to extract more information from it. For
instance, we can add transparency (alpha) to avoid overplotting.




```python
rainfall_data.plot(x='UT',y='data',kind='scatter',alpha=0.1)
```


We can also add colours for **all** the points. 

<!--This is a little more complex since the scatter _points_ are stored _inside_ the variable `points` as a collection. Therefore we must pass an array of length *number of points* with the colour of each _point_ within the collection to the function `set_color()`.

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
-->
```python
rainfall_data.plot(x='UT',y='data',kind='scatter',alpha=0.1,c='r')
```

Or to color each raingauge in the plot differently:

```python
rainfall_data.plot(x='UT',y='data',kind='scatter',alpha=0.1,c='raingauges_id')
```
What do you see?
NOTE: In order to plot a different *colour* for each raingauge we had to tell the `plot` function to use the raingauges_id column.

##Challenge

Can you change the default colour scheme?

(Hint: some useful links: [https://pandas.pydata.org/pandas-docs/stable/visualization.html](https://pandas.pydata.org/pandas-docs/stable/visualization.html), [https://matplotlib.org/examples/color/colormaps_reference.html](https://matplotlib.org/examples/color/colormaps_reference.html))

<!--
Answer:

```python
rainfall_data.plot(x='UT',y='data',kind='scatter',alpha=0.1,c='raingauges_id',colormap='jet')
```
-->


<!--This is quite advanced so let's go through it line-by-line

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
-->

# Boxplot

The scatter plot we have just genrated doesn't provide much detail on the characteristics of the dataset. So let's visualise the distribution of rainfall within each raingauge.


In order to plot all the boxes on one axis we must _pivot_ our Dataframe so that each raingauge is a column. For that we use the [`pivot_table()`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.pivot_table.html) function.

```python
grouped = rainfall_data.pivot_table(values='data',columns='raingauges_id',index='UT')

grouped.plot(kind='box')
```

<!--
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
-->
What do you see? Can you make sense of this?

#Challenges
>Can you find another way to generate a Boxplot without using the `pivot_table()` function? (Hint: look [here](https://pandas.pydata.org/pandas-docs/stable/visualization.html))
>
>Can you change the axes limits to see the boxes?

<!--Answer

grouped.plot(kind='box',grid=False,ylim=(0,1))
-->

By default python adds outliers to the plot. Use the *sym* key word to change how they are presented. To remove them:

```python
grouped.plot(kind='box',grid=False,ylim=(0,1),sym='')
```

Boxplots are useful summaries, but hide the *shape* of the distribution. For example, if there is a bimodal distribution, this would not be observed with a boxplot.

 
Let's plot the rainfall distribution for each gauge as a scatter plot. From this we can have a better idea of the shape of the measurements and their distributions:


```python
# In order to display points spread out in the horizontal 
# direction around each group we will need to add some horizontal 
# noise to the raingauge ID's (to prevent overlap).

# for this we will need numpy and matplotlib.pyplot

import numpy as np
import matplotlib.pyplot as plt

jitter = rainfall_data['raingauges_id']+np.random.normal(loc=0,scale=0.04,size = len(rainfall_data['raingauges_id'])

# the loc and scale keywords are just the mean and standard deviation.           
            
plt.scatter(jitter,rainfall_data['data'],s=1,ylim=(0,1))

```


An alternative to the boxplot is the [`violinplot()`](https://matplotlib.org/gallery/statistics/violinplot.html) plot (sometimes known as a
 beanplot), where the shape (of the density of points) is drawn.
 


## Challenges
> In many types of data, it is important to consider the *scale* of the
> observations.  For example, it may be worth changing the scale of the axis to
> better distribute the observations in the space of the plot.  Changing the scale
> of the axes is done similarly to adding/modifying other components (i.e., by
> incrementally adding commands).
>
> - Change the yscale to Log see more [here](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html)



<!-- Answers

```python
## Challenges:


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

Let's calculate the number of rainfall records per day for each raingauge. 
...To do that we need
to create a day column, group the data by raingauge and count records within each group...
To simplify, this has already been done so all we need to do is load in the csv file. For those interested in how to add another column to a Dataframe see below:

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
Let's load in the *new* file:

```python
rainfall_combined = pd.read_csv('data/rainfall/rainfall_combined.csv')
```


Timeseries data can be visualised as a line plot with days on x axis and counts
on y axis.




```python
rainfall_combined.plot(x='day',y='data')
```


Unfortunately this does not work, because we plot data for all the gauges
together. We need to tell pyplot to draw a line for each raingauge by grouping the data by raingauges_id. 
We will be able to distinguish gauges in the plot if we add colors.

Now let's group the data by day and raingauge and then sum the total amount of rainfall for each day for each gauge:
 
```python
dailyRain = rainfall_combined.pivot_table(values='data',columns='raingauges_id',index='day',aggfunc='sum')

#now plot only first 5 gauges

dailyRain.loc[:,1:5].plot()
```
<!--
```python
# reshape the Dataframe to have each raingauge as a column
day_count_pGauge = day_counts.pivot_table(values = 'rainfall',columns = 'raingauges_id',index='day')

#now plot only first 5 gauges

day_count_pGauge.loc[:,1:5].plot()



```
-->

We might need to perform these operations again so in your editor let's create a function called reOrganize and save our script:

```python
def reOrganize():
	rainfall_combined = pd.read_csv('data/rainfall/rainfall_combined.csv')

	dailyRain = rainfall_combined.pivot_table(values='data',columns='raingauges_id',index='day',aggfunc='sum')
	
	return dailyRain
```
##Challenges

> Can you add a y_label?
> 
> How do we turn the legend on or off?




# Subplots

The plot function can create a very cluttered graphic. We can reduce this clutter using subplotting. This is a useful way to represent complex data. 

We will use the `pandas.Dataframe.plot()` function. In order to make use of subplotting we must pass the *subplots* keyword into the `plot()` function. The *subplots* keyword is of type *boolean* meaning it can either have a value of *True* or *False*.

Let's start with the default settings:

```python
# we will only plot the first 5 raingauges
day_count_pGauge.loc[:,1:5].plot(legend=False,subplots=True)
```

The default layout is to vertically stack each raingauge plot.

Can you figure out how we can change our layout (give yourself 5mins? ([Hint](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html))

<!-- Answer
Let's plot the first 5 raingauges in a layout with 2 columns and 3 rows

```python
dailyRain.loc[:,1:5].plot(legend=False,subplots=True,layout=(3,2))
```
-->

Is there anything you notice about the limits of each Y Axis?
How do we ensure all the plots have the same limits?

```python
dailyRain.loc[:,1:5].plot(legend=False,subplots=True,layout=(3,2),sharey=True,xlim(1,7)
```

<!--
The `subplots` keyword takes the *num_rows*, *num_cols* and an *index* as arguments. More on the `add_subplot` function can be found [here](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.subplot.html#matplotlib.pyplot.subplot). This is great but what if we have a large number of plots to draw? Would we have to assign a variable to each subplot?? Can we predefine all the subplots and then refer to specific axes within our plot?

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

-->
# Challenge


## Customization

>NOTE: In order to dynamically change the text of our plot we must first change our backend that our computer uses to show us the plot. Up to now you have been vizualizing your plots *inline*... **but** we cannot dynamically edit our plot.
>
> To change this go to *preferences --> IPython console --> Graphics.
> Change the Graphic Backend to OS X (Mac) ? (Windows).
> 
> Restart your kernel and lets begin...




Take a look at the matplotlib `text` documentation [here](https://matplotlib.org/users/text_intro.html), and
think of ways to improve the plot labels. You can write down some of your ideas as
comments in the Etherpad.

Now, let's change names of axes to something more informative add a title to this figure:




```python
#let's reload our data:

import pandas as pd
import matplotlib.pyplot as plt

#run your saved script reOrganize from earlier...

dailyRain = reOrganize()

axes = dailyRain.loc[:,1:5].plot(legend=False,subplots=True,layout=(3,2),sharey=True,sharex=True,title='Daily Rainfall Intensity Over the past 7 Days',xlim=(1,7))

#extract our figure object from axes

fig = axes[0,0].figure

xLabel = fig.text(0.5,0.04,'Day',ha='center',fontsize=5)
yLabel = fig.text(0.04,0.5,'Rainfall Itensity mm/day',va='center',rotation='vertical',fontsize=5)
```
-->

Can you think why we have assigned variable names to the text?

The axes have more informative names, but their readability can be improved by
increasing the font size. While we are at it, we'll also change the font family:




```python
yLabel.set_fontsize(10)
xLabel.set_fontsize(10)
```

How do we get rid of those pesky *day* labels on a couple of our plots... We must turn them off one-by-one

```python
for i in range(axes.shape[0]):
    for j in range(axes.shape[1]):
        axes[i,j].xaxis.set_label('')
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


