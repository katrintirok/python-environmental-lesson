---
title: Data workflows and automation
teaching: 40
exercises: 50
questions:
  - " How can I automate operations in Python? "
  - " What are functions and why should I use them? "
objectives:
    - Describe why for loops are used in Python.
    - Employ for loops to automate data analysis.
    - Write unique filenames in Python.
    - Build reusable code in Python.
    - Write functions using conditional statements (if, then, else).
---

So far, we've used Python and the pandas library to explore and manipulate
individual datasets by hand, much like we would do in a spreadsheet. The beauty
of using a programming language like Python, though, comes from the ability to
automate data processing through the use of loops and functions.

## For loops

Loops allow us to repeat a workflow (or series of actions) a given number of
times or while some condition is true. We would use a loop to automatically
process data that's stored in multiple files (daily values with one file per
ward, for example). Loops lighten our work load by performing repeated tasks
without our direct involvement and make it less likely that we'll introduce
errors by making mistakes while processing each file by hand.

Let's write a simple for loop that simulates what a kid might see during a
visit to the zoo:

```python
animals = ['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
print(animals)
['lion', 'tiger', 'crocodile', 'vulture', 'hippo']

for creature in animals:
...    print(creature)
lion
tiger
crocodile
vulture
hippo
```

The line defining the loop must start with `for` and end with a colon, and the
body of the loop must be indented.

In this example, `creature` is the loop variable that takes the value of the next
entry in `animals` every time the loop goes around. We can call the loop variable
anything we like. After the loop finishes, the loop variable will still exist
and will have the value of the last entry in the collection:

```python
animals = ['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
for creature in animals:
...    pass

print('The loop variable is now: ' + creature)
The loop variable is now: hippo
```

We are not asking python to print the value of the loop variable anymore, but
the for loop still runs and the value of `creature` changes on each pass through
the loop. The statement `pass` in the body of the loop just means "do nothing".

> ## Challenge - Loops
>
> 1. What happens if we don't include the `pass` statement?
>
> 2. Rewrite the loop so that the animals are separated by commas, not new lines
> (Hint: You can concatenate strings using a plus sign. For example,
> `print(string1 + string2)` outputs 'string1string2').
{: .challenge}

## Automating data processing using For Loops

The file we've been using so far, `rainfall_combined.csv`, contains 110 wards of data and is quite large. We would like to separate the data for each ward into a separate
file.

Let's start by making a new directory inside the folder `data_output` to store all of
these files using the module `os`:

```python
    import os

    os.mkdir('data_output/ward_files')
```

The command `os.mkdir` is equivalent to `mkdir` in the shell. Just so we are
sure, we can check that the new directory was created within the `data_output` folder:

```python
os.listdir('data_output')
[...
 'ward_files']
```

The command `os.listdir` is equivalent to `ls` in the shell.

In previous lessons, we saw how to use the library pandas to load the rainfall
data into memory as a DataFrame, how to select a subset of the data using some
criteria, and how to write the DataFrame into a csv file. Let's write a script
that performs those three steps in sequence for ward 1:

```python
import pandas as pd

# Load the data into a DataFrame
rainfall_df = pd.read_csv('data/rainfall_combined.csv')

# Select only data for ward 1
rainfall1 = rainfall_df[rainfall_df.ward_id == 1]

# Write the new DataFrame to a csv file
rainfall1.to_csv('data_output/ward_files/rainfall1.csv')
```

To create data files for each ward, we could repeat the last two commands over and
over, once for each ward. Repeating code is neither elegant nor
practical, and is very likely to introduce errors into your code. We want to
turn what we've just written into a loop that repeats the last two commands for
every ward in the dataset.

Let's start by writing a loop that simply prints the names of the files we want
to create - the dataset we are using covers 34 wards with ids between 1 and 110, and we'll create
a separate file for each of those wards. Listing the filenames is a good way to
confirm that the loop is behaving as we expect.

We have seen that we can loop over a list of items, so we need a list of wards
to loop over. We can get the wards in our DataFrame with:

```python
rainfall_df['ward_id']
```
```
0       66.0
1       66.0
2       66.0
3       66.0
4       66.0
5       66.0
6       66.0
7       66.0
8       66.0
9       66.0
10      66.0
11      66.0
12      66.0
13      66.0
14      66.0
15      66.0
16      66.0
17      66.0
18      66.0
19      66.0
20      66.0
21      66.0
22      66.0
23      66.0
24      66.0
25      66.0
26      66.0
27      66.0
28      66.0
29      66.0
...
6719    97.0
6720    97.0
6721    97.0
6722    97.0
6723    97.0
6724    97.0
6725    97.0
6726    97.0
6727    97.0
6728    97.0
6729    97.0
6730    97.0
6731    97.0
6732    97.0
6733    97.0
6734    97.0
6735    97.0
6736    97.0
6737    97.0
6738    97.0
6739    97.0
6740    97.0
6741    97.0
6742    97.0
6743    97.0
6744     NaN
6745     NaN
6746     NaN
6747     NaN
6748     NaN

```

but we want only unique wards, which we can get using the `unique` function
which we have already seen.  

```python
rainfall_df['ward_id'].unique()
```
```
array([  66.,   70.,   71.,   26.,   32.,   23.,   63.,   25.,   27.,
         24.,   30.,   31.,   68.,   64.,   36.,   11.,  110.,   52.,
          3.,   60.,   35.,   59.,    1.,  103.,    9.,    8.,    7.,
         18.,   10.,   80.,   93.,   99.,   97.,   nan])
```

Putting this into our for loop we get

```python
for ward in rainfall_df['ward_id'].unique():
...    filename='data_output/ward_files/rainfall' + str(ward) + '.csv'
...    print(filename)
...
data_output/ward_files/rainfall66.0.csv
data_output/ward_files/rainfall70.0.csv
data_output/ward_files/rainfall71.0.csv
data_output/ward_files/rainfall26.0.csv
data_output/ward_files/rainfall32.0.csv
data_output/ward_files/rainfall23.0.csv
data_output/ward_files/rainfall63.0.csv
data_output/ward_files/rainfall25.0.csv
data_output/ward_files/rainfall27.0.csv
data_output/ward_files/rainfall24.0.csv
data_output/ward_files/rainfall30.0.csv
data_output/ward_files/rainfall31.0.csv
data_output/ward_files/rainfall68.0.csv
data_output/ward_files/rainfall64.0.csv
data_output/ward_files/rainfall36.0.csv
data_output/ward_files/rainfall11.0.csv
data_output/ward_files/rainfall110.0.csv
data_output/ward_files/rainfall52.0.csv
data_output/ward_files/rainfall3.0.csv
data_output/ward_files/rainfall60.0.csv
data_output/ward_files/rainfall35.0.csv
data_output/ward_files/rainfall59.0.csv
data_output/ward_files/rainfall1.0.csv
data_output/ward_files/rainfall103.0.csv
data_output/ward_files/rainfall9.0.csv
data_output/ward_files/rainfall8.0.csv
data_output/ward_files/rainfall7.0.csv
data_output/ward_files/rainfall18.0.csv
data_output/ward_files/rainfall10.0.csv
data_output/ward_files/rainfall80.0.csv
data_output/ward_files/rainfall93.0.csv
data_output/ward_files/rainfall99.0.csv
data_output/ward_files/rainfall97.0.csv
data_output/ward_files/rainfallnan.csv
```

We can now add the rest of the steps we need to create separate text files:

```python
# Load the data into a DataFrame
rainfall_df = pd.read_csv('data/rainfall_combined.csv')

for ward in rainfall_df['ward_id'].unique():

    # Select data for the ward
    rainfall_ward = rainfall_df[rainfall_df.ward_id == ward]

    # Write the new DataFrame to a csv file
    filename = 'data_output/ward_files/rainfall' + str(ward) + '.csv'
    rainfall_ward.to_csv(filename)
```

Look inside the `ward_files` directory and check a couple of the files you
just created to confirm that everything worked as expected.

## Writing Unique FileNames

Notice that the code above created a unique filename for each ward.

```python
	filename = 'data_output/ward_files/rainfall' + str(ward) + '.csv'
```

Let's break down the parts of this name:

* The first part is simply some text that specifies the directory to store our
  data file in (data_output/ward_files/) and the first part of the file name
  (rainfall): `'data_output/ward_files/rainfall'`
* We can concatenate this with the value of a variable, in this case `ward` by
  using the plus `+` sign: `+
  str(ward)`, we use the `str()` function to convert from numeric value of `ward` to a string
* Then we add the file extension as another text string: `+ '.csv'`

Notice that we use single quotes to add text strings. The variable is not
surrounded by quotes. This code produces the string
`data_output/ward_files/rainfall1.csv` which contains the path to the new filename
AND the file name itself.

