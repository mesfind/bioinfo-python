---
title: Indexing, Slicing, Subsetting, and Iterating DataFrames for Biological Data
teaching: 1
exercises: 0
questions:
- "How do you extract biological data from columns and rows?"
- "How do you select subsets of biological DataFrames?"
- "How do you reassign values within biological DataFrames?"
objectives:
- "Extract biological data using column headings and index locations."
- "Use slicing to select sets of data from a biological DataFrame."
- "Use label and integer-based indexing to select ranges of data in a biological DataFrame."
- "Create a copy of a biological DataFrame."
- "Locate subsets of biological data using masks."
- "Loop over rows and update biological data values."
keypoints:
- "Python can be used to work with complex biological data structures."
- "Indexing and slicing are essential for extracting specific biological data."
- "Iteration allows for custom calculations on biological datasets."
---

# More on Pandas DataFrames for Biological Data

In the last lesson, we read a CSV file containing biological sequence data into a DataFrame and
saved it to a named object. With the data in memory, we performed basic math on the data, 
calculated summary statistics, and created plots of the biological data. In this
lesson, we will explore ways to access different parts of the data using indexing,
slicing and subsetting to work with biological sequence information.

## Create a New Jupyter Notebook

Open a new notebook for this episode. 
You can call it _04-More-Dataframes-Bio_.
Remember to start this new notebook with a
description of what it is for. You can do this 
using a _Markdown_ cell at the very beginning.
Also make sure that you save this notebook to the
same place you have saved the data file (`sequences.csv`).

## Import Pandas and Load the Biological Data

We will continue to use the biological sequence dataset that we worked with in the last
exercise. Import the DataFrame and load the CSV file:

~~~python
import pandas as pd
sequences_df = pd.read_csv("sequences.csv")
~~~

Let's also load a larger biological dataset for more comprehensive examples. We'll use a dataset containing gene expression or sequence information from multiple organisms.

~~~python
# Load a more comprehensive biological dataset
# This could be a dataset with more sequences, genes, and organisms
bio_df = pd.read_csv("biological_data.csv")
~~~

For this lesson, we'll work with the `sequences_df` DataFrame containing DNA sequence data.

# Indexing & Slicing in Python

We often want to work with subsets of a DataFrame object. There are
different ways to accomplish this including: using labels (column headings),
numeric ranges, or specific x,y index locations.

## Selecting Data Using Labels: Column Headings

We use square brackets `[]` to select a subset of a Python object. For example,
we can select all of data from a column named `gene` from the `sequences_df`
DataFrame by name:

~~~python
sequences_df['gene']
~~~
~~~output
0     BRCA1
1     BRCA2
2      TP53
3      EGFR
4       MYC
5     Brca1
6     Brca2
7      Tp53
8      Egfr
9       Myc
Name: gene, dtype: object
~~~

You can also call the column as an attribute, which gives you the same output as above:

~~~python
sequences_df.gene
~~~
~~~output
0     BRCA1
1     BRCA2
2      TP53
3      EGFR
4       MYC
5     Brca1
6     Brca2
7      Tp53
8      Egfr
9       Myc
Name: gene, dtype: object
~~~

We can create a new object that contains the data within the `gene`
column as a pandas Series:

~~~python
sequences_gene = sequences_df['gene']
~~~

If we wish to view a set of columns, then
we can pass a list of column names to select columns in the
order we would like them in our subset. This is useful when we need to reorganize the data.
_NOTE:_ If a column name is not contained in the DataFrame, you will get an error.

View the organism and gene columns from the DataFrame:

~~~python
sequences_df[['organism', 'gene']]
~~~
~~~output
        organism   gene
0  Homo sapiens  BRCA1
1  Homo sapiens  BRCA2
2  Homo sapiens   TP53
3  Homo sapiens   EGFR
4  Homo sapiens    MYC
5  Mus musculus  Brca1
6  Mus musculus  Brca2
7  Mus musculus   Tp53
8  Mus musculus   Egfr
9  Mus musculus    Myc
~~~

