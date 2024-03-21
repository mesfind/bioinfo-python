---
title: Introduction to Python Datatypes and Packages
teaching: 1
exercises: 0
questions:
- "What are the basic datatypes I can use in Python?"
- "How do I define a function?"
- "How do I write documentation for my Python code?"
- "How do I install and manage packages?"
objectives:
- "Brief overview of basic datatypes like lists, tuples, & dictionaries."
- "Syntax for defining a function."
- "Recommendations for proper code documentation."
- "Installing, updating, and importing packages."
- "Verify that everyone's Python environment is ready."
keypoints:
- "Are you flying yet?"
---



# Python's Basic Built-in Datatypes

## Strings, integers and floats

The most basic data types in Python are strings, integers and floats:

~~~
text = "Go Cyclones"
number = 42
pi_value = 3.1415
~~~
{: .python}

Here we've assigned data to variables, namely `text`, `number` and `pi_value`,
using the assignment operator `=`. The variable called `text` is a string which
means it can contain letters and numbers. We could reassign the variable `text`
to an integer too - but be careful reassigning variables as this can get
confusing.

To print out the value stored in a variable we can simply type the name of the
variable into the interpreter:

~~~
text
~~~
{: .python}

~~~
Go Cyclones
~~~
{: .output}

however, in scripts we must use the `print` function:

~~~
# Comments start with #
# Next line will print out text
print(text)
~~~
{: .python}

~~~
Go Cyclones
~~~
{: .output}



## Sequential types: Lists and Tuples

### Lists

**Lists** are a common data structure to hold an ordered sequence of
elements. Each element can be accessed by an index.  Remember that Python indices start with 0 instead of 1:

~~~
numbers = [1,2,3]
numbers[0]
~~~
{: .python}

~~~
1
~~~
{: .output}

A `for` loop can be used to access the elements in a list or other Python data structure one at a time:

~~~
for num in numbers:
    print(num)
~~~
{: .python}

~~~
1
2
3
~~~
{: .output}

**Remember _indentation_ is very important in Python.** Note that the second line in the
example above is indented. This is Python's way of marking a block of code.

To add elements to the end of a list, we can use the `append` method:

~~~
numbers.append(4)
print(numbers)
~~~
{: .python}

~~~
[1,2,3,4]
~~~
{: .output}

Methods are a way to interact with an object (a list, for example). We can invoke
a method using the dot `.` followed by the method name and a list of arguments in parentheses.
To find out what methods are available for an object, we can use the built-in `help` command (you can exit the help page by typing `q`):

~~~
help(numbers)
~~~
{: .python}

~~~
Help on list object:

class list(object)
 |  list(iterable=(), /)
 |
 |  Built-in mutable sequence.
 |  
 |  If no argument is given, the constructor creates a new empty list.
 |  The argument must be an iterable if specified.
 |  
 |  Methods defined here:
 |  
 |  __add__(self, value, /)
 |      Return self+value.
 ...
~~~
{: .output}

We can also access a list of methods using `dir`. Some methods names are
surrounded by double underscores. Those methods are called "special", and
usually we access them in a different way. For example `__add__` method is
responsible for the `+` operator.

~~~
dir(numbers)
~~~
{: .python}

~~~
['__add__', '__class__', '__contains__', ...]
~~~
{: .output}

### Tuples

A tuple is similar to a list in that it's an ordered sequence of elements. However,
tuples cannot be changed once created (they are "immutable"). Tuples are
created by placing comma-separated values inside parentheses `()`.

~~~
# tuples use parentheses
my_tuple = (0,1,2)
colors = ('red','blue','green')
# Note: lists use square brackets
my_list = [0,1,2]
~~~
{: .python}


