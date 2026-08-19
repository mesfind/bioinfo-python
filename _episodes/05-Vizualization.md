---
title: Visualizing Biological Data in Python
teaching: 1
exercises: 0
questions:
- "How do you create appealing biological data plots in Python?"
- "How do you compare distributions of biological data?"
- "How do different plotting libraries work for biological visualization?"
objectives:
- "Examples of `seaborn` for biological data visualization."
- "Make box plots, violin plots, and histograms for biological data in `seaborn`."
- "Some examples of plots using `bokeh` and `ggplot` for biological data."
keypoints:
- "There are many tools for plotting biological data in Python."
- "Visualization is essential for understanding biological patterns."
- "Different plot types are suited for different biological data types."
---

Visualization is meant to convey information about biological data.

> The power of a graph is its ability to enable one to take in the quantitative information, organize it, and see patterns and structure not readily revealed by other means of studying the data.
>
> - Cleveland and McGill, 1984

# Plotting Biological Data with `matplotlib`

Now, we'll start learning how to create visualizations of biological data in Python. We'll start by using a popular python package called matplotlib, and later on use a second package called seaborn that builds on matplotlib. First we'll import matplotlib, along with several other Python packages.

~~~python
import math
import random
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns
~~~

One of the nice features of Jupyter notebooks is that figures can be plotted inline, which means they appear below the code cell that creates them. This is not the default behavior however, and so we'll use the below "Magic" statement to tell the Jupyter notebook to plot the figures inline (instead of opening a separate browser window to show them).

~~~python
%matplotlib inline
~~~

To illustrate what matplotlib can do, we'll need to use a biological dataset. We'll use a dataset containing gene expression, sequence characteristics, and other biological information from multiple organisms.

First, let's create a biological dataset for demonstration purposes:

~~~python
# Create a biological dataset
bio_data = {
    'gene': ['BRCA1', 'BRCA2', 'TP53', 'EGFR', 'MYC', 'KRAS', 'PTEN', 'RB1', 'APC', 'VHL',
             'BRCA1', 'BRCA2', 'TP53', 'EGFR', 'MYC', 'KRAS', 'PTEN', 'RB1', 'APC', 'VHL'],
    'organism': ['Human']*10 + ['Mouse']*10,
    'expression_level': [12.5, 8.3, 15.7, 20.1, 18.4, 5.2, 9.8, 7.6, 11.3, 6.9,
                        14.2, 9.1, 17.3, 22.5, 19.8, 6.4, 11.2, 8.9, 12.7, 7.8],
    'gc_content': [41, 39, 42, 53, 43, 38, 40, 41, 39, 42,
                   45, 43, 46, 56, 48, 42, 44, 45, 43, 46],
    'sequence_length': [3200, 3100, 2900, 3500, 2800, 2100, 2600, 2400, 2900, 2200,
                       3300, 3200, 3000, 3600, 2900, 2200, 2700, 2500, 3000, 2300],
    'protein_length': [1863, 3418, 393, 1210, 439, 189, 403, 928, 2843, 213,
                       1872, 3425, 400, 1217, 446, 192, 410, 935, 2851, 220]
}

df = pd.DataFrame(bio_data)
df.head()
~~~

Now we have a biological dataset containing information about genes from human and mouse organisms, including expression levels, GC content, and sequence lengths.

To look at the first few rows of the dataset we'll use the `.head()` method of the dataframe.

~~~python
df.head()
~~~
~~~output
    gene organism  expression_level  gc_content  sequence_length  protein_length
0  BRCA1    Human              12.5          41             3200            1863
1  BRCA2    Human               8.3          39             3100            3418
2   TP53    Human              15.7          42             2900             393
3   EGFR    Human              20.1          53             3500            1210
4    MYC    Human              18.4          43             2800             439
~~~

Let's get an idea of how gene expression was distributed across all of the genes by calculating some summary statistics.