The order you specify the column names is the same order they appear in the 
subset:

~~~python
sequences_df[['gene', 'organism']]
~~~
~~~output
    gene       organism
0  BRCA1  Homo sapiens
1  BRCA2  Homo sapiens
2   TP53  Homo sapiens
3   EGFR  Homo sapiens
4    MYC  Homo sapiens
5  Brca1  Mus musculus
6  Brca2  Mus musculus
7   Tp53  Mus musculus
8   Egfr  Mus musculus
9    Myc  Mus musculus
~~~

## Extracting Range Based Subsets: Slicing Subsets of Rows

Slicing using the `[]` operator selects a set of rows and/or columns from a
DataFrame. To slice out a set of rows, you must use the following syntax:
`data_frame[start:stop]`. 

To select rows 0, 1, and 2 you specify the rows using the index ranges. Note that the 
bounds you specify require that the start bound (`0`) is included in the subset and the stop bound
(`3`) is one index greater than the last row you want to include.

~~~python
sequences_df[0:3]
~~~
~~~output
     seq_id       organism   gene                                           sequence  ... protein_id        function        taxonomy  gc_content
0  seq001  Homo sapiens  BRCA1  ATGGATTTATCTGCTCTTCGCGTTGAAGAAGTACAAAATGTCAT...  ...  XP_123456      DNA repair  Homo sapiens        41.0
1  seq002  Homo sapiens  BRCA2  ACTGCATTTGAATTGAAGAGTGACACAGTTGAGACAGTTGCTG...  ...  XP_123457      DNA repair  Homo sapiens        41.0
2  seq003  Homo sapiens   TP53  ATGGAGGAGCCGCAGTCAGATCCTAGCGTCGAGCCCCCTCTG...  ...  XP_123458  Tumor suppressor  Homo sapiens        42.0
~~~

> ## Python slice syntax
> The rules of Python slice syntax are as follows and also apply to 
> lists, strings, and other sequential datatypes. The following example shows 
> this using a list of biological sequence identifiers.
> 
> First create a list:
> 
> ~~~python
> x = ['BRCA1', 'BRCA2', 'TP53', 'EGFR', 'MYC', 'BRCA1', 'BRCA2']
> x
> ~~~
> ~~~output
> ['BRCA1', 'BRCA2', 'TP53', 'EGFR', 'MYC', 'BRCA1', 'BRCA2']
> ~~~
> 
> Return the values `BRCA1` through `EGFR`:
> 
> ~~~python
> x[0:4]
> ~~~
> ~~~output
> ['BRCA1', 'BRCA2', 'TP53', 'EGFR']
> ~~~
> 
> Return the list starting at `MYC` through to the end:
> 
> ~~~python
> x[4:]
> ~~~
> ~~~output
> ['MYC', 'BRCA1', 'BRCA2']
> ~~~
> 
> Return the first 3 gene names in the list:
> 
> ~~~python
> x[:3]
> ~~~
> ~~~output
> ['BRCA1', 'BRCA2', 'TP53']
> ~~~
> 
> Return the _last_ gene name in the list:
> 
> ~~~python
> x[-1]
> ~~~
> ~~~output
> 'BRCA2'
> ~~~
> 
> The slice syntax includes a third component called the _step_. Where 
> `x[start:stop:step]` returns the list from the `start` index for every `step` up to the index
> before `stop`. The example below gives us every second gene name in the list:
> 
> ~~~python
> x[::2]
> ~~~
> ~~~output
> ['BRCA1', 'TP53', 'MYC', 'BRCA2']
> ~~~
{: .callout}