> ## What happens when you try to change a list versus a tuple?
>
> Change the value of a single element in `my_list ` and `my_tuple`.
>
> > ## Solution: List
> >
> > ~~~
> > my_list[1] = 5
> > print("my_list =", my_list)
> > ~~~
> > {: .python}
> >
> > ~~~
> > my_list = [0, 5, 2]
> > ~~~
> > {: .output}
> {: .solution}
>
> > ## Solution: Tuple
> >
> > ~~~
> > my_tuple[1] = 5
> > ~~~
> > {: .python}
> >
> > ~~~
> >Traceback (most recent call last):
> >  File "<stdin>", line 1, in <module>
> >TypeError: 'tuple' object does not support item assignment
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Once created, tuples cannot be changed in any way. This means you cannot add new values to your tuple container. As a result, Python stores these variables differently from lists since there is no need to allocate any additional memory for potential changes in size. This, in turn, makes tuples more memory efficient. 

### Assigning Elements of a List or Tuple to New Variables

One very handy feature in Python is that 
the individual elements of a list or tuple can 
be assigned to individual variable names.
For example you can create a list:

~~~
my_list = ['Cyclones','Go','!']
~~~
{: .python}

Then assign the individual values of that list 
to new separate variables just by separating 
the names with commas:

~~~
v1, v2, v3 = my_list
print(v2, v1, v3)
~~~
{: .python}

~~~
Go Cyclones !
~~~
{: .output}

This works the same for a tuple:

~~~
my_tup = ('U','I','S')
c, a, b = my_tup
print(a, b, c)
~~~
{: .python}

~~~
I S U
~~~
{: .output}



### Mixing Types

In Python, it is also possible to mix types in a list or tuple. This makes it easy to store data of variable type.
Thus, you can have a list that contains both natural numbers, floating point numbers, strings, lists, and tuples:

~~~
my_data = [('Manidae', 'Phataginus', 'tricuspis', 1821), 23.56, 30, [10.8,8.45,9.33]]
~~~
{: .python}

## Dictionaries

A **dictionary** is a container that holds pairs of objects - keys and values. This datatype is one of the most useful types in Python.

~~~
numbers = {"one" : 1, "two" : 2}
numbers["one"]
~~~
{: .python}

~~~
1
~~~
{: .output}

Dictionaries work a lot like lists - except that you index them with *keys*.
You can think about a key as a name for or a unique identifier for a set of values
in the dictionary. Keys can only have particular types - they have to be
"hashable", which means they don't change. Strings, numeric types, and tuples are acceptable, but lists are not.

Here is a dictionary with natural numbers as keys. You access the values associated with each key, by providing the key in `[]`

~~~
numbers2 = {1 : "one", 2 : "two"}
numbers2[1]
~~~
{: .python}

~~~
'one'
~~~
{: .output}

You can also use a tuple as a key, but only if the tuple contains hashable elements (strings, numbers, tuples):

~~~
my_dict2 = {(1,2,3) : 3}
my_dict2
~~~
{: .python}

~~~
{(1, 2, 3): 3}
~~~
{: .output}

However, you cannot create a dictionary with a list as a key:

~~~
bad_dictionary = {[1,2,3] : 3}
~~~
{: .python}

~~~
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
~~~
{: .output}


To add an item to the dictionary we assign a value to a new key:

~~~
numbers2 = {1 : 'one', 2 : 'two'}
numbers2[3] = 'three'
numbers2
~~~
{: .python}

~~~
{1: 'one', 2: 'two', 3: 'three'}
~~~
{: .output}

It is often necessary to check if the dictionary has a key before adding it, otherwise you will overwrite the value associated with that key:

~~~
if 4 not in numbers2:
    numbers2[4] = 'four'

numbers2
~~~
{: .python}

~~~
{1: 'one', 2: 'two', 3: 'three', 4: 'four'}
~~~
{: .output}


Using `for` loops with dictionaries is a little more complicated. We can do this
in two ways.

We can use the method `.items()` to return the key-value pair (this is the recommended way of iterating over a dictionary in Python):

~~~
for key, value in numbers2.items():
    print(key, "->", value)
~~~
{: .python}

~~~
1 -> one
2 -> two
3 -> three
4 -> four
~~~
{: .output}

