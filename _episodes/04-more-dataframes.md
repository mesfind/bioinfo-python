---
title: Indexing, Slicing, and Subsetting DataFrames in Python
teaching: 60
exercises: 0
questions:
- "How do you extract data from columns and rows?"
- "How do you select subsets of the DataFrame?"
- "How do you reassign values within the DataFrame?"
objectives:
- "Extract data using column headings and index locations."
- "Use slicing to select sets of data from a DataFrame."
- "Use label and integer-based indexing to select ranges of data in a DataFrame."
- "Create a copy of a DataFrame."
- "Locate subsets of data using masks."
keypoints:
- "Python can be used to work with complex data structures."
---

# More on Pandas DataFrames

In the last lesson, we read a CSV file into a DataFrame and
saved it to a named object. With the data in memory, we performed basic math on the data, 
calculated summary statistics, and created plots of the data. In this
lesson, we will explore ways to access different parts of the data using indexing,
slicing and subsetting.

## Import Pandas and Load the Data

We will continue to use the surveys dataset that we worked with in the last
exercise. Import the DataFrame and load the CSV file:

~~~
import pandas as pd
surveys_df = pd.read_csv("surveys.csv")
~~~
{: .python}


# Indexing & Slicing in Python

We often want to work with subsets of a DataFrame object. There are
different ways to accomplish this including: using labels (column headings),
numeric ranges, or specific x,y index locations.


## Selecting Data Using Labels: Column Headings

We use square brackets `[]` to select a subset of a Python object. For example,
we can select all of data from a column named `species_id` from the `surveys_df`
DataFrame by name:

~~~
surveys_df['species_id']
~~~
{: .python}

You can also call the column as an attribute, which gives you the same output as above

~~~
surveys_df.species_id
~~~
{: .python}

~~~
0         NL
1         NL
2         DM
3         DM
4         DM
5         PF
6         PE
7         DM
...
~~~
{: .output}


We can create an new object that contains the data within the `species_id`
column as a pandas Series:

~~~
surveys_species = surveys_df['species_id']
~~~
{: .python}

If we wish to view a set of columns, then
we can pass a list of column names to select columns in the
order we would like them in our subset. This is useful when we need to reorganize the data.
_NOTE:_ If a column name is not contained in the DataFrame, an exception
(error) will be raised.

View the species and plot number columns from the DataFrame:

~~~
surveys_df[['species_id', 'plot_id']]
~~~
{: .python}

~~~
      species_id  plot_id
0             NL        2
1             NL        3
2             DM        2
3             DM        7
4             DM        3
5             PF        1
6             PE        2
7             DM        1
...
~~~
{: .output}

The order you specify the column names is the same order they appear in the 
subset:

~~~
surveys_df[['plot_id', 'species_id']]
~~~
{: .python}

~~~
       plot_id species_id
0            2         NL
1            3         NL
2            2         DM
3            7         DM
4            3         DM
5            1         PF
6            2         PE
7            1         DM
...
~~~
{: .output}


## Extracting Range Based Subsets: Slicing Subsets of Rows

Slicing using the `[]` operator selects a set of rows and/or columns from a
DataFrame. To slice out a set of rows, you must use the following syntax:
`data_frame[start:stop]`. 

To select rows 0, 1, and 2 you specify the rows using the index ranges. Note that the 
bounds you specify require that the start bound (`0`) is included in the subset and the stop bound
(`3`) is one index greater than the last row you want to include.

~~~
surveys_df[0:3]
~~~
{: .python}

~~~
   record_id  month  day  year  plot_id species_id sex  hindfoot_length     weight
0          1      7   16  1977        2         NL   M             32.0        NaN
1          2      7   16  1977        3         NL   M             33.0        NaN
2          3      7   16  1977        2         DM   F             37.0        NaN
~~~
{: .output}