> ## Challenge - Modifying loops
>
> 1. Some of the rainfall data you saved are missing data (they have null values that
> show up as NaN - Not A Number - in the DataFrames and do not show up in the text
> files). Modify the for loop so that the entries with null values are not
> included in the ward files.
>
> 2. Let's say you only want to look at data from a specific set of wards. How would you modify your loop in order to generate a data file for only ward 7, 10, 59 and 66?
>
> 3. Instead of splitting out the data by wards, a colleague wants to do analyses with each raingauge separately. How would you write a unique csv file for each raingauge?
{: .challenge}

## Building reusable and modular code with functions

Suppose that separating large data files into individual ward files is a task
that we frequently have to perform. We could write a **for loop** like the one above
every time we needed to do it but that would be time consuming and error prone.
A more elegant solution would be to create a reusable tool that performs this
task with minimum input from the user. To do this, we are going to turn the code
we've already written into a function.

Functions are reusable, self-contained pieces of code that are called with a
single command. They can be designed to accept arguments as input and return
values, but they don't need to do either. Variables declared inside functions
only exist while the function is running and if a variable within the function
(a local variable) has the same name as a variable somewhere else in the code,
the local variable hides but doesn't overwrite the other.

Every method used in Python (for example, `print`) is a function, and the
libraries we import (say, `pandas`) are a collection of functions. We will only
use functions that are housed within the same code that uses them, but it's also
easy to write functions that can be used by different programs.

Functions are declared following this general structure:

```python
def this_is_the_function_name(input_argument1, input_argument2):

    # The body of the function is indented
    # This function prints the two arguments to screen
    print('The function arguments are:', input_argument1, input_argument2, '(this is done inside the function!)')

    # And returns their product
    return input_argument1 * input_argument2
```

The function declaration starts with the word `def`, followed by the function
name and any arguments in parenthesis, and ends in a colon. The body of the
function is indented just like loops are. If the function returns something when
it is called, it includes a return statement at the end.

This is how we call the function:

```python
product_of_inputs = this_is_the_function_name(2,5)
The function arguments are: 2 5 (this is done inside the function!)

print('Their product is:', product_of_inputs, '(this is done outside the function!)')
Their product is: 10 (this is done outside the function!)
```

> ## Challenge - Functions
>
> 1. Change the values of the arguments in the function and check its output
> 2. Try calling the function by giving it the wrong number of arguments (not 2)
>   or not assigning the function call to a variable (no `product_of_inputs =`)
> 3. Declare a variable inside the function and test to see where it exists (Hint:
>   can you print it from outside the function?)
> 4. Explore what happens when a variable both inside and outside the function
>   have the same name. What happens to the global variable when you change the
>   value of the local variable?
{: .challenge}

We can now turn our code for saving ward data files into a function. There are
many different "chunks" of this code that we can turn into functions, and we can
even create functions that call other functions inside them. Let's first write a
function that separates data for just one ward and saves that data to a file:

```python
def one_ward_csv_writer(this_ward, all_data):
    """
    Writes a csv file for data from a given ward.

    this_ward --- ward for which data is extracted
    all_data --- DataFrame with multi-ward data
    """

    # Select data for the ward
    rainfall_ward = all_data[all_data.ward_id == this_ward]

    # Write the new DataFrame to a csv file
    filename = 'data_output/ward_files/function_rainfall' + str(this_ward) + '.csv'
    rainfall_ward.to_csv(filename)
```

The text between the two sets of triple double quotes is called a docstring and
contains the documentation for the function. It does nothing when the function
is running and is therefore not necessary, but it is good practice to include
docstrings as a reminder of what the code does. Docstrings in functions also
become part of their 'official' documentation:

```python
one_ward_csv_writer?
```

```python
one_ward_csv_writer(1,rainfall_df)
```

We changed the root of the name of the csv file so we can distinguish it from
the one we wrote before. Check the `ward_files` directory for the file. Did it
do what you expect?

What we really want to do, though, is create files for multiple wards without
having to request them one by one. Let's write another function that replaces
the entire For loop by simply looping through a sequence of wards and repeatedly
calling the function we just wrote, `one_ward_csv_writer`:


```python
def ward_data_csv_writer(start_ward, end_ward, all_data):
    """
    Writes separate csv files for each ward of data.

    start_ward --- the first ward of data we want
    end_ward --- the last ward of data we want
    all_data --- DataFrame with multi-ward data
    """

    # "end_ward" is the last ward of data we want to pull, so we loop to end_ward+1
    for ward in range(start_ward, end_ward+1):
        one_ward_csv_writer(ward, all_data)
```