Alternatively, you can iterate over the keys. When you loop over the dictionary in this way, Python just assigns the key value to the iterator variable:

~~~
for key in numbers2:
    print(key, "->", numbers2[key])
~~~
{: .python}

~~~
1 -> one
2 -> two
3 -> three
4 -> four
~~~
{: .output}


> ## Can you reassign the value of an element in a dictionary?
>
> Try to reassign the second value of `numbers2` (in the *key value pair*) so that it no longer reads "two" but instead reads "spam and eggs". What does your dictionary look like after you make this change?
>
> > ## Solution
> >
> > ~~~
> > numbers2[2] = 'spam and eggs'
> > numbers2
> > ~~~
> > {: .python}
> >
> > ~~~
> >{1: 'one', 2: 'spam and eggs', 3: 'three', 4: 'four'}
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

It is important to note that dictionaries in Python 3.6 or greater are insertion _ordered_ and preserve the
sequence of their items (i.e., the order in which key:value pairs were added to
the dictionary). In previous versions of Python, dictionaries are unordered, where the order in which items are returned from loops
over dictionaries might appear random and can even change with time.


Regardless of the order in which they were entered, if the dictionary's keys are of a single type that can be sorted (numbers or strings), then you can get a sorted list of the keys and iterate over it in a loop.

A loop over the values of an unsorted dictionary:

~~~
d = {3 : 'three', 2 : 'two', 6 : 'six', 5 : 'five'}
for value in d.values():
    print(value)
~~~
{: .python}

~~~
three
two
six
five
~~~
{: .output}

To get a list of sorted keys, you can use the `sorted()` function on the list returned by the `.keys()` method:

~~~
d = {3 : 'three', 2 : 'two', 6 : 'six', 5 : 'five'}
for key in sorted(d.keys()):
    print(key, ' = ', d[key])
~~~
{: .python}

~~~
2  =  two
3  =  three
5  =  five
6  =  six
~~~
{: .output}


## Functions

Defining part of a program in Python as a function is done using the `def`
keyword. For example a function that takes two arguments and returns their sum
can be defined as:

~~~
def add_function(a, b):
    result = a + b
    return result

z = add_function(20, 22)
print(z)
~~~
{: .python}

~~~
42
~~~
{: .output}

The important members of a function are the input **arguments** and the **return values**. 
(Note that not all functions have these components.)

In the function called `add_function(a,b)` above
the arguments are `a` and `b` and the returned
variable is `result`.  

One very handy feature of Python is that
a function can return _multiple variables_. 
This essentially returns the objects as
a _tuple_. 

~~~
def fun_function(lett_str, num_str):
    lett_up = lett_str.upper()        # string.upper() converts to uppercase
    int_num = int(num_str)            # int(string) converts to an integer
    return lett_up, int_num+1         # returns a tuple (increments num +1)
~~~
{: .python}

Thus, you can assign the 
multiple return values to different variables
when calling the function.

~~~
a, b = fun_function("octothorpe", "1886")
print(a)
print(b)
~~~
{: .python}

~~~
OCTOTHORPE
1887
~~~
{: .output}