> ## Python slice syntax
> The rules of Python slice syntax are as follows and also apply to 
> lists, stings, and other sequential datatypes. The following example shows 
> this using a list.
> 
> First create a list:
> 
> ~~~
> a = list(range(10))
> a
> ~~~
> {: .python}
> 
> ~~~
> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
> ~~~
> {: .output}
> 
> Print the value of indices 1 through 4:
> 
> ~~~
> a[0:5]
> ~~~
> {: .python}
> 
> ~~~
> [0, 1, 2, 3, 4]
> ~~~
> {: .output}
> 
> Print the list starting at index 4 through to the end:
> 
> ~~~
> a[4:]
> ~~~
> {: .python}
> 
> ~~~
> [4, 5, 6, 7, 8, 9]
> ~~~
> {: .output}
> 
> Print the first 4 elements in the list:
> 
> ~~~
> a[:4]
> ~~~
> {: .python}
> 
> ~~~
> [0, 1, 2, 3]
> ~~~
> {: .output}
> 
> Print the _last_ element in the list:
> 
> ~~~
> a[-1]
> ~~~
> {: .python}
> 
> ~~~
> 9
> ~~~
> {: .output}
> 
> The slice syntax includes a third component called the _step_. Where 
> `a[start:stop:step]` returns the list from the `start` index for every `step` up to the index
> before `stop`. The example below prints every third element in the list starting from `0`
> all the way to the end.
> 
> ~~~
> a[::3]
> ~~~
> {: .python}
> 
> ~~~
> [0, 3, 6, 9]
> ~~~
> {: .output}
{: .callout}

> ## Select a subset of rows from a column
>
> Combine selecting a subset with column headings and slice syntax for rows. Get
> every 5th row for rows 20-60, from the columns `plot_id`, `species_id`, and `sex`. 
>
> > <!--## Solution
> > 
> > ~~~
> > surveys_df[['plot_id', 'species_id','sex']][20:61:5]
> > ~~~
> > {: .python}
> > 
> > ~~~
> >     plot_id species_id sex
> > 20       14         DM   F
> > 25       15         DM   M
> > 30       15         DM   F
> > 35       16         OT   F
> > 40       23         DM   F
> > 45       19         DM   M
> > 50       21         DM   F
> > 55       20         DM   M
> > 60       23         DM   M
> > ~~~
> > {: .output}
> {: .solution}-->
{: .challenge}

## Changing Values in a DataFrame

We can reassign values within subsets of our DataFrame. But before we do that, let's make a 
copy of our DataFrame so as not to modify our original imported data. 

~~~
surveys_copy = surveys_df
~~~
{: .python}

Now set the first three rows of data in the DataFrame to 0 for every column

~~~
surveys_copy[0:3] = 0
~~~
{: .python}


Next, print the first 6 rows of `surveys_copy` using the `.head()` method: 

~~~
surveys_copy.head(6)
~~~
{: .python}

~~~
   record_id  month  day  year  plot_id species_id sex  hindfoot_length  \
0          0      0    0     0        0          0   0              0.0   
1          0      0    0     0        0          0   0              0.0   
2          0      0    0     0        0          0   0              0.0   
3          4      7   16  1977        7         DM   M             36.0   
4          5      7   16  1977        3         DM   M             35.0   
5          6      7   16  1977        1         PF   M             14.0   

   weight  
0     0.0  
1     0.0  
2     0.0  
3     NaN  
4     NaN  
5     NaN  
~~~
{: .output}

Now print the first 6 rows of `surveys_df`

~~~
surveys_df.head(6)
~~~
{: .python}

What is the difference between the two data frames? Did `surveys_copy = surveys_df` make a 
proper copy of the DataFrame?

## Referencing Objects vs. Copying Objects in Python

