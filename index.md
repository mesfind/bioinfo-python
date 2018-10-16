---
layout: lesson
root: .
---

# Welcome to the Python part of the BCB546 course!

The materials we will work through are a sample of the lessons created for [Data Carpentry](http://www.datacarpentry.org/python-ecology-lesson/00-short-introduction-to-Python/) and [Software Carpentry](http://swcarpentry.github.io/python-novice-inflammation). 
The goal of this section is to demonstrate the utility of Python for working with biological data. 

## Why Python?

Python is a great programming language that is relatively easy to learn and also very easy to read. 
For bioinformatics, you will find yourself needing to know this language. Many tools for dealing with genomic data are written in Python and knowing how to progam in Python allows you to modify these tools and assemble them together in cohesive pipelines. 
[Ethan White](http://whitelab.weecology.org/) outlines some nice reasons for using Python on his [Programming for Biologists site](http://www.programmingforbiologists.org/about/why-python/). Where he also references this [xkcd](https://xkcd.com/) comic:

![xkcd](http://imgs.xkcd.com/comics/python.png)

## The Lessons

Most of the lessons we will use for this course were written for the [Data Carpentry](http://www.datacarpentry.org/) series of workshops. In particular, we are using the [Data Analysis & Visualization in Python: Python for Ecologists](http://www.datacarpentry.org/python-ecology-lesson/) set of lessons. It is worth noting that one of the primary contributors and maintainers of this teaching material is [April Wright](https://paleantology.com/the-wright-lab/), a former postdoc in EEOB at Iowa State and now an assistant professor at Southeastern Louisiana University. 

The data we are using for this lesson are from the Portal Project Teaching Database -
[available on FigShare](https://figshare.com/articles/Portal_Project_Teaching_Database/1314459).
More details about the files we'll use and where to downlod them are available on the "[Setup](setup/)" page


> ## Prerequisites
>
> Learners need to understand the concepts of files and directories
> (including the working directory) and how to start a Python
> interpreter before tackling this lesson. This lesson references the Jupyter
> notebook although it can be taught through any Python interpreter.
> The commands in this lesson pertain to **Python 3**.
{: .prereq}

### Getting Started
To get started with installing Python, follow the directions given in the [Python section of the course Software page](https://eeob-biodata.github.io/EEOB-BCB-546X/software#python).
In addition to installing Python on your own computer, you will also need to download the data files used in the tutorials. Deatils for doing this are found in the [Setup](setup/) page.

### Format

These lessons will provide command line text and code in specific formats.

All commands that are intended to be executed in your Unix terminal will be shown with the `$` prompt. For example:

```
$ cd my_directory
$ pwd
```

All output from any execution will be shown with a black bar on the side:

~~~
/home/my_directory
~~~
{: .output}

All Python code will be given in boxes with a purple bar on the side and in purple text, with no prompt:

~~~
import numpy as np
a = 12
~~~
{: .python}