Fun fact: [_octothorpe_](https://en.wiktionary.org/wiki/octothorpe) is a word from the 1960s or 1970s that was used to describe a hash symbol: `#`.


# Properly Documenting Your Code

When writing programs, code, scripts, etc., it is very 
important to provide good documentation of your work.
This is because you want your science to be reproducible 
by anyone who evaluates your work or wishes to build
on your findings. But, even more importantly, you
need to properly document your code so that **_you_**
know what your own programs and scripts do when you open your 
them again after spending several months finishing
some other project. 

Here are some basic tips for writing code 
documentation using comments and/or Markdown formatted
descriptions in a notebook.

## Header Documentation
It is good to always provide a description of code files.
This is done at the very top of the file or notebook. 
The window below gives an example of header comments
in a Jupyter notebook. This header provides a description
of the contents of the file, lists any packages or files
that it requires to execute without errors, and gives 
the names of the authors and date it was written. 

![Header docs](../fig/02-header-docs.png)

## Function Documentation
When writing functions in your code, it is important
to provide details about how the function works. You 
can do this as comments or Markdown formatted text in 
your notebook. When written _above_ the function 
definition, this helps to make it clear that the 
code below is a function. An added benefit of using
Jupyter notebooks is that you can create hyperlinks
and other references in your documentation. This 
is a nice way to cite sources and provide clear 
information. See the example from a 
Jupyter notebook below. 

![Func docs](../fig/02-func-docs.png)



## Function Docstrings
While function comments and Markdown descriptions are 
great for making it easy to find functions in a file
and read information about them, Python also has
a way for you to provide information about a function
that can be retrieved from the function _itself_. 
These are called *Docstrings* and are inserted right 
after the first line instantiating the function. 

~~~
def cat_two_strings(a, b):
    """Description: Concatenates two strings
    
    Arguments: 
       a  :  the first string
       b  :  the second string
    
    Return: a string that is a + b
    
    Example of usage:
       >>> string_a = "howdy "
       >>> string_b = "world"
       >>> ab_string = cat_two_strings(string_a, string_b)
       >>> print(ab_string)
       
       Output:
          howdy world
    """
    return a + b
~~~
{: .python}

The Docstring is contained within 
triple quotes: `"""Docstring"""`. And after a function
is defined, the information from the Docstring is 
available using the `help()` function:

~~~
help(cat_two_strings)
~~~
{: .python}

~~~
Help on function cat_two_strings in module __main__:

cat_two_strings(a, b)
     Description: Concatenates two strings
    
     Arguments: 
        a  :  the first string
        b  :  the second string
    
     Return: a string that is a + b
    
     Example of usage:
       >>> string_a = "howdy "
       >>> string_b = "world"
       >>> ab_string = cat_two_strings(string_a, string_b)
       >>> print(ab_string)
       
        Output:
          howdy world
~~~
{: .output}

## In-line Comments
You may also find it necessary to add comments
within your code. In Python comments are 
preceded by an octothorpe (i.e., `#`). It really is up to you 
to decide when and how to use such comments.
Ultimately, this type of documentation is 
really for adding information or details 
in places where it isn't obvious from the 
code what is going on. Another use for
in-line comments is to add flags for debugging
or "to-dos" for work that is currently in progress. 
In these cases, the comments should be written on the line
above the code it refers to or just to the 
right on the same line.

~~~
def cat_two_strings(a, b):
   return a + b    # TODO: verify that a & b are strings
~~~
{: .python}

For experienced programmers, it is probably best to avoid too many superfluous comments in your code. 
This is because code with lots of comments may be difficult to read. And you should work
to write code that can be understood without comments. 

Look at the comments below. Are they really all that necessary? Do these in-line comments distract from the actual written code?

![Inline](../fig/02-excess-comments.png)

Nevertheless, even the comments above may be helpful to someone new to Python. And as you learn
a new programming language, you may find it helpful to annotate your scripts this way or to 
add notes that help you remember important syntax details, etc. 

<!-- ~~~
# Below are some totally obvious in-line comments
for i in range(10):  # a for-loop over a list of 10 elements
    print(i)         # print element i
~~~
{: .python}
 -->

# Python Packages

There is an immense number of Python packages (also called libraries) out there that do a lot of different things. Python doesn't have a heavily managed central resource like CRAN for R, but you can find a long and probably incomplete [list of packages for Python online](https://pypi.python.org/pypi/). Additionally, Anaconda provides easy installs of over 600 packages listed [here](https://anaconda.org/anaconda/repo).

## Installing New Packages

Anaconda helps install many important Python packages. You can use `conda` to install others that do not come with the default install. One very useful Python library for bioinformatics is [biopython](http://biopython.org/). You can install this it easily with `conda` from your terminal window.

```
$ conda install biopython
```

The prompt will ask you if you want to proceed with installing this package. Simply type `y` followed by the enter key and `conda` will manage the download and installation.

For packages that are not available via Anaconda, you may also be able to use the application `pip` to install. 
<!-- If you like using ggplot2 in R, you may like to use it in Python. The Python [ggplot](http://ggplot.yhathq.com/) package can be installed using `pip`: -->
One the package [`string-color`](https://pypi.org/project/string-color/) allows you to print strings to screen in different colors. 

```
$ pip install string-color
```

## Updating Packages

Anaconda makes it easy to update packages in `conda`. For any individual package, you can use `update`. The example below updates the package [pandas](http://pandas.pydata.org/), which allows us to deal with complex data structures.

```
$ conda update pandas
```

Alternatively, you can update all of the packages managed by Anaconda using:

```
$ conda update --all
```

For packages installed using `pip`, you can use the `-U` flag or `--upgrade`.

```
$ pip install string-color -U
```

## Importing Packages

Once you have a package installed, you don't have to do that again (except you may have to update them from time-to-time). However, you cannot use a package in any Python instance without importing it first. This is quite simple.

Open a Python interpreter and type:

~~~
import stringcolor
~~~
{: .python}
With this package, you can now try to print in color: `print(stringcolor.cs("PURPLE", "purple"))`

For convenience, you can also give packages aliases in your scripts. That way you don't have to type out the whole name each time. The example below imports the Python package [numpy](http://www.numpy.org/) and uses the `numpy.sum()` function to add up the values in a list. Notice that `np` is an alias for `numpy`.

~~~
import numpy as np
np.sum([2,4,6])
~~~
{: .python}

~~~
12
~~~
{: .output}

### Just for Fun

~~~
import antigravity
~~~
{: .python}

And sage advice:

~~~
import this
~~~
{: .python}

> ## Take-Home Challenge: Create a Documented Jupyter Notebook Exploring Python Strings and Lists
>
> Create a new Jupyter notebook and document the following exercises with explanations about your solutions.
>
> 1. Create a string `a = 'Go Cyclones'` and use python to extract the character `'l'` from that string and assign it to a new variable. What index is that character assigned in the string?
>
> 2. Convert your string `a` so that it is spelled out backwards `'senolcyC oG'`. (Try using a for loop, or you can find examples of how to do this with Python subsetting techniques.)
>
> 3. Create two lists `l1 = [53, 16, 1, 86, 3, 33, 41, 76, 71, 4]` and `l2 = [30, 100, 94, 90, 17, 11, 39, 23, 90, 52]`. Use python to create a new list that contains only the odd numbers from both lists. The third list should look like this: `[53, 1, 3, 33, 41, 71, 17, 11, 39, 23]`.
>
> > ## Solutions
> > 
> > (1) The index for character `l` is `a[6]`.
> > 
> > ~~~
> > a = 'Go Cyclones'
> > b = a[6]
> > print(b)
> > ~~~
> > {: .python}
> > 
> > ~~~
> > l
> > ~~~
> > {: .output}
> > 
> > <br> 
> > 
> > (2) Use list indexing notation to reverse the string.
> > 
> > ~~~
> > print(a[::-1])
> > ~~~
> > {: .python}
> > 
> > ~~~
> > senolcyC oG
> > ~~~
> > {: .output}
> > 
> > <br> 
> > 
> > (3) Use the modulo operator `%` to determine if the list element is divisible by `2`.
> > 
> > ~~~
> > l1 = [53, 16, 1, 86, 3, 33, 41, 76, 71, 4]
> > l2 = [30, 100, 94, 90, 17, 11, 39, 23, 90, 52]
> > 
> > for n in l1+l2:
> >     if n % 2 != 0:
> >         l3.append(n)
> > 
> > print(l3)
> > ~~~
> > {: .python}
> > 
> > ~~~
> > [53, 1, 3, 33, 41, 71, 17, 11, 39, 23]
> > ~~~
> > {: .output}
> > 
> > 
> {: .solution}
{: .challenge}