~~~python
df['expression_level'].describe()
~~~
~~~output
count    20.000000
mean     12.740000
std       5.545679
min       5.200000
25%       8.100000
50%      11.850000
75%      16.300000
max      22.500000
Name: expression_level, dtype: float64
~~~

## Histograms for Biological Data

Histograms plot an discretized distribution of a one-dimensional dataset across all the values it has taken. They visualize how many data points are in each bin, each of which has a pre-defined range.

To create a histogram plot in matplotlib we'll use pyplot, which is a collection of command style functions that make matplotlib work like MATLAB and save many lines of repeated code. By convention, pyplot is aliased to plt, which we've already done in the above import cell.

Let's use `plt.hist()` to create a histogram of gene expression levels:

~~~python
plt.hist(df['expression_level']);
plt.title('Distribution of Gene Expression Levels')
plt.xlabel('Expression Level (RPKM)')
plt.ylabel('Number of Genes');
~~~
{: .python}

![png](../fig/bio-plt-hist.png)

This histogram tells us that many of the genes have expression levels between 5-10 RPKM. There is also a second peak around 15-20 RPKM. This type of distribution may indicate different classes of genes (housekeeping vs. tissue-specific).

To make this histogram more interpretable let's add a title and labels for the x and y axes.

~~~python
plt.hist(df['expression_level'], bins=15);
plt.title('Distribution of Gene Expression Levels')
plt.xlabel('Expression Level (RPKM)')
plt.ylabel('Number of Genes');
~~~
{: .python}

![png](../fig/bio-plt-hist2.png)

Now let's look at the distribution of GC content across genes:

~~~python
plt.hist(df['gc_content'], bins=10, color='green', alpha=0.7);
plt.title('Distribution of GC Content Across Genes')
plt.xlabel('GC Content (%)')
plt.ylabel('Number of Genes');
~~~
{: .python}

![png](../fig/bio-plt-hist3.png)

We can see this histogram shows that most genes have GC content between 40-45%, with some outliers in the 50-55% range.

## Bar Plots for Biological Data

Next, it might be interesting to get a sense of how many genes per organism we have data for.

~~~python
gene_counts = df.groupby('organism', as_index=False)['gene'].count()
gene_counts
~~~
~~~output
  organism  gene
0    Human    10
1    Mouse    10
~~~

Let's create a bar plot to visualize this:

~~~python
plt.figure(figsize=(8, 6))
sns.barplot(data=gene_counts, x='organism', y='gene', color='skyblue')
plt.xlabel('Organism')
plt.ylabel('Number of Genes')
plt.title('Number of Genes by Organism')
plt.show()
~~~
{: .python}

![png](../fig/bio-barplot.png)

Let's also look at the average expression level by organism:

~~~python
avg_expression = df.groupby('organism')['expression_level'].mean().reset_index()

plt.figure(figsize=(8, 6))
sns.barplot(data=avg_expression, x='organism', y='expression_level', color='lightcoral')
plt.xlabel('Organism')
plt.ylabel('Average Expression Level (RPKM)')
plt.title('Average Gene Expression by Organism')
plt.show()
~~~
{: .python}

![png](../fig/bio-barplot2.png)

## Continuous Data Analysis

A quick way to analyze the distribution of numerical columns in a biological DataFrame is to calculate summary statistics, create histograms to visualize the distribution, use box plots to identify outliers, and generate normal probability plots to assess normality.

~~~python
from scipy.stats import probplot

# Set use_inf_as_na to True to handle infinite values
pd.set_option('mode.use_inf_as_na', True)

# Analyze numerical columns
for col in df.select_dtypes(np.number).columns:
    plt.figure(figsize=(14, 4))
    print(f"Skewness of {col}:", df[col].skew())
    print(f"Kurtosis of {col}:", df[col].kurtosis())
    plt.subplot(131)
    sns.histplot(df[col], kde=True)
    plt.title(f'Distribution of {col}')
    plt.subplot(132)
    sns.boxplot(data=df, x=col)
    plt.title(f'Boxplot of {col}')
    plt.subplot(133)
    probplot(df[col], dist='norm', rvalue=True, plot=plt)
    plt.title(f'Q-Q Plot of {col}')
    plt.suptitle(col)
    plt.show()