Because people will naturally expect that the end ward for the files is the last
ward with data, the stop value for the `range()` function has to be `end_ward + 1`. `range()` 
eturns an object that produces a sequence of integers from start (inclusive)
to stop (exclusive)
By writing the entire loop into a function, we've made a reusable tool for whenever
we need to break a large data file into individual files. Because we can specify the
first and last ward for which we want files, we can even use this function to
create files for a subset of the wards available. This is how we call this
function:

```python
# Load the data into a DataFrame
rainfall_df = pd.read_csv('data/rainfall_combined.csv')

# Create csv files
ward_data_csv_writer(1, 110, rainfall_df)
```

BEWARE! If you are using IPython Notebooks and you modify a function, you MUST
re-run that cell in order for the changed function to be available to the rest
of the code. Nothing will visibly happen when you do this, though, because
simply defining a function without *calling* it doesn't produce an output. Any
cells that use the now-changed functions will also have to be re-run for their
output to change.

> ## Challenge- More functions
>
> 1. Add two arguments to the functions we wrote that take the path of the
>    directory where the files will be written and the root of the file name.
>    Create a new set of files with a different name in a different directory.
> 2. How could you use the function `ward_data_csv_writer` to create a csv file
>    for only one ward? (Hint: think about the syntax for `range`)
> 3. Make the functions return a list of the files they have written. There are
>    many ways you can do this (and you should try them all!): either of the
>    functions can print to screen, either can use a return statement to give back
>    numbers or strings to their function call, or you can use some combination of
>    the two. You could also try using the `os` library to list the contents of
>    directories.
> 4. Explore what happens when variables are declared inside each of the functions
>    versus in the main (non-indented) body of your code. What is the scope of the
>    variables (where are they visible)? What happens when they have the same name
>   but are given different values?
{: .challenge}

The functions we wrote demand that we give them a value for every argument.
Ideally, we would like these functions to be as flexible and independent as
possible. Let's modify the function `ward_data_csv_writer` so that the
`start_ward` and `end_ward` default to the full range of the data if they are
not supplied by the user. Arguments can be given default values with an equal
sign in the function declaration. Any arguments in the function without default
values (here, `all_data`) is a required argument and MUST come before the
argument with default values (which are optional in the function call).

```python
    def ward_data_arg_test(all_data, start_ward = 1, end_ward = 110):
        """
        Modified from ward_data_csv_writer to test default argument values!

        start_ward --- the first ward of data we want --- default: 1
        end_ward --- the last ward of data we want --- default: 110
        all_data --- DataFrame with multi-ward data
        """

        return start_ward, end_ward


    start,end = ward_data_arg_test (rainfall_df, 40, 50)
    print('Both optional arguments:\t', start, end)

    start,end = ward_data_arg_test (rainfall_df)
    print('Default values:\t\t\t', start, end)
```

```
    Both optional arguments:	40 50
    Default values:			1 110
```

The "\t" in the `print` statements are tabs, used to make the text align and be
easier to read.

But what if our dataset doesn't start in 1 and end in 110? We can modify the
function so that it looks for the start and end wards in the dataset if those
dates are not provided:

```python
    def ward_data_arg_test(all_data, start_ward = None, end_ward = None):
        """
        Modified from ward_data_csv_writer to test default argument values!

        start_ward --- the first ward of data we want --- default: None - check all_data
        end_ward --- the last ward of data we want --- default: None - check all_data
        all_data --- DataFrame with multi-ward data
        """

        if not start_ward:
            start_ward = min(all_data.ward_id)
        if not end_ward:
            end_ward = max(all_data.ward_id)

        return start_ward, end_ward


    start,end = ward_data_arg_test (rainfall_df, 40, 50)
    print('Both optional arguments:\t', start, end)

    start,end = ward_data_arg_test (rainfall_df)
    print('Default values:\t\t\t', start, end)
```
```
    Both optional arguments:	40 50
    Default values:			1 110
```

The default values of the `start_ward` and `end_ward` arguments in the function
`ward_data_arg_test` are now `None`. This is a build-it constant in Python
that indicates the absence of a value - essentially, that the variable exists in
the namespace of the function (the directory of variable names) but that it
doesn't correspond to any existing object.

> ## Challenge - Variables
>
> 1. What type of object corresponds to a variable declared as `None`? (Hint:
> create a variable set to `None` and use the function `type()`)
>
> 2. Compare the behavior of the function `ward_data_arg_test` when the
> arguments have `None` as a default and when they do not have default values.
>
> 3. What happens if you only include a value for `start_ward` in the function
> call? Can you write the function call with only a value for `end_ward`? (Hint:
> think about how the function must be assigning values to each of the arguments -
> this is related to the need to put the arguments without default values before
> those with default values in the function definition!)
{: .challenge}