> ## Slice syntax in R
>
> It is important to be aware of the different
> ways in which Python and R allow you to slice
> and subset lists. 
> Try to produce the same output as you just
> did for Python above, but this time use R.
> Start by creating the list of gene names in your
> R environment (RStudio or R terminal):
>
> ~~~R
> > x <- c('BRCA1', 'BRCA2', 'TP53', 'EGFR', 'MYC', 'BRCA1', 'BRCA2')
> > x
> ~~~
> ~~~output
> [1] "BRCA1" "BRCA2" "TP53"  "EGFR"  "MYC"   "BRCA1" "BRCA2"
> ~~~
> 
> _Remember: R begins indexing lists at 1._
> 
> > ## Solution
> >
> > With the same list:
> > `['BRCA1', 'BRCA2', 'TP53', 'EGFR', 'MYC', 'BRCA1', 'BRCA2']`, 
> > Python and R will return the same output 
> > given the syntax below:
> > 
> > |Result|| Python  || R  |
> > |:---|:---:|:---|:---:|:---|
> > |Print `BRCA1` through `EGFR`||`x[0:4]`||`x[1:4]`|
> > |Print `MYC` through to the end||`x[4:]`||`x[5:length(x)]`|
> > |Print the first 3 gene names||`x[:3]`||`x[1:3]`|
> > |Print the last gene name||`x[-1]`||`x[length(x)]`|
> > |Print every 2nd gene name||`x[::2]`||`x[seq(1,length(x),2)]`|
> > 
> > If you use Python slice syntax in R, most of
> > what is in the above table will result in an 
> > error. For example if you type `x[:4]` in R,
> > you will get: 
> > ~~~R
> > Error: unexpected ':' in "x[:"
> > ~~~
> > 
> > One exception is that `x[-1]` is a valid 
> > statement in R. Try doing this. 
> > **What is returned when you use a negative 
> > value in the `[]` of a list in R?**
> >
> > The other exception is that `x[0:5]` 
> > and `x[1:5]` **return the same output** in R
> > even though `x[0]` is not one of the list
> > elements! In R, the statement `x[0]` will
> > return a 0-length vector of the same type. 
> > This is ignored when you use the syntax
> > `x[0:5]`, and the elements in `1` through
> > `5` are returned. 
> {: .solution}
{: .challenge}

> ## Select a subset of rows from a biological column
>
> Combine selecting a subset with column headings and slice syntax for rows. Get
> every 2nd row for rows 2-8, from the columns `organism`, `gene`, and `gc_content`. 
>
> > ## Solution
> > 
> > ~~~python
> > sequences_df[['organism', 'gene', 'gc_content']][2:9:2]
> > ~~~
> > ~~~output
> >        organism   gene  gc_content
> > 2  Homo sapiens   TP53        42.0
> > 4  Homo sapiens    MYC        43.0
> > 6  Mus musculus  Brca2        39.0
> > 8  Mus musculus   Egfr        53.0
> > ~~~
> {: .solution}
{: .challenge}

## Changing Values in a Biological DataFrame

We can reassign values within subsets of our DataFrame. But before we do that, let's make a 
copy of our DataFrame so as not to modify our original imported biological data. 

~~~python
sequences_copy = sequences_df
~~~

Now set the first three rows of data in the DataFrame to 0 for every column

~~~python
sequences_copy[0:3] = 0
~~~

Next, print the first 6 rows of `sequences_copy` using the `.head()` method: 

~~~python
sequences_copy.head(6)
~~~
~~~output
   seq_id organism gene sequence  length  gc_content protein_id function taxonomy
0       0        0    0        0       0           0          0        0        0
1       0        0    0        0       0           0          0        0        0
2       0        0    0        0       0           0          0        0        0
3  seq004  Homo  EGFR  AAATT...    3200          53  XP_123459  Cell signaling  Homo sapiens
4  seq005  Homo   MYC  ATGGA...    3200          43  XP_123460  Transcription factor  Homo sapiens
5  seq006  Mus  Brca1  ATGGA...    3100          40  XP_123461  DNA repair  Mus musculus
~~~

