---
title: Combining DataFrames with pandas
teaching: 20
exercises: 25
questions:
- " Can I work with data from multiple sources? "
- " How can I combine data from different data sets? "
objectives:
    - Combine data from multiple files into a single DataFrame using merge and concat.
    - Combine two DataFrames using a unique ID found in both DataFrames.
    - Employ `to_csv` to export a DataFrame in CSV format.
    - Join DataFrames using common fields (join keys).
---

In many "real world" situations, the data that we want to use come in multiple
files. We often need to combine these files into a single DataFrame to analyze
the data. The pandas package provides [various methods for combining
DataFrames](http://pandas.pydata.org/pandas-docs/stable/merging.html) including
`merge` and `concat`.

To work through the examples below, we first need to load the different  files into pandas DataFrames. We will work with the files 'raingauges_data.csv', 'raingauges.csv' and 'region.csv'.

(If done SQL already remember the files from there, otherwise open in text editor for first look)

```python
import pandas as pd
rain_df = pd.read_csv("data/raingauge_data.csv")
rain_df.head()

   ID              TR          UT  data  raingauges_id    update_ref  invalid  \
0   1  17/12/04 10:51  1512227100   0.6              1  1512227100-1        0   
1   2  17/12/04 10:51  1512227400   0.8              1  1512227400-1        0   
2   3  17/12/04 10:51  1512227700   0.2              1  1512227700-1        0   
3   4  17/12/04 10:51  1512228000   0.2              1  1512228000-1        0   
4   5  17/12/04 10:51  1512252000   0.0              1  1512252000-1        0   

   hours_surrounding_total  
0                      1.8  
1                      1.8  
2                      1.8  
3                      1.8  
4                      0.0  
```

```python
gauges_df = pd.read_csv("data/raingauges.csv")
gauges_df.head()

  id                  name  location_x  location_y  region_id reference  \
0   1        BLUFF RES NO.3   31.005075  -29.933965          1    bluff3   
1   2   CHATSWORTH RES NO.1   30.900285  -29.911171          1    chats1   
2   3   CHATSWORTH RES NO.4   30.859903  -29.903620          1    chats4   
3   4  CITY ENGINEER'S DEPT   31.023634  -29.851328          1   cityeng   
4   5        CRABTREE S-P-S   30.992646  -29.883997          1  crabtree   

   ward_id  
0       66  
1       70  
2       71  
3       26  
4       32  
```
```python
regions_df = pd.read_csv("data/regions.csv", names=('id','region'))
regions_df.head()
```

Take note that the `read_csv` method we used can take some additional options which
we didn't use previously. Many functions in python have a set of options that
can be set by the user if needed. In this case we used `name` to hand over a list with column names, since the region.csv file missed column names.
[More about all of the read_csv options here.](http://pandas.pydata.org/pandas-docs/dev/generated/pandas.io.parsers.read_csv.html)

# Concatenating DataFrames

We can use the `concat` function in Pandas to append either columns or rows from
one DataFrame to another.  Let's grab two subsets of our data to see how this
works.

```python
# read in first 10 lines of rain table
rain_sub = rain_df.head(10)
# grab the last 10 rows 
rain_sub_last10 = rain_df.tail(10)
#reset the index values to the second dataframe appends properly
survey_sub_last10=survey_sub_last10.reset_index(drop=True)
# drop=True option avoids adding new index column with old index values
```

When we concatenate DataFrames, we need to specify the axis. `axis=0` tells
Pandas to stack the second DataFrame under the first one. It will automatically
detect whether the column names are the same and will stack accordingly.
`axis=1` will stack the columns in the second DataFrame to the RIGHT of the
first DataFrame. To stack the data vertically, we need to make sure we have the
same columns and associated column format in both datasets. When we stack
horizontally, we want to make sure what we are doing makes sense (ie the data are
related in some way).

```python
# stack the DataFrames on top of each other
vertical_stack = pd.concat([rain_sub, rain_sub_last10], axis=0)

# place the DataFrames side by side
horizontal_stack = pd.concat([rain_sub, rain_sub_last10], axis=1)
```

### Row Index Values and Concat
Have a look at the `vertical_stack` dataframe? Notice anything unusual?
The row indexes for the two data frames `rain_sub` and `rain_sub_last10`
have been repeated. We can reindex the new dataframe using the `reset_index()` method.

```python
vertical_stack.reset_index(drop=True)
```

<!--
## Writing Out Data to CSV

We can use the `to_csv` command to do export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash to the file
`vertical_stack.to_csv('foldername/out.csv')`. We use the 'index=False' so that
pandas doesn't include the index number for each line.

```python
# Write DataFrame to CSV
vertical_stack.to_csv('out.csv', index=False)
```

Check out your working directory to make sure the CSV wrote out properly, and
that you can open it! If you want, try to bring it back into python to make sure
it imports properly.

```python
# for kicks read our output back into python and make sure all looks good
new_output = pd.read_csv('out.csv', keep_default_na=False, na_values=[""])
```
-->

> ## Challenge - Combine Data
>
> In the data folder, there are two rainfall data files: `rainfallNorth.csv` and
> `rainfallSouth.csv`. Read the data into python and combine the files to make one
> new data frame.
> Export your results as a CSV and make sure it reads back into python properly.
{: .challenge}

# Joining DataFrames

When we concatenated our DataFrames we simply added them to each other -
stacking them either vertically or side by side. Another way to combine
DataFrames is to use columns in each dataset that contain common values (a
common unique id). Combining DataFrames using a common field is called
"joining". The columns containing the common values are called "join key(s)".
Joining DataFrames in this way is often useful when one DataFrame is a "lookup
table" containing additional data that we want to include in the other.

NOTE: This process of joining tables is similar to what we do with tables in an SQL database.

For example, the `raingauges.csv` and the `regions.csv` file are lookup
tables. The `raingauges.csv` table contains the name, x and y coordinates, and a short reference for the 42 raingauges. The
raingauges id is unique for each line. These raingauges are identified in our rainfall
data as well using the unique id. Rather than adding more columns
for the name, location etc to each of the 6,744 lines in the rainfall data table, we
can maintain the shorter table with the raingauges information. When we want to
access that information, we can create a query that joins the additional columns
of information to the rainfall data.

Storing data in this way has many benefits including:

1. It ensures consistency in the spelling of raingauge attributes (name, reference ...) given each raingauge is only entered once. Imagine the possibilities
   for spelling errors when entering the raingauge names thousands of times!
2. It also makes it easy for us to make changes to the raingauge information once
   without having to find each instance of it in the larger rainfall data.
3. It optimizes the size of our data.


## Joining Two DataFrames

To better understand joins, let's grab the first 10 lines of our data as a
subset to work with. We'll use the `.head` method to do this. 

```python
# read in rainfall data and grab first 10 lines 
rain_df = pd.read_csv('data/raingauge_data.csv')
rain_sub = rain_df.head(10)

# read in raingauge infos and grab first 10 lines gauges_df = pd.read_csv('data/raingauges.csv')
gauges_sub = gauges_df.head(10)
```

In this example, `gauges_sub` is the lookup table containing name and location of raingauges that we want to join with the data in `rain_sub` to produce a new DataFrame that contains all of the columns from both `rain_df` *and*
`gauges_df`.


## Identifying join keys

To identify appropriate join keys we first need to know which field(s) are
shared between the files (DataFrames). We might inspect both DataFrames to
identify these columns. If we are lucky, both DataFrames will have columns with
the same name that also contain the same data. If we are less lucky, we need to
identify a (differently-named) column in each DataFrame that contains the same
information.

```python
>>> gauges_sub.columns

Index(['id', 'name', 'location_x', 'location_y', 'region_id', 'reference', 'ward_id'],
      dtype='object')
      
>>> rain_sub.columns

Index(['ID', 'UT', 'data', 'raingauges_id'], dtype='object')
```

In our example, the join key is the column containing the id (integer between 1 and 44) for the raingauge, which is called `id` in the `gauges` table and `raingauges_id` in the `rain` table.

Now that we know the fields with the common raingauge ID attributes in each
DataFrame, we are almost ready to join our data. However, since there are
[different types of joins](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/), we
also need to decide which type of join makes sense for our analysis.

## Inner joins

The most common type of join is called an _inner join_. An inner join combines
two DataFrames based on a join key and returns a new DataFrame that contains
**only** those rows that have matching values in *both* of the original
DataFrames.

Inner joins yield a DataFrame that contains only rows where the value exists in BOTH tables. An example of an inner join, adapted from [this
page](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/) is below:

![Inner join -- courtesy of codinghorror.com](http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png)

The pandas function for performing joins is called `merge` and an Inner join is
the default option:  

```python
merged_inner = pd.merge(left=rain_sub,right=gauges_sub, left_on='raingauges_id', right_on='id')
# since the keys have different names (`raingauges_id` and `id`) in the two dataframes we have to define the join keys with `left_on` and `right_on`. If the join keys would have the same name we could skip `left_on` and `right_on` 

# what's the size of the output data?
merged_inner.shape
merged_inner.head()
```

**OUTPUT:**

```
  
```

The result of an inner join of `rain_sub` and `gauges_sub` is a new DataFrame
that contains the combined set of columns from `rain_sub` and `gauges_sub`. It
*only* contains rows that have raingauge ids that are the same in
both the `rain_sub` and `gauges_sub` DataFrames. In other words, if a row in
`rain_sub` has a value of `raingauges_id` that does *not* appear in the `id`
column of `gauges_sub`, it will not be included in the DataFrame returned by an
inner join.  Similarly, if a row in `gauges_sub` has a value of `id`
that does *not* appear in the `raingauges_id` column of `rain_sub`, that row will not
be included in the DataFrame returned by an inner join.

The two DataFrames that we want to join are passed to the `merge` function using
the `left` and `right` argument. The `left_on='raingauges_id'` argument tells `merge`
to use the `raingauges_id` column as the join key from `rain_sub` (the `left`
DataFrame). Similarly , the `right_on='id'` argument tells `merge` to
use the `id` column as the join key from `gauges_sub` (the `right`
DataFrame). For inner joins, the order of the `left` and `right` arguments does
not matter.

The result `merged_inner` DataFrame contains all of the columns from `rain_sub`
(ID, UT, data etc.) as well as all the columns from `gauges_sub`
(id, name, location_x, etc).

In our case `merged_inner` has the same number of rows than `rain_sub`. This is an
indication that there were rows in `rain_df` with value(s) for `raingauges_id` that
do not exist as value(s) for `id` in `gauges_df`.

## Left joins

What if we want to add information from `species_sub` to `survey_sub` without
losing any of the information from `survey_sub`? In this case, we use a different
type of join called a "left outer join", or a "left join".

Like an inner join, a left join uses join keys to combine two DataFrames. Unlike
an inner join, a left join will return *all* of the rows from the `left`
DataFrame, even those rows whose join key(s) do not have values in the `right`
DataFrame.  Rows in the `left` DataFrame that are missing values for the join
key(s) in the `right` DataFrame will simply have null (i.e., NaN or None) values
for those columns in the resulting joined DataFrame.

Note: a left join will still discard rows from the `right` DataFrame that do not
have values for the join key(s) in the `left` DataFrame.

![Left Join](http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b01287770273e970c-pi.png)

A left join is performed in pandas by calling the same `merge` function used for
inner join, but using the `how='left'` argument:

```python
merged_left = pd.merge(left=survey_sub,right=species_sub, how='left', left_on='species_id', right_on='species_id')

merged_left

**OUTPUT:**

   record_id  month  day  year  plot_id species_id sex  hindfoot_length  \
0          1      7   16  1977        2         NL   M               32   
1          2      7   16  1977        3         NL   M               33   
2          3      7   16  1977        2         DM   F               37   
3          4      7   16  1977        7         DM   M               36   
4          5      7   16  1977        3         DM   M               35   
5          6      7   16  1977        1         PF   M               14   
6          7      7   16  1977        2         PE   F              NaN   
7          8      7   16  1977        1         DM   M               37   
8          9      7   16  1977        1         DM   F               34   
9         10      7   16  1977        6         PF   F               20   

   weight       genus   species    taxa  
0     NaN     Neotoma  albigula  Rodent  
1     NaN     Neotoma  albigula  Rodent  
2     NaN   Dipodomys  merriami  Rodent  
3     NaN   Dipodomys  merriami  Rodent  
4     NaN   Dipodomys  merriami  Rodent  
5     NaN         NaN       NaN     NaN  
6     NaN  Peromyscus  eremicus  Rodent  
7     NaN   Dipodomys  merriami  Rodent  
8     NaN   Dipodomys  merriami  Rodent  
9     NaN         NaN       NaN     NaN  
```

The result DataFrame from a left join (`merged_left`) looks very much like the
result DataFrame from an inner join (`merged_inner`) in terms of the columns it
contains. However, unlike `merged_inner`, `merged_left` contains the **same
number of rows** as the original `survey_sub` DataFrame. When we inspect
`merged_left`, we find there are rows where the information that should have
come from `species_sub` (i.e., `species_id`, `genus`, and `taxa`) is
missing (they contain NaN values):

```python
merged_left[ pd.isnull(merged_left.genus) ]
**OUTPUT:**
   record_id  month  day  year  plot_id species_id sex  hindfoot_length  \
5          6      7   16  1977        1         PF   M               14   
9         10      7   16  1977        6         PF   F               20   

   weight genus species taxa  
5     NaN   NaN     NaN  NaN  
9     NaN   NaN     NaN  NaN
```

These rows are the ones where the value of `species_id` from `survey_sub` (in this
case, `PF`) does not occur in `species_sub`.


## Other join types

The pandas `merge` function supports two other join types:

* Right (outer) join: Invoked by passing `how='right'` as an argument. Similar
  to a left join, except *all* rows from the `right` DataFrame are kept, while
  rows from the `left` DataFrame without matching join key(s) values are
  discarded.
* Full (outer) join: Invoked by passing `how='outer'` as an argument. This join
  type returns the all pairwise combinations of rows from both DataFrames; i.e.,
  the result DataFrame will `NaN` where data is missing in one of the dataframes. This join type is
  very rarely used.

# Final Challenges

> ## Challenge - Distributions
> Create a new DataFrame by joining the contents of the `surveys.csv` and
> `species.csv` tables. Then calculate and plot the distribution of:
>
> 1. taxa by plot
> 2. taxa by sex by plot
{: .challenge}

> ## Challenge - Diversity Index
>
> 1. In the data folder, there is a plot `CSV` that contains information about the
>    type associated with each plot. Use that data to summarize the number of
>   plots by plot type.
> 2. Calculate a diversity index of your choice for control vs rodent exclosure
>   plots. The index should consider both species abundance and number of
>   species. You might choose to use the simple [biodiversity index described
>   here](http://www.amnh.org/explore/curriculum-collections/biodiversity-counts/plant-ecology/how-to-calculate-a-biodiversity-index)
>   which calculates diversity as:
>
>        the number of species in the plot / the total number of individuals in the plot = Biodiversity index.
{: .challenge}