We might have thought that we were creating a fresh copy of the `surveys_df` values when we 
used `surveys_copy = surveys_df`. However, for objects of certain datatypes (like lists and 
DataFrames) the assignment operator `=` only copies by reference. 
That is, it creates a new variable `surveys_copy` that refers to the **same** 
object `surveys_df` refers to. 
This means that there is only one object 
(the DataFrame), and both `surveys_df` and `surveys_copy` refer to it. So when we assign 
the first 3 rows 
the value of 0 using the 
`surveys_copy` DataFrame, the `surveys_df` DataFrame is modified too. To create a fresh, _duplicate_ 
copy of the `surveys_df`
DataFrame we use the syntax `surveys_copy = surveys_df.copy()`. 
But first we have to read the `surveys_df` again 
because the current version contains the unintentional changes made to the first 3 rows.

~~~
surveys_df = pd.read_csv("surveys.csv")
surveys_copy= surveys_df.copy()
~~~
{: .python}

Now reassign the first three rows to have the value `0` for all columns:

~~~
surveys_copy[0:3] = 0
~~~
{: .python}

Print the first 5 rows of both DataFrames:

~~~
surveys_copy.head(5)
~~~
{: .python}

~~~
surveys_df.head(5)
~~~
{: .python}


Did both DataFrames get altered this time?

<!--## Slicing Subsets of Rows and Columns in Python

We can select specific ranges of our data in 
both the row and column directions
using either label or integer-based indexing.

- `loc`: indexing via *labels* or *integers*
- `iloc`: indexing via *integers*

To select a subset of rows AND columns from our DataFrame, we can use the `iloc`
index. For example, we can select month, day and year (columns 2, 3 and 4 if we
start counting at 1), like this:

~~~
surveys_df.iloc[0:3, 1:4]
~~~
{: .python}

~~~
   month  day  year
0      7   16  1977
1      7   16  1977
2      7   16  1977
~~~
{: .output}


Notice that we asked for a slice from 0:3. This yielded 3 rows of data. When you
ask for 0:3, you are actually telling python to start at index 0 and select rows
0, 1, 2 **up to but not including 3**.

Let's next explore some other ways to index and select subsets of data:

~~~
# select all columns for rows of index values 0 and 10
surveys_df.loc[[0, 10], :]
# what does this do?
surveys_df.loc[0, ['species_id', 'plot_id', 'weight']]

# What happens when you type the code below?
surveys_df.loc[[0, 10, 35549], :]
~~~
{: .python}


NOTE: Labels must be found in the DataFrame or you will get a `KeyError`. The
start bound and the stop bound are **included**.  When using `loc`, integers
*can* also be used, but they refer to the index label and not the position. Thus
when you use `loc`, and select 1:4, you will get a different result than using
`iloc` to select rows 1:4.

We can also select a specific data value according to the specific row and
column location within the data frame using the `iloc` function:
`dat.iloc[row,column]`.


~~~
surveys_df.iloc[2,6]
~~~
{: .python}


~~~
'F'
~~~
{: .output}


Remember that Python indexing begins at 0. So, the index location [2, 6] selects
the element that is 3 rows down and 7 columns over in the DataFrame.-->

<!--## Challenge Activities

1. What happens when you type:

	- surveys_df[0:3]
	- surveys_df[:5]
	- surveys_df[-1:]

2. What happens when you call:
    - `dat.iloc[0:4, 1:4]`
    - `dat.loc[0:4, 1:4]`
    - How are the two commands different?
-->
<!--## Subsetting Data Using Criteria

We can also select a subset of our data using criteria. For example, we can
select all rows that have a year value of 2002.

~~~
surveys_df[surveys_df.year == 2002]
~~~
{: .python}

~~~
record_id  month  day  year  plot_id species_id  sex  hindfoot_length  weight
33320      33321      1   12  2002        1         DM    M     38      44 
33321      33322      1   12  2002        1         DO    M     37      58
33322      33323      1   12  2002        1         PB    M     28      45
33323      33324      1   12  2002        1         AB  NaN    NaN     NaN
33324      33325      1   12  2002        1         DO    M     35      29
...
35544      35545     12   31  2002       15         AH  NaN    NaN     NaN
35545      35546     12   31  2002       15         AH  NaN    NaN     NaN
35546      35547     12   31  2002       10         RM    F     15      14
35547      35548     12   31  2002        7         DO    M     36      51
35548      35549     12   31  2002        5        NaN  NaN    NaN     NaN