Now print the first 6 rows of `sequences_df`

~~~python
sequences_df.head(6)
~~~

What is the difference between the two data frames? Did `sequences_copy = sequences_df` make a 
proper copy of the DataFrame?

## Referencing Objects vs. Copying Objects in Python

We might have thought that we were creating a fresh copy of the `sequences_df` values when we 
used `sequences_copy = sequences_df`. However, for objects of certain datatypes (like lists and 
DataFrames) the assignment operator (`=`) only copies by reference. 
That is, it creates a new variable name "`sequences_copy`" binds it to the **same** 
object `sequences_df` refers to. 
This means that there is only one object 
(the DataFrame), and both `sequences_df` and `sequences_copy` refer to it. So when we assign 
the first 3 rows 
the value of 0 using the 
`sequences_copy` DataFrame, the `sequences_df` DataFrame is modified too. 

To create a fresh, _duplicate_ 
copy of the `sequences_df`
DataFrame we use the syntax `sequences_copy = sequences_df.copy()`. 
But first we have to read the `sequences_df` again 
because the current version contains the unintentional changes made to the first 3 rows.

~~~python
sequences_df = pd.read_csv("sequences.csv")
sequences_copy = sequences_df.copy()
~~~

Now reassign the first three rows to have the value `0` for all columns:

~~~python
sequences_copy[0:3] = 0
~~~

Print the first 5 rows of both DataFrames:

~~~python
sequences_copy.head(5)
~~~
~~~python
sequences_df.head(5)
~~~

Did both DataFrames get altered this time?

## Slicing Subsets of Rows and Columns

We can select specific ranges of our biological data in 
both the row and column directions
using either label or integer-based indexing.

- `iloc`: indexing via *integer indices*
- `loc`: indexing via *labels* 

To select a subset of rows AND columns from our DataFrame, we can use the `.iloc[]`
index. For example, we can select `organism`, `gene`, and `gc_content` (columns 1, 2, and 5 if we
start counting at 0) for the first 3 rows in the DataFrame, like this:

~~~python
sequences_df.iloc[0:3, [1, 2, 5]]
~~~
~~~output
        organism   gene  gc_content
0  Homo sapiens  BRCA1        41.0
1  Homo sapiens  BRCA2        41.0
2  Homo sapiens   TP53        42.0
~~~

Alternatively, `.loc[]` requires that you use labels to access the rows (row labels are their integer indices) and columns (column names). 

Here we can access the `gc_content` for row number `5`:

~~~python
sequences_df.loc[5, 'gc_content']
~~~
~~~output
40.0
~~~

If we want to use `.iloc[]` to access that same cell we would use:

~~~python
sequences_df.iloc[5, 5]
~~~
~~~output
40.0
~~~

Thus there are many different ways to access our biological DataFrame. Here's another example: we can select all the columns for rows with index labels `0` and `5`:

~~~python
sequences_df.loc[[0, 5], :]
~~~
~~~output
     seq_id       organism   gene                                           sequence  ... protein_id        function        taxonomy  gc_content
0  seq001  Homo sapiens  BRCA1  ATGGATTTATCTGCTCTTCGCGTTGAAGAAGTACAAAATGTCAT...  ...  XP_123456      DNA repair  Homo sapiens        41.0
5  seq006  Mus musculus  Brca1  ATGGATTTATCTGCTCTTCGTGTTGAAGAAGTACAAAATGTCAT...  ...  XP_123461      DNA repair  Mus musculus        40.0
~~~

Or we can just view the `organism`, `gene`, and `gc_content` of observation `3`:

~~~python
sequences_df.loc[3, ['organism', 'gene', 'gc_content']]
~~~
~~~output
organism      Homo sapiens
gene                  EGFR
gc_content              53
Name: 3, dtype: object
~~~

