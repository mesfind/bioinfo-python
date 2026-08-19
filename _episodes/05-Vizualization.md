
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
