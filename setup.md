---
layout: page
title: Setup
---

> ## Data
> For this lesson, we will use
rainfall data from eThekwini from December 2017.
The files can be downloaded using this [google link](https://drive.google.com/open?id=1WOd94TOmvcnotwMYjY-2xmsip9EiGT7V).
{: .prereq}



> ## Software
> [Python](http://python.org) is a popular language for
> scientific computing, and great for general-purpose programming as
> well.  Installing all of its scientific packages individually can be
> a bit difficult, so we recommend an all-in-one installer.
>
> For this workshop we use Python version 3.x.
>
> ### Required Python Packages for this workshop
>
> * [Pandas](http://pandas.pydata.org/)
> * [Numpy](http://www.numpy.org/)
> * [Matplotlib](http://matplotlib.org/)
{: .prereq}

## Install the workshop packages

For installing these packages we will use Anaconda or Miniconda.
They both use [Conda](http://conda.pydata.org/docs/), the main difference is
that Anaconda comes with a lot of packages pre-installed.
With Miniconda you will need to install the required packages.

### Anaconda installation

Anaconda will install the workshop packages for you.

#### Download and install Anaconda

Download and install [Anaconda](https://www.continuum.io/downloads).
Remember to download and install the installer for Python 3.x.

### Miniconda installation

Miniconda is a "light" version of Anaconda. If you install and use Miniconda
you will also need to install the workshop packages.

#### Download and install Miniconda

Download and install [Miniconda](http://conda.pydata.org/miniconda.html)
following the instructions. Remember to download and run the installer for
Python 3.x.

#### Check the installation of Miniconda

From the terminal, type:

~~~
conda list
~~~
{: .language-bash}

### Install the required workshop packages with conda

From the terminal, type:

~~~
conda install -y numpy pandas matplotlib
~~~
{: .language-bash}