~~~
{: .python}

![png](../fig/bio-histogram-probplot1.png)
![png](../fig/bio-histogram-probplot2.png)

## Categorical Data Plot

A count plot, also known as a bar plot, is a type of plot that displays the count or frequency of each category in a categorical variable. In this case, the 'organism' column is a categorical variable, and the count plot shows the number of occurrences for each organism in the DataFrame.

~~~python
plt.figure(figsize=(10, 6))
sns.countplot(data=df, x='organism')
plt.title('Number of Genes by Organism')
plt.xticks(rotation=0)
plt.show()
plt.close('all')
~~~
{: .python}

![png](../fig/bio-countplot.png)

## HeatMap for Biological Data

A heatmap is a graphical representation of data where the individual values contained in a matrix are represented as colors. It is a powerful tool for visualizing and analyzing complex biological data, especially when dealing with large datasets. The heatmap provides a visual representation of the correlations between numerical columns in the DataFrame, helping to identify relationships and patterns in the biological data.

~~~python
# Filter out only the numerical columns
numerical_df = df.select_dtypes(include=['float64', 'int64'])

plt.figure(figsize=(10, 8))
fig = sns.heatmap(numerical_df.corr(), annot=True, cmap='coolwarm', vmin=-1.0, vmax=1.0)
plt.title('Correlation Matrix of Biological Features')
plt.xticks(rotation=45)
plt.show()
plt.close('all')
~~~
{: .python}

![png](../fig/bio-heatmap.png)

This heatmap shows correlations between different biological features. For example, we might see that GC content and sequence length have some correlation, or that expression level correlates with certain sequence features.

# Plotting with `seaborn` for Biological Data