[2229 rows x 9 columns]
~~~
{: .output}


Or we can select all rows that do not contain the year 2002.

~~~
surveys_df[surveys_df.year != 2002]
~~~
{: .python}


We can define sets of criteria too:

~~~
surveys_df[(surveys_df.year >= 1980) & (surveys_df.year <= 1985)]
~~~
{: .python}



## Challenge Activities

1. Select a subset of rows in the `surveys_df` DataFrame that contain data from
   the year 1999 and that contain weight values less than or equal to 8. How
   many columns did you end up with? What did your neighbor get?
2. You can use the `isin` command in python to query a DataFrame based upon a
   list of values as follows:
   `surveys_df[surveys_df['species_id'].isin([listGoesHere])]`. Use the `isin` function
   to find all plots that contain particular species in
   the surveys DataFrame. How many records contain these values?
3. Experiment with other queries. Create a query that finds all rows with a weight value > or equal to 0.
4. The `~` symbol in Python can be used to return the OPPOSITE of the selection that you specify in python. 
It is equivalent to **is not in**. Write a query that selects all rows that are NOT equal to 'M' or 'F' in the surveys
data.


# Using Masks

A mask can be useful to locate where a particular subset of values exist or
don't exist - for example,  NaN, or "Not a Number" values. To understand masks,
we also need to understand `BOOLEAN` objects in python.

Boolean values include `true` or `false`. So for example

~~~
# set x to 5
x = 5
# what does the code below return?
x > 5
# how about this?
x == 5
~~~
{: .python}


When we ask python what the value of `x > 5` is, we get `False`. This is because x
is not greater than 5 it is equal to 5. To create a boolean mask, you first create the
True / False criteria (e.g. values > 5 = True). Python will then assess each
value in the object to determine whether the value meets the criteria (True) or
not (False). Python creates an output object that is the same shape as
the original object, but with a True or False value for each index location.

Let's try this out. Let's identify all locations in the survey data that have
null (missing or NaN) data values. We can use the `isnull` method to do this.
Each cell with a null value will be assigned a value of  `True` in the new
boolean object.


~~~
pd.isnull(surveys_df)
~~~
{: .python}


~~~
      record_id  month    day   year plot_id species_id    sex  hindfoot_length weight
0         False  False  False  False   False      False  False   False      True
1         False  False  False  False   False      False  False   False      True
2         False  False  False  False   False      False  False   False      True
3         False  False  False  False   False      False  False   False      True
4         False  False  False  False   False      False  False   False      True

[35549 rows x 9 columns]
~~~
{: .output}


To select the rows where there are null values,  we can use 
the mask as an index to subset our data as follows:

~~~
#To select just the rows with NaN values, we can use the .any() method
surveys_df[pd.isnull(surveys_df).any(axis=1)]
~~~
{: .python}


Note that there are many null or NaN values in the `weight` column of our DataFrame.
We will explore different ways of dealing with these in Lesson 03.

We can apply `isnull()` to a particular column too. What does the code below do?

~~~
# what does this do?
empty_weights = surveys_df[pd.isnull(surveys_df).any(axis=1)]['weight']
~~~
{: .python}


Let's take a minute to look at the statement above. We are using the Boolean
object as an index. We are asking python to select rows that have a `NaN` value
for weight.


# Challenges

1. Create a new DataFrame that only contains observations with sex values that
   are **not** female or male. Assign each sex value in the new DataFrame to a
   new value of 'x'. Determine the number of null values in the subset.
2. Create a new DataFrame that contains only observations that are of sex male
   or female and where weight values are greater than 0. Create a stacked bar
   plot of average weight by plot with male vs female values stacked for each
   plot.
-->