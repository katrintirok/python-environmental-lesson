
# Introduction to python
 ***Data Carpentry contributors***
minutes: 45


### Learning Objectives

* Define the following terms as they relate to python: variables, objects, assign, call, function, arguments, options.
* Create variables and and assign values to them.
* Use comments to inform script.
* Do simple arithmetic operations in python using values and variables.
* Call functions and use arguments to change their default options.
* Inspect the content of lists and manipulate their content.
* Use libraries in python.
* Create and manipulate numPy arrays.
* Subset and extract values from lists and arrays.
* Correctly define and handle missing values.



## Creating variables in python

You can get output from python simply by typing math in the console:

```
3 + 5
12 / 7
```

However, to do useful and interesting things, we need to assign values to _variables_ (or link _objects_ to names/variables). To create a variable, we need to give it a name followed by an `=` and the value we want to give it:

```
weight_kg = 55
```

`=` is the assignment operator. It assigns values on the right to variables/names on the left. So, after executing `x = 3`, the value of `x` is `3`. Assignments can be done on more than one variable "simultaneously" on the same line like this:

```
a, b = 3, 4
```

Variables can be given any name such as `x`, `current_temperature`, or
`subject_id`. You want your variable names to be explicit and not too long. They cannot start with a number (`2x` is not valid, but `x2` is). python is case sensitive (e.g., `weight_kg` is different from `Weight_kg`). There are some names that cannot be used because they are the names of fundamental functions in python (e.g., `if`, `else`, `for`,
see [here](https://docs.python.org/2.5/ref/keywords.html) for a complete list). In general, even if it's allowed, it's best to not use other function names (e.g., `list`, `mean`, `data`, `len`). If in doubt, check the help to see if the name is already in use. It's also not allowd to use a dot or minus (`.`, `-`) within a variable name as in `my.dataset` or `my-dataset`. 
It is also recommended to use nouns for variable names, and verbs for function names. It's important to be consistent in the styling of your
code (where you put spaces, how you name variables, etc.). A speciality of python is [indentation](https://docs.python.org/3/reference/lexical_analysis.html#indentation), which is used to define code blocks and consequently structures a program. While this is generally a good style for program code, in the case of python, it is a language requirement. Using a consistent coding style makes your code clearer to read for your future self and your collaborators. There are several style guides for python, e.g. [python's PEP 8](https://www.python.org/dev/peps/pep-0008/), [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html), and [The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/writing/style/). 

Just remember how we assigned a value to `weight_kg`:

```
weight_kg = 55
```

Now that python has `weight_kg` in memory, we can do arithmetic with it. We can perform mathematical calculations in python using the basic operators
 `+, -, /, *, %`. For instance, we may want to convert this weight into pounds (weight in pounds is 2.2 times the weight in kg):

```
2.2 * weight_kg
```

We can also change a variable's value by assigning it a new one:

```
weight_kg = 57.5
2.2 * weight_kg
```

This means that assigning a value to one variable does not change the values of other variables.  For example, let's store the animal's weight in pounds in a new variable, `weight_lb`:

```
weight_lb = 2.2 * weight_kg
```

and then change `weight_kg` to 100.

```
weight_kg = 100
```

What do you think is the current content of the object `weight_lb`? 126.5 or 220?

## Comments

The comment character in python is `#`, anything to the right of a `#` in a script will be ignored by python. It is useful to leave notes, and explanations in your scripts.
Spyder makes it easy to comment or uncomment a paragraph: after selecting the
lines you  want to comment press at the same time <kbd>Ctrl</kbd> (or <kbd>Cmd</kbd> on a Mac) and <kbd>1</kbd>. If you only want to comment out one line, you can put the cursor at any location of that line (i.e. no need to select the whole line), then press <kbd>Ctrl</kbd> + <kbd>1</kbd>.

> ## Challenge
>
> What are the values after each statement in the following?
>
> ```
> mass = 47.5            # mass?
> age  = 122             # age?
> mass = mass * 2.0      # mass?
> age  = age - 20        # age?
> mass_index = mass/age  # mass_index?
> ```

## Objects vs. variables

Everything in python is an `object`: numbers, lists, functions, even code. A `variable`, as we use the term in Python, is a *way to access* a specific object. It is a binding between a name and an object. 

In python, every object has an ID, a type, and a value. The ID is a unique identifier that identifies a particular instance of an object, the type identifies the class (e.g. integer, string, list) of an object, and the value is the contents of the object. Mutable objects can change their value; immutable objects cannot.


## Functions and their arguments
***--> need to fix, python no math function natively, need to import math ..., something on modules, some popular basic modules for science, such as math, statistics...***


Functions are "canned scripts" that automate more complicated sets of commands
including operations assignments, etc. Many functions are predefined, or can be made available by importing python *packages* (more on that later). A function usually gets one or more inputs called *arguments*. Functions often (but not always) return a *value*. A typical example would be the function `abs()`. The input (the argument) must be a number, and the return value (in fact, the output) is the absolute value of that number. Executing a function ('running it') is called *calling* the function. An example of a function call is:

```
b = abs(a)
```

Here, the value of `a` is given to the `abs()` function, the `abs()` function
calculates the absolute value, and returns the value which is then assigned to
variable `b`. This function is very simple, because it takes just one argument.

The return 'value' of a function need not be numerical (like that of `abs()`),
and it also does not need to be a single item: it can be a set of things, or
even a dataset. We'll see that when we read data files into python.

Arguments can be anything, not only numbers or filenames, but also other
objects. Exactly what each argument means differs per function, and must be
looked up in the documentation (see below). Some functions take arguments which may either be specified by the user, or, if left out, take on a *default* value: these are called *options*. Options are typically used to alter the way the function operates, such as whether it ignores 'bad values', or what symbol to use in a plot.  However, if you want something specific, you can specify a value of your choice which will be used instead of the default.

Let's try a function that can take multiple arguments: `round()`.

```
round(3.14159)
```

Here, we've called `round()` with just one argument, `3.14159`, and it has
returned the value `3`.  That's because the default is to round to the nearest
whole number. If we want more digits we can see how to do that by getting
information about the `round` function.  We can look at the help for this function using `help(round)`.

```
help(round)
```

We see that if we want a different number of digits, we can type `ndigits=2` or however many we want.

```
round(3.14159, ndigits = 2)
```

If you provide the arguments in the exact same order as they are defined you
don't have to name them:

```
round(3.14159, 2)
```

And if you do name the arguments, you can switch their order:

```
round(ndigits = 2, number = 3.14159)
```

It's good practice to put the non-optional arguments (like the number you're
rounding) first in your function call, and to specify the names of all optional arguments.  If you don't, someone reading your code might have to look up the definition of a function with unfamiliar arguments to understand what you're doing.

### Using modules and external libraries with python

The always available [built-in functions](https://docs.python.org/3/library/functions.html) in python are limited. To get to use more functions for e.g. mathematical operations, modules can be used. There are a couple of modules that come with the [python standard library](https://docs.python.org/3.6/library/index.html) including for example the `math` module, which provides access to mathematical functions and the `statistics` module for calculations of mathematical statistics.

Modules need to be imported using `import` and their functions can then be used by calling the name of the function together with the module name using the syntax `module_name.function_name`. Adding the module name with a `.` before the function name tells python where to find the function. 

For example the function `sqrt` is part of the module `math` and can be called using `math.sqrt()`:

```
import math
math.sqrt(9)
```

A big advantage of python is its expandability with external modules (or libraries). In this workshop we will make use of `numPy`, `pandas`, and `matplotlib` which are all part of the [SciPy ecosystem](https://scipy.org).

When importing libraries we can give them a nickname to shorten the command, e.g. `np` is commonly used as a nickname for the `numpy` module:

```
import numpy as np
np.cos(np.pi)
```

In the example above, we have imported numPy as np. This means we don't have to type out `numpy` each time we call a numPy function. `np.cos` calculates the cosinus of pi. Pi also exists as a function within numpy and can be called with `np.pi`.


## Data types

Individual values can be 'numbers', 'strings' or 'logical values' ('booleans', i.e. TRUE and FALSE). Numerical data types include integers (`int`),  floating point real value (`float`), and colmplex numbers (`complex`).

Data structures represent data types that can contain more than one value. Lists are the most versatile of python's compound data types. A list contains items separated by commas and enclosed within square brackets ([]). For example we can create a list of animal weights and assign it to a new variable `weight_g`:

```
weight_g = [50, 60, 65, 82]
weight_g
```

A list can also contain strings:

```
animals = ["mouse", "rat", "dog"]
animals
```

The quotes around "mouse", "rat", etc. are essential here. Without the quotes python will assume there are variables called `mouse`, `rat` and `dog`. As these variables don't exist in python's memory, there will be an error message.

Lists in python can also hold of items of different data type, i.e. a mix of numbers and strings:

```
streets = ["first", "second", "third", 1, 2, 3]
streets
```
You can use functions with lists, e.g. to get the number of elements in a list use `len()`:

```
len(animals)
```

The function `type` indicates the class of a variable, in this case a `list`:

```
type(animals)
```

Objects in python can have `methods`, which allow for changes (mutations) to the object. For example, you can use the `.append()` method to add other values to the end of your list:

```
weight_g.append(90)    # add 90 to the end of the list
weight_g
[50, 60, 65, 82, 90]
```

or use the `.remove()` method to delete the first occurrence of an object from the list:

```
animals.remove("rat")
animals
["mouse", "dog"]
```

Use `help(list)` to get an overview of all methods for lists. 

Other built-in data types in python are `tuples` and `dictionaries`.

As described earlier, external libraries or modules largely extend the functionality of python and they also provide more data structures. In this workshop we will use **numpy arrays** (defined with `np.array`) and **pandas dataframes**.

NumPy arrays are multidimensional containers of items of the same type. That is, unlike lists, numpy arrays can only contain e.g. integers or strings, but not both. They allow us doing so called vectorised operations, that is doing an operation on each element in the array, something that is not possible for python's lists. (add challenge on that and on type mutation)

Create a list ...
Create a numpy array ...
do the following calculations:
- list/array * 2
- list/array + 2
- what happens with the list/with the array?


Another very useful data structure when working with data/doing data analysis in python is a DataFrame from the pandas module. A DataFrame more or less represents a table, i.e. it has columns, with columns headers and rows with one observation per row. A column needs to be of the same data type, i.e. either numeric or character, but different columns can differ in their data type. That means, pandas DataFrames are ideal to hold observational data tables imported into python.

...

We just saw 2 of the 6 main **atomic vector** types (or **data types**) that R
uses: `"character"` and `"numeric"`. These are the basic building blocks that
all R objects are built from. The other 4 are:

* `"logical"` for `TRUE` and `FALSE` (the boolean data type)
* `"integer"` for integer numbers (e.g., `2L`, the `L` indicates to R that it's an integer)
* `"complex"` to represent complex numbers with real and imaginary parts (e.g.,
  `1 + 4i`) and that's all we're going to say about them
* `"raw"` that we won't discuss further

Vectors are one of the many **data structures** that R uses. Other important
ones are lists (`list`), matrices (`matrix`), data frames (`data.frame`),
factors (`factor`) and arrays (`array`).


> ### Challenge
>
>
> * We’ve seen that atomic vectors can be of type character,
>   numeric, integer, and logical. But what happens if we try to mix these types in
>   a single vector?
> <!-- * _Answer_: R implicitly converts them to all be the same type -->
>
> * What will happen in each of these examples? (hint: use `class()`
>   to check the data type of your objects):
>
>     ```r
>     num_char <- c(1, 2, 3, 'a')
>     num_logical <- c(1, 2, 3, TRUE)
>     char_logical <- c('a', 'b', 'c', TRUE)
>     tricky <- c(1, 2, 3, '4')
>     ```
>
> * Why do you think it happens?
> <!-- * _Answer_: Vectors can be of only one data type. R tries to convert (coerce)
>   the content of this vector to find a "common denominator". -->
>
> * You've probably noticed that objects of different types get
>   converted into a single, shared type within a vector. In R, we
>   call converting objects from one class into another class
>   _coercion_. These conversions happen according to a hierarchy,
>   whereby some types get preferentially coerced into other
>   types. Can you draw a diagram that represents the hierarchy of how
>   these data types are coerced?
> <!-- * _Answer_: `logical -> numeric -> character <-- logical` -->

```{r, echo=FALSE, eval=FALSE, purl=TRUE}
## We’ve seen that atomic vectors can be of type character, numeric, integer, and
## logical. But what happens if we try to mix these types in a single
## vector?

## What will happen in each of these examples? (hint: use `class()` to
## check the data type of your object)
num_char <- c(1, 2, 3, "a")

num_logical <- c(1, 2, 3, TRUE)

char_logical <- c("a", "b", "c", TRUE)

tricky <- c(1, 2, 3, "4")

## Why do you think it happens?

## You've probably noticed that objects of different types get
## converted into a single, shared type within a vector. In R, we call
## converting objects from one class into another class
## _coercion_. These conversions happen according to a hierarchy,
## whereby some types get preferentially coerced into other types. Can
## you draw a diagram that represents the hierarchy of how these data
## types are coerced?
```

## Subsetting vectors

If we want to extract one or several values from a vector, we must provide one
or several indices in square brackets. For instance:

```{r, results='show', purl=FALSE}
animals <- c("mouse", "rat", "dog", "cat")
animals[2]
animals[c(3, 2)]
```

We can also repeat the indices to create an object with more elements than the
original one:

```{r, results='show', purl=FALSE}
more_animals <- animals[c(1, 2, 3, 2, 1, 4)]
more_animals
```

R indices start at 1. Programming languages like Fortran, MATLAB, Julia, and R start
counting at 1, because that's what human beings typically do. Languages in the C
family (including C++, Java, Perl, and Python) count from 0 because that's
simpler for computers to do.

### Conditional subsetting

Another common way of subsetting is by using a logical vector. `TRUE` will
select the element with the same index, while `FALSE` will not:

```{r, results='show', purl=FALSE}
weight_g <- c(21, 34, 39, 54, 55)
weight_g[c(TRUE, FALSE, TRUE, TRUE, FALSE)]
```

Typically, these logical vectors are not typed by hand, but are the output of
other functions or logical tests. For instance, if you wanted to select only the
values above 50:

```{r, results='show', purl=FALSE}
weight_g > 50    # will return logicals with TRUE for the indices that meet the condition
## so we can use this to select only the values above 50
weight_g[weight_g > 50]
```

You can combine multiple tests using `&` (both conditions are true, AND) or `|`
(at least one of the conditions is true, OR):

```{r, results='show', purl=FALSE}
weight_g[weight_g < 30 | weight_g > 50]
weight_g[weight_g >= 30 & weight_g == 21]
```

Here, `<` stands for "less than", `>` for "greater than", `>=` for "greater than
or equal to", and `==` for "equal to". The double equal sign `==` is a test for
numerical equality between the left and right hand sides, and should not be
confused with the single `=` sign, which performs variable assignment (similar
to `<-`).

A common task is to search for certain strings in a vector.  One could use the
"or" operator `|` to test for equality to multiple values, but this can quickly
become tedious. The function `%in%` allows you to test if any of the elements of
a search vector are found:

```{r, results='show', purl=FALSE}
animals <- c("mouse", "rat", "dog", "cat")
animals[animals == "cat" | animals == "rat"] # returns both rat and cat
animals %in% c("rat", "cat", "dog", "duck", "goat")
animals[animals %in% c("rat", "cat", "dog", "duck", "goat")]
```

> ### Challenge (optional){.challenge}
>
> * Can you figure out why `"four" > "five"` returns `TRUE`?

```{r, echo=FALSE, purl=TRUE}
### Challenge (optional)
##
## * Can you figure out why `"four" > "five"` returns `TRUE`?
```

<!--
```{r, purl=FALSE}
## Answers
## * When using ">" or "<" on strings, R compares their alphabetical order. Here
##   "four" comes after "five", and therefore is "greater than" it.
```
-->


## Missing data

As R was designed to analyze datasets, it includes the concept of missing data
(which is uncommon in other programming languages). Missing data are represented
in vectors as `NA`.

When doing operations on numbers, most functions will return `NA` if the data
you are working with include missing values. This feature
makes it harder to overlook the cases where you are dealing with missing data.
You can add the argument `na.rm=TRUE` to calculate the result while ignoring
the missing values.

```{r, purl=FALSE}
heights <- c(2, 4, 4, NA, 6)
mean(heights)
max(heights)
mean(heights, na.rm = TRUE)
max(heights, na.rm = TRUE)
```

If your data include missing values, you may want to become familiar with the
functions `is.na()`, `na.omit()`, and `complete.cases()`. See below for
examples.


```{r, purl=FALSE}
## Extract those elements which are not missing values.
heights[!is.na(heights)]

## Returns the object with incomplete cases removed. The returned object is atomic.
na.omit(heights)

## Extract those elements which are complete cases.
heights[complete.cases(heights)]
```

> ### Challenge
>
> 1. Using this vector of length measurements, create a new vector with the NAs
> removed.
>
>     ```r
>     lengths <- c(10,24,NA,18,NA,20)
>     ```
>
> 2. Use the function `median()` to calculate the median of the `lengths` vector.

```{r,echo=FALSE, purl=TRUE}
## ### Challenge
## 1. Using this vector of length measurements, create a new vector with the NAs
## removed.
##
##    lengths <- c(10,24,NA,18,NA,20)
##
## 2. Use the function `median()` to calculate the median of the `lengths` vector.
```

Now that we have learned how to write scripts, and the basics of R's data
structures, we are ready to start working with the Portal dataset we have been
using in the other lessons, and learn about data frames.


<p style="text-align: right; font-size: small;">Page build on: `r format(Sys.time())`</p>