Python has powerful built-in plotting capabilities and for this exercise, we will focus on using the [`seaborn`](https://seaborn.pydata.org/) package, which facilitates the creation of highly-informative plots of structured biological data. The `seaborn` library is built on `matplotlib` and features very nice color palettes. This library makes manipulating the features of a `matplotlib` plot somewhat easier.

## Getting Started

Create a new Jupyter notebook for this lesson and begin by importing the necessary packages:

~~~python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
~~~

Here, we have given seaborn the alias `sns`.

Now load the biological DataFrame we created earlier:

~~~python
# Use the biological dataset we created
bio_df = df.copy()
~~~

## A Simple Scatterplot for Biological Data

Let's start with a basic scatterplot. We'll plot expression level on the horizontal axis and protein length on the vertical axis. This uses the seaborn function `.lmplot()`. This function can take a Pandas DataFrame directly. It also will fit a regression line, by default. Since we may not want to visualize these data with a regression line, we will use the `fit_reg=False` argument.

~~~python
sns.lmplot(x="expression_level", y="protein_length", data=bio_df, fit_reg=False)
plt.title('Gene Expression vs. Protein Length')
plt.xlabel('Expression Level (RPKM)')
plt.ylabel('Protein Length (amino acids)')
plt.show()
~~~
{: .python}

![png](../fig/bio-seaborn-scatter-1.png)

Out of the box, seaborn will plot with a given set of aesthetics. We may want to change the label font size and make the background gray. For this, we can call the `.set()` method.

~~~python
sns.set(font_scale=1.5)
~~~

This will increase the font size of the labels by 50% in all of our subsequent plots.

~~~python
sns.lmplot(x="expression_level", y="protein_length", data=bio_df, fit_reg=False)
plt.title('Gene Expression vs. Protein Length')
plt.xlabel('Expression Level (RPKM)')
plt.ylabel('Protein Length (amino acids)')
plt.show()
~~~
{: .python}

![png](../fig/bio-seaborn-scatter-2.png)

The plot size is small, by default. For `.lmplot()`, we can set the plot size directly using the `height` and `aspect` arguments.

~~~python
sns.lmplot(x="expression_level", y="protein_length", data=bio_df, fit_reg=False, height=8, aspect=1.5)
plt.title('Gene Expression vs. Protein Length')
plt.show()
~~~
{: .python}

![png](../fig/bio-seaborn-scatter-3.png)

### Changing marker aesthetics

One issue with this plot is that because we have a small dataset, it's fine, but for larger biological datasets we might need to avoid overplotting. We can change the size of the marker using `scatter_kws`.

~~~python
sns.lmplot(x="expression_level", y="protein_length", data=bio_df, fit_reg=False, 
           height=8, aspect=1.5, scatter_kws={"s": 100})
plt.title('Gene Expression vs. Protein Length')
plt.show()
~~~
{: .python}

![png](../fig/bio-seaborn-scatter-4.png)

### Coloring markers by a categorical value

We can also specify that the organism labels indicate categories that determine a point's color:

~~~python
sns.lmplot(x="expression_level", y="protein_length", data=bio_df, 
           fit_reg=False, height=8, aspect=1.5, scatter_kws={'alpha':0.7, "s": 150}, 
           hue='organism', markers='D')
plt.title('Gene Expression vs. Protein Length by Organism')
plt.show()
~~~
{: .python}

![png](../fig/bio-seaborn-scatter-5.png)

### Setting the axis labels

For `.lmplot()`, we can create a figure variable and use a member method of that variable to set the axis labels.

~~~python
my_fig = sns.lmplot(x="expression_level", y="protein_length", data=bio_df, 
                    fit_reg=False, height=8, aspect=1.5, 
                    scatter_kws={'alpha':0.7, "s": 150}, 
                    hue='organism', markers='D')
my_fig.set_axis_labels('Expression Level (RPKM)', 'Protein Length (amino acids)')
plt.title('Gene Expression vs. Protein Length by Organism')
plt.show()
~~~
{: .python}

![png](../fig/bio-seaborn-scatter-6.png)

> ## Scatter plot for a single organism
>
> How would you plot expression level vs. GC content for just a single organism?
> Create a scatter plot for Human genes only and color by gene.
>
> > ## Solution
> > 
> > ~~~python
> > human_data = bio_df[bio_df.organism == 'Human']
> > my_fig = sns.lmplot(x="expression_level", y="gc_content", data=human_data, 
>                     fit_reg=False, height=8, aspect=1.5, 
>                     scatter_kws={'alpha':0.7, "s": 200}, 
>                     hue='gene', markers='8')
> > my_fig.set_axis_labels('Expression Level (RPKM)', 'GC Content (%)')
> > plt.title('Expression vs. GC Content for Human Genes')
> > plt.show()
> > ~~~
> > {: .python}
> > 
> > ![png](../fig/bio-scatter-single-organism.png)
> {: .solution}
{: .challenge}

# Box Plots & Violin Plots for Biological Data

We often like to compare the distributions of values across different categorical biological variables. Box plots and violin plots are often used to do this in a simple way.

In seaborn, both the `.boxplot()` and `.violinplot()` functions return matplotlib Axes objects. Thus, these plot functions do not have arguments for `height` and `aspect` like the scatter plot function above.

In order to change the size of these plots, we must create a matplotlib figure and axes and set the dimensions of the figure.

~~~python
plot_dims = (14, 9)
~~~

~~~python
fig, ax = plt.subplots(figsize=plot_dims)
sns.boxplot(x='gene', y='expression_level', data=bio_df)
ax.set(xlabel='Gene', ylabel='Expression Level (RPKM)')
plt.xticks(rotation=45)
plt.title('Expression Levels by Gene')
plt.show()
~~~
{: .python}

![png](../fig/bio-boxplot-1.png)

Now let's use a violin plot to visualize the distribution of GC content by organism:

~~~python
fig, ax = plt.subplots(figsize=plot_dims)
sns.violinplot(x='organism', y='gc_content', data=bio_df, linewidth=0.5)
ax.set(xlabel='Organism', ylabel='GC Content (%)')
plt.title('GC Content Distribution by Organism')
plt.show()
~~~
{: .python}

![png](../fig/bio-violinplot-1.png)

> ## Violin plot for gene expression
>
> Use seaborn to make a violin plot comparing the relative distributions of expression levels 
> across different genes.
>
> > ## Solution
> > 
> > ~~~python
> > fig, ax = plt.subplots(figsize=(14, 8))
> > sns.violinplot(x='gene', y='expression_level', data=bio_df, palette='Set3')
> > ax.set(xlabel='Gene', ylabel='Expression Level (RPKM)')
> > plt.xticks(rotation=45)
> > plt.title('Distribution of Expression Levels by Gene')
> > plt.show()
> > ~~~
> > {: .python}
> > 
> > ![png](../fig/bio-violinplot-2.png)
> {: .solution}
{: .challenge}

# Histograms for Biological Data

Often, a histogram is a better way to visualize a biological distribution. This is relatively simple using seaborn's `.displot()` function.

~~~python
sns.displot(bio_df['expression_level'], color='blue', bins=15, 
            height=8, aspect=1.5)
plt.title('Distribution of Gene Expression Levels')
plt.xlabel('Expression Level (RPKM)')
plt.ylabel('Number of Genes')
plt.show()
~~~
{: .python}

![png](../fig/bio-distplot-1.png)

By default, the `.displot()` function plots the density as a histogram. Alternatively, you can plot a kernel density estimate by setting the `kind` argument to `kind="kde"`.

~~~python
sns.displot(bio_df['gc_content'], color='green', kind="kde", 
            height=8, aspect=1.5)
plt.title('Distribution of GC Content')
plt.xlabel('GC Content (%)')
plt.show()
~~~
{: .python}

![png](../fig/bio-distplot-2.png)

## Pairplot for Biological Data

A pairplot is a great way to visualize relationships between multiple biological variables at once:

~~~python
sns.pairplot(bio_df, hue='organism', vars=['expression_level', 'gc_content', 'sequence_length', 'protein_length'])
plt.suptitle('Biological Feature Relationships', y=1.02)
plt.show()
~~~
{: .python}

![png](../fig/bio-pairplot.png)

# Plotting with `bokeh` for Biological Data

Another library called `bokeh` can create amazing, interactive graphics using D3.js (javascript). This package is also easy to install with `conda`:

```
$ conda install bokeh
```

Return to your Jupyter notebook and import the necessary bokeh plotting tools:

~~~python
from bokeh.plotting import figure 
from bokeh.io import output_notebook, show
import numpy as np
~~~

We can now execute the `output_notebook()` function that will ensure that our Javascript images are displayed in our html notebook.

~~~python
output_notebook()
~~~
{: .python}

![png](../fig/bokeh1.png)

We can reproduce the histogram of the expression levels:

~~~python
hist, edges = np.histogram(bio_df['expression_level'], density=True, bins=15)
my_fig = figure(title="Expression Level Distribution", background_fill_color="#EBC8EB")
my_fig.quad(top=hist, bottom=0, left=edges[:-1], right=edges[1:], fill_color="#036564", line_color="#033649")
my_fig.xaxis.axis_label = 'Expression Level (RPKM)'
my_fig.yaxis.axis_label = 'Density'
show(my_fig)
~~~
{: .python}

![png](../fig/bio-bokeh-hist.png)

We can also create an interactive scatter plot for biological data:

~~~python
from bokeh.models import HoverTool

# Create scatter plot
p = figure(title="Gene Expression vs. GC Content", width=800, height=600)
p.scatter(bio_df['expression_level'], bio_df['gc_content'], 
          size=12, color='navy', alpha=0.6)

# Add hover tool
hover = HoverTool()
hover.tooltips = [("Gene", "@gene"), ("Organism", "@organism")]
p.add_tools(hover)

p.xaxis.axis_label = 'Expression Level (RPKM)'
p.yaxis.axis_label = 'GC Content (%)'
show(p)
~~~
{: .python}

![png](../fig/bio-bokeh-scatter.png)

We can also save this file as an html file that we can share with others or embed in files on the web.

~~~python
from bokeh.resources import CDN
from bokeh.embed import file_html

html = file_html(p, CDN, "Gene Expression vs GC Content")
with open('gene_expression_bokeh.html', 'w') as out:
    out.write(html)
~~~
{: .python}

If you open the file you created in your web browser, you now have an interactive version of your biological figure. You can then use this to embed in a website.

> ## Take-Home Challenge: More Biological Visualizations in Python
>
> Continue to use `seaborn` and `bokeh` to try out different biological visualizations. 
>
> 1. Produce a histogram of the protein length for all genes in the dataset. Try making the histogram using both `seaborn` and `bokeh`. Consider altering the bin numbers to help visualize the distribution. 
>
> 2. Create a visualization that helps us better understand how gene expression might correlate with GC content and sequence length. Use a scatter plot matrix or pairplot to show these relationships.
>
> 3. Create an interactive visualization using bokeh that allows you to hover over points to see gene names and expression levels.
>
> 4. Compare the distribution of GC content between Human and Mouse genes using overlapping histograms.
>
> > ## Solutions
> >
> > 1. Protein length histogram:
> > ~~~python
> > # Seaborn version
> > sns.displot(bio_df['protein_length'], bins=20, color='purple', height=8, aspect=1.5)
> > plt.title('Distribution of Protein Lengths')
> > plt.xlabel('Protein Length (amino acids)')
> > plt.ylabel('Number of Genes')
> > plt.show()
> > 
> > # Bokeh version
> > hist, edges = np.histogram(bio_df['protein_length'], density=True, bins=20)
> > p = figure(title="Protein Length Distribution", background_fill_color="#EBC8EB")
> > p.quad(top=hist, bottom=0, left=edges[:-1], right=edges[1:], fill_color="#036564", line_color="#033649")
> > p.xaxis.axis_label = 'Protein Length (amino acids)'
> > p.yaxis.axis_label = 'Density'
> > show(p)
> > ~~~
> >
> > 2. Scatter plot matrix:
> > ~~~python
> > sns.pairplot(bio_df, vars=['expression_level', 'gc_content', 'sequence_length', 'protein_length'], hue='organism')
> > plt.show()
> > ~~~
> >
> > 3. Interactive bokeh plot:
> > ~~~python
> > from bokeh.models import HoverTool, ColumnDataSource
> > 
> > source = ColumnDataSource(bio_df)
> > p = figure(title="Gene Expression vs GC Content", width=800, height=600)
> > p.scatter('expression_level', 'gc_content', source=source, size=15, color='navy', alpha=0.6)
> > 
> > hover = HoverTool()
> > hover.tooltips = [("Gene", "@gene"), ("Organism", "@organism"), ("Expression", "@expression_level")]
> > p.add_tools(hover)
> > 
> > p.xaxis.axis_label = 'Expression Level (RPKM)'
> > p.yaxis.axis_label = 'GC Content (%)'
> > show(p)
> > ~~~
> >
> > 4. Overlapping histograms:
> > ~~~python
> > human_gc = bio_df[bio_df.organism == 'Human']['gc_content']
> > mouse_gc = bio_df[bio_df.organism == 'Mouse']['gc_content']
> > 
> > plt.figure(figsize=(10, 6))
> > plt.hist(human_gc, bins=10, alpha=0.5, label='Human', color='blue')
> > plt.hist(mouse_gc, bins=10, alpha=0.5, label='Mouse', color='red')
> > plt.xlabel('GC Content (%)')
> > plt.ylabel('Number of Genes')
> > plt.title('GC Content Distribution: Human vs Mouse')
> > plt.legend()
> > plt.show()
> > ~~~
> {: .solution}
{: .challenge}

13. **Updated documentation** - Changed documentation to reflect biological data focus

14. **Added overlapping histograms** - Included example of comparing distributions between organisms