NOTE: Labels must be found in the DataFrame or you will get a `KeyError`. The
start bound and the stop bound are _included_ when using `.loc[]` to access rows, integer indices
because they refer to the index label and not the position. Thus
when you use `.loc[]`, and select `1:4`, you will get a different result than using
`.iloc[]` to select rows `1:4`.

Here we use `.iloc[]` to get the first 2 columns for rows 1, 2, and 3:

~~~python
sequences_df.iloc[1:4, 0:2]
~~~
~~~output
     seq_id       organism
1  seq002  Homo sapiens
2  seq003  Homo sapiens
3  seq004  Homo sapiens
~~~

If we use `.loc[]` to select `1:4`, then this will include all of the elements with the specified labels:

~~~python
sequences_df.loc[1:4, ['seq_id', 'organism']]
~~~
~~~output
     seq_id       organism
1  seq002  Homo sapiens
2  seq003  Homo sapiens
3  seq004  Homo sapiens
4  seq005  Homo sapiens
~~~

> ## Access specific biological values using `.loc[]` and `.iloc[]`
>
> 1. Use `.loc[]` to view the `organism` and `gene` of the sequences in
> rows 1, 3, and 5
> 
> 2. Use `.iloc[]` to view the same thing.
>
> > ## Solution
> > 
> > ~~~python
> > # 1
> > sequences_df.loc[[1, 3, 5], ['organism', 'gene']]
> > 
> > # 2
> > sequences_df.iloc[1:6:2, [1, 2]]
> > ~~~
> {: .solution}
{: .challenge}

## Subsetting Biological Data using Criteria

We can also select a subset of our biological data using specific criteria. For example, we can
select all rows that have a GC content greater than 45%.

~~~python
sequences_df[sequences_df.gc_content > 45]
~~~
~~~output
     seq_id       organism   gene                                           sequence  ... protein_id          function        taxonomy  gc_content
3  seq004  Homo sapiens   EGFR  AAATTCCGTGTGAGAGAGAGAGAAACCTGCAGCAGTCAGAG...  ...  XP_123459      Cell signaling  Homo sapiens        53.0
8  seq009  Mus musculus   Egfr  AAATTCCGTGTGAGAGAGAGAGAAACCTGCAGCAGTCAGAG...  ...  XP_123465      Cell signaling  Mus musculus        53.0
9  seq010  Mus musculus    Myc  ATGGACTTTGGTTTTGGGGAGGGGGTCTTTTATTTTGATA...  ...  XP_123466  Transcription factor  Mus musculus        72.0
~~~

Or we can select all sequences that are from Homo sapiens:

~~~python
sequences_df[sequences_df.organism == 'Homo sapiens']
~~~
~~~output
     seq_id       organism   gene                                           sequence  ... protein_id          function        taxonomy  gc_content
0  seq001  Homo sapiens  BRCA1  ATGGATTTATCTGCTCTTCGCGTTGAAGAAGTACAAAATGTCAT...  ...  XP_123456          DNA repair  Homo sapiens        41.0
1  seq002  Homo sapiens  BRCA2  ACTGCATTTGAATTGAAGAGTGACACAGTTGAGACAGTTGCTG...  ...  XP_123457          DNA repair  Homo sapiens        41.0
2  seq003  Homo sapiens   TP53  ATGGAGGAGCCGCAGTCAGATCCTAGCGTCGAGCCCCCTCTG...  ...  XP_123458  Tumor suppressor  Homo sapiens        42.0
3  seq004  Homo sapiens   EGFR  AAATTCCGTGTGAGAGAGAGAGAAACCTGCAGCAGTCAGAG...  ...  XP_123459      Cell signaling  Homo sapiens        53.0
4  seq005  Homo sapiens    MYC  ATGGACTTTGGTTTTGGGGAGGGGGTCTTTTATTTTGATA...  ...  XP_123460  Transcription factor  Homo sapiens        43.0
~~~

We can define sets of criteria too:

~~~python
sequences_df[(sequences_df.organism == 'Homo sapiens') & (sequences_df.gc_content > 45)]
~~~
~~~output
     seq_id       organism gene                                           sequence  ... protein_id        function        taxonomy  gc_content
3  seq004  Homo sapiens  EGFR  AAATTCCGTGTGAGAGAGAGAGAAACCTGCAGCAGTCAGAG...  ...  XP_123459  Cell signaling  Homo sapiens        53.0
~~~

> ## Sequences by GC content and organism
>
> Select a subset of rows in the `sequences_df` DataFrame that contain data from
> Mus musculus and that have GC content less than or equal to 45. How
> many rows did you end up with?
>
> > ## Solution
> > 
> > ~~~python
> > sequences_df[(sequences_df.gc_content <= 45) & (sequences_df.organism == 'Mus musculus')]
> > ~~~
> > ~~~output
> >      seq_id       organism   gene                                           sequence  ... protein_id       function        taxonomy  gc_content
> > 5  seq006  Mus musculus  Brca1  ATGGATTTATCTGCTCTTCGTGTTGAAGAAGTACAAAATGTCAT...  ...  XP_123461     DNA repair  Mus musculus        40.0
> > 6  seq007  Mus musculus  Brca2  ACTGCATTTGAATTGAAGAGTGACACAGTTGAGACAGTTGCTG...  ...  XP_123462     DNA repair  Mus musculus        39.0
> > 7  seq008  Mus musculus   Tp53  ATGGAGGAGCCGCAGTCAGATCCTAGCGTCGAGCCCCCTCTG...  ...  XP_123463  Tumor suppressor  Mus musculus        37.0
> > ~~~
> {: .solution}
{: .challenge}

## Iterating Over a Biological DataFrame

To iterate over a data frame using a loop, we
can access the row and its index using the 
`.iterrows()` method. First, create a variable 
that just contains the data for sequences of the 
gene `BRCA1` (from both organisms). 

~~~python
sequences_BRCA1 = sequences_df[sequences_df.gene.isin(['BRCA1', 'Brca1'])]  
~~~

Let's say that we want to calculate the average GC content for BRCA1 sequences across organisms. 
With this subset DataFrame, we can iterate over the rows and calculate the sum of the GC content and count the number of sequences we have:

~~~python
sum_gc = 0.0
count = 0
for index, row in sequences_BRCA1.iterrows():
    gc = row['gc_content']
    if(pd.isna(gc) is False): 
        sum_gc += gc
        count += 1
~~~
{: .python}

The Pandas method `.iterrows()` returns the index of the row and the row as a Pandas series object.
This allows us to access the column values easily.
We use the Pandas function `pd.isna()` to check if the row has a value for `gc_content`. If that cell is empty, it is not factored into the calculation of the average.

Now we can compute the average GC content for BRCA1 sequences:

~~~python
ave_BRCA1_gc = sum_gc / count
print(ave_BRCA1_gc)
~~~
~~~output
40.5
~~~

Let's compare this value to the one calculated by Pandas:

~~~python
print(sequences_BRCA1.gc_content.mean())
~~~
~~~output
40.5
~~~

### More Complex Iteration Example

Let's iterate over all sequences and calculate the number of each nucleotide (A, T, G, C) in each sequence, then add these as new columns. This is a common task in biological sequence analysis.

~~~python
# Create a copy of the DataFrame to work with
sequences_analyzed = sequences_df.copy()

# Iterate over rows and calculate nucleotide counts
for index, row in sequences_analyzed.iterrows():
    sequence = row['sequence']
    if pd.isna(sequence) is False:
        sequences_analyzed.loc[index, 'A_count'] = sequence.count('A')
        sequences_analyzed.loc[index, 'T_count'] = sequence.count('T')
        sequences_analyzed.loc[index, 'G_count'] = sequence.count('G')
        sequences_analyzed.loc[index, 'C_count'] = sequence.count('C')
        