## If Statements

The body of the test function now has two conditionals (if statements) that
check the values of `start_ward` and `end_ward`. If statements execute a segment 
of code when some condition is met. They commonly look something like this:

```python
    a = 5

    if a<0: # meets first condition?

        # if a IS less than zero
        print('a is a negative number')

    elif a>0: # did not meet first condition. meets second condition?

        # if a ISN'T less than zero and IS more than zero
        print('a is a positive number')

    else: # met neither condition

        # if a ISN'T less than zero and ISN'T more than zero
        print('a must be zero!')
```

Which would return:

```
    a is a positive number
```

Change the value of `a` to see how this function works. The statement `elif`
means "else if", and all of the conditional statements must end in a colon.

The if statements in the function `ward_data_arg_test` check whether there is an
object associated with the variable names `start_ward` and `end_ward`. If those
variables are `None`, the if statements return the boolean `True` and execute whatever
is in their body. On the other hand, if the variable names are associated with
some value (they got a number in the function call), the if statements return `False`
and do not execute. The opposite conditional statements, which would return
`True` if the variables were associated with objects (if they had received value
in the function call), would be `if start_ward` and `if end_ward`.

As we've written it so far, the function `ward_data_arg_test` associates
values in the function call with arguments in the function definition just based
in their order. If the function gets only two values in the function call, the
first one will be associated with `all_data` and the second with `start_ward`,
regardless of what we intended them to be. We can get around this problem by
calling the function using keyword arguments, where each of the arguments in the
function definition is associated with a keyword and the function call passes
values to the function using these keywords:

```python
    def ward_data_arg_test(all_data, start_ward = None, end_ward = None):
        """
        Modified from ward_data_csv_writer to test default argument values!

        start_ward --- the first ward of data we want --- default: None - check all_data
        end_ward --- the last ward of data we want --- default: None - check all_data
        all_data --- DataFrame with multi-ward data
        """

        if not start_ward:
            start_ward = min(all_data.ward)
        if not end_ward:
            end_ward = max(all_data.ward)

        return start_ward, end_ward


    start,end = ward_data_arg_test (rainfall_df)
    print('Default values:\t\t\t', start, end)

    start,end = ward_data_arg_test (rainfall_df, 40, 50)
    print('No keywords:\t\t\t', start, end)

    start,end = ward_data_arg_test (rainfall_df, start_ward = 40, end_ward = 50)
    print('Both keywords, in order:\t', start, end)

    start,end = ward_data_arg_test (rainfall_df, end_ward = 40, start_ward = 50)
    print('Both keywords, flipped:\t\t', start, end)

    start,end = ward_data_arg_test (rainfall_df, start_ward = 40)
    print('One keyword, default end:\t', start, end)

    start,end = ward_data_arg_test (rainfall_df, end_ward = 50)
    print('One keyword, default start:\t', start, end)
```
```
    Default values:			1 110
    No keywords:			40 50
    Both keywords, in order:	40 50
    Both keywords, flipped:		40 50
    One keyword, default end:	40 110
    One keyword, default start:	1 50
```

> ## Challenge - Modifying functions
>
> 1. Rewrite the `one_ward_csv_writer` and `ward_data_csv_writer` functions to
> have keyword arguments with default values
>
> 2. Modify the functions so that they don't create ward files if there is no
> data for a given ward and display an alert to the user (Hint: use conditional
> statements to do this. For an extra challenge, use `try`
> statements!)
>
> 3. The code below checks to see whether a directory exists and creates one if it
> doesn't. Add some code to your function that writes out the CSV files, to check
> for a directory to write to.
>
> ```python
>	if 'dir_name_here' in os.listdir('.'):
>	    print('Processed directory exists')
>	else:
>	    os.mkdir('dir_name_here')
>	    print('Processed directory created')
> ```
>
> 4. The code that you have written so far to loop through the wards is good,
> however it is not necessarily reproducible with different datasets.
> For instance, what happens to the code if we have additional wards of data
> in our CSV files? Using the tools that you learned in the previous activities,
> make a list of all wards represented in the data. Then create a loop to process
> your data, that begins at the earliest ward and ends at the latest ward using
> that list.
>
> HINT: you can create a loop with a list as follows: `for wards in ward_list:`
{: .challenge}