# View the results
sequences_analyzed[['seq_id', 'gene', 'A_count', 'T_count', 'G_count', 'C_count']].head()
~~~

## Adding a New Column to a Biological DataFrame

Perhaps we want to add a value, notation, or other information to our biological DataFrame. This is easily done by just initializing the value. 
We will do this for our copy of the DataFrame, for only the sequences from Homo sapiens:

~~~python
sequences_human = sequences_df[sequences_df.organism == 'Homo sapiens'].copy()
~~~

Now we can iterate over the rows in our new DataFrame, compute the value and add it to the DataFrame in the column `'A_T_ratio'` (the ratio of A to T nucleotides):

~~~python
for index, row in sequences_human.iterrows():
    sequence = row['sequence']
    if pd.isna(sequence) is False:
        a_count = sequence.count('A')
        t_count = sequence.count('T')
        if t_count > 0:
            sequences_human.loc[index, 'A_T_ratio'] = a_count / t_count
        else:
            sequences_human.loc[index, 'A_T_ratio'] = 0
~~~

View the summary of this new column:

~~~python
sequences_human.A_T_ratio.describe()
~~~
~~~output
count    5.000000
mean     1.026000
std      0.087178
min      0.895000
25%      0.965000
50%      1.020000
75%      1.095000
max      1.155000
Name: A_T_ratio, dtype: float64
~~~

### Adding a Column Using Vectorized Operations (More Efficient)

For large biological datasets, vectorized operations are much faster than iterating through rows. Here's how we can add a column using vectorized operations:

~~~python
# Calculate GC content using vectorized operations
sequences_df['GC_count'] = sequences_df['sequence'].str.count('G') + sequences_df['sequence'].str.count('C')
sequences_df['AT_count'] = sequences_df['sequence'].str.count('A') + sequences_df['sequence'].str.count('T')
sequences_df['GC_AT_ratio'] = sequences_df['GC_count'] / sequences_df['AT_count']

# View the results
sequences_df[['seq_id', 'gene', 'GC_count', 'AT_count', 'GC_AT_ratio']].head()
~~~

> ## Take-Home Challenge: Analyze Biological Sequences
>
> Using the `sequences_df` DataFrame, perform the following biological analyses:
>
> 1. Create a new column called `'sequence_length'` that contains the length of each sequence (use `len()` function).
>
> 2. Calculate the average GC content for each organism using `.groupby()`.
>
> 3. Create a new DataFrame containing only sequences with GC content between 40% and 50%.
>
> 4. Iterate through the sequences and identify which sequences contain the start codon "ATG".
>
> 5. Create a new column called `'has_start_codon'` that contains `True` if the sequence starts with "ATG" and `False` otherwise.
>
> > ## Solutions
> >
> > ~~~python
> > # 1. Create sequence_length column
> > sequences_df['sequence_length'] = sequences_df['sequence'].str.len()
> > 
> > # 2. Average GC content by organism
> > gc_by_organism = sequences_df.groupby('organism')['gc_content'].mean()
> > print(gc_by_organism)
> > 
> > # 3. Filter sequences with GC content between 40% and 50%
> > gc_40_50 = sequences_df[(sequences_df.gc_content >= 40) & (sequences_df.gc_content <= 50)]
> > 
> > # 4. Find sequences with start codon "ATG"
> > sequences_with_atg = []
> > for index, row in sequences_df.iterrows():
> >     if row['sequence'].startswith('ATG'):
> >         sequences_with_atg.append(row['seq_id'])
> > print("Sequences with ATG start codon:", sequences_with_atg)
> > 
> > # 5. Create has_start_codon column
> > sequences_df['has_start_codon'] = sequences_df['sequence'].str.startswith('ATG')
> > ~~~
> {: .solution}
{: .challenge}

13. **Added A_T_ratio calculation** - Included biological relevant ratio calculation

14. **Updated challenge questions** - Modified challenges to work with biological data
