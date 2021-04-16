---
title: Introduction to Biopython 
teaching: 1
exercises: 0
questions:
- "What does Biopython do?"
- "How does Biopython handle sequences?"
- "How can I access sequences and data from Genbank?"
objectives:
- "Learn about the `Seq` and `SeqRecord` objects."
- "Read in sequences from FASTA files."
- "Download a sequence record directly from Genbank using the NCBI E-utilities."
keypoints:
- "Biopython is a very useful toolbox for working with sequence data."
---


# Biopython Background

Biopython is a freely available package for working with molecular biological data.
In this lesson, we will just cover some basics of working with Biopython. 
The developers of this package have written a comprehensive [tutorial and cookbook](http://biopython.org/DIST/docs/tutorial/Tutorial.html).

The tutorial we are working with today was written by [Dr. Iddo Friedberg](https://iddo-friedberg.net/) and [Dr. Stuart Brown](https://scholar.google.com/citations?user=ig0QSzAAAAAJ&hl=en)

## What can Biopython do?

The [documentation page for Biopython](http://biopython.org/DIST/docs/tutorial/Tutorial.html#htoc3) provides a list of the many
different tools in the package:

- The ability to parse bioinformatics files into Python utilizable data structures, including support for the following formats:
  - Blast output – both from standalone and WWW Blast
  - Clustalw
  - FASTA
  - GenBank
  - PubMed and Medline
  - ExPASy files, like Enzyme and Prosite
  - SCOP, including ‘dom’ and ‘lin’ files
  - UniGene
  - SwissProt 
- Files in the supported formats can be iterated over record by record or indexed and accessed via a Dictionary interface.
- Code to deal with popular on-line bioinformatics destinations such as:
  - NCBI – Blast, Entrez and PubMed services
  - ExPASy – Swiss-Prot and Prosite entries, as well as Prosite searches 
- Interfaces to common bioinformatics programs such as:
  - Standalone Blast from NCBI
  - Clustalw alignment program
  - EMBOSS command line tools 
- A standard sequence class that deals with sequences, ids on sequences, and sequence features.
- Tools for performing common operations on sequences, such as translation, transcription and weight calculations.
- Code to perform classification of data using k Nearest Neighbors, Naive Bayes or Support Vector Machines.
- Code for dealing with alignments, including a standard way to create and deal with substitution matrices.
- Code making it easy to split up parallelizable tasks into separate processes.
- GUI-based programs to do basic sequence manipulations, translations, BLASTing, etc.
- Extensive documentation and help with using the modules, including this file, on-line wiki documentation, the web site, and the mailing list.
- Integration with BioSQL, a sequence database schema also supported by the BioPerl and BioJava projects.

# Getting Started

## Download Example Files

This lesson will use the example files in the 
[`biopython`](https://github.com/EEOB-BioData/BCB546-Spring2021/tree/master/course-files/biopython) 
folder of the course 
files in the `BCB546-Spring2021` repository. 
Download these files and make sure they are in the same directory
where you are creating your Jupyter notebook.



## Install Biopython and Create a Jupyter Notebook

The easiest way to install the Biopython tools is to use `pip`. From your terminal, you simply need to execute the following:

```
$ pip install biopython
```

Now create a new Jupyter notebook for this lesson. 


# Working with Biopython

## The `Seq` Object

The `Seq` object class is simple and fundamental for a lot of
Biopython work. A Seq object can contain DNA, RNA, or protein.
It contains a string (the sequence) and a defined alphabet for that string.
The alphabets are actually defined objects such as `IUPACAmbiguousDNA` or 
`IUPACProtein`. A Seq object with a DNA alphabet has some different methods than one with an Amino Acid alphabet.

First, import the `Seq` object from Biopython

~~~
from Bio.Seq import Seq
~~~
{: .python}

Now we can create a `Seq` object: 

~~~
my_seq = Seq("AGTACACTGGT")
my_seq
~~~
{: .python}

~~~
Seq('AGTACACTGGT')
~~~
{: .output}

<!-- We can create protein sequence by specifying the alphabet:
~~~
my_prot = Seq("AGTACACTGGT", IUPAC.protein)
my_prot
~~~
{: .python}

~~~
Seq('AGTACACTGGT', IUPACProtein())
~~~
{: .output} -->

The nice thing about the sequence object is 
that it can be treated
just like a Python string object.

~~~
print(my_seq[:3])
~~~
{: .python}
~~~
AGT
~~~
{: .output}

`Seq` objects also have string methods like `.count()`
~~~
my_seq.count('AC')
~~~
{: .python}
~~~
2
~~~
{: .output}

And you can use functions that act on strings like `len()`

~~~
len(my_seq)
~~~
{: .python}
~~~
11
~~~
{: .output}


`Seq` objects also have special methods. For example, you can get the reverse complement of a sequence:

~~~
my_seq = Seq("GATCGATGGGCCTATATAGGATCGAAAATCGC")
print(my_seq.reverse_complement())
~~~
{: .python}
~~~
GCGATTTTCGATCCTATATAGGCCCATCGATC
~~~
{: .output}

Just like strings in Python, the `Seq` object is immutable, 
meaning you cannot change it. If you try to change one of the sites in this
sequence, you will get an error. If you want an editable sequence object, you
will need to create a `MutableSeq` object. 

~~~
from Bio.Seq import MutableSeq
mutable_seq = MutableSeq("GCCATTGTAATGGGCCGCTGAAAGGGTGCCCGA")
~~~
{: .python}

Now you can try changing the nucleotide at index 3 to `'G'`.

## The `SeqRecord` Object

Biopython's `SeqRecord` is a complex object that contains a 
`Seq` object as well as other fields for attributes of that 
sequence (i.e., metadata). These attributes are also called 
"annotation fields":

- `.seq`
  - The sequence itself, typically a Seq object.
- `.id`
  - The primary ID used to identify the sequence – a string. In most cases this is something like an accession number.
- `.name`
  - A “common” name/id for the sequence – a string. In some cases this will be the same as the accession number, but it could also be a clone name. I think of this as being analogous to the LOCUS id in a GenBank record.
- `.description`
  - A human readable description or expressive name for the sequence – a string.
- `.letter_annotations`
  - Holds per-letter-annotations using a (restricted) dictionary of additional information about the letters in the sequence. The keys are the name of the information, and the information is contained in the value as a Python sequence (i.e. a list, tuple or string) with the same length as the sequence itself. This is often used for quality scores (e.g. Section 20.1.6) or secondary structure information (e.g. from Stockholm/PFAM alignment files).
- `.annotations`
  - A dictionary of additional information about the sequence. The keys are the name of the information, and the information is contained in the value. This allows the addition of more “unstructured” information to the sequence.
- `.features`
  - A list of SeqFeature objects with more structured information about the features on a sequence (e.g. position of genes on a genome, or domains on a protein sequence)
- `.dbxrefs`
  - A list of database cross-references as strings. 

You can create a `SeqRecord` by giving the constructor a `Seq` object:
~~~
from Bio.SeqRecord import SeqRecord
simple_seq = Seq("GATC")
simple_seq_r = SeqRecord(simple_seq)
~~~
{: .python}

And you can provide attributes:

~~~
simple_seq_r.id = "AC12345"
simple_seq_r.description = "This sequence is pretend."
print(simple_seq_r)
~~~
{: .python}
~~~
ID: AC12345
Name: <unknown name>
Description: This sequence is pretend.
Number of features: 0
Seq('GATC')
~~~
{: .output}

## Reading Sequences from FASTA files

`SeqIO` enables reading in sequences from FASTA files and storing the data in a `SeqRecord`. Addtionally `SeqIO` provides tools for writing sequence data to a file.

We will read in the example file [`NC_005816.fna`](https://github.com/EEOB-BioData/BCB546-Spring2021/blob/master/course-files/biopython/NC_005816.fna) using `SeqIO`.

~~~
from Bio import SeqIO
record = SeqIO.read("NC_005816.fna", "fasta")
record
~~~
{: .python}
<!-- ~~~
SeqRecord(seq=Seq('TGTAACGAACGGTGCAATAGTGATCCACACCCAACGCCTGAAATCAGATCCAGG...CTG', SingleLetterAlphabet()), id='gi|45478711|ref|NC_005816.1|', name='gi|45478711|ref|NC_005816.1|', description='gi|45478711|ref|NC_005816.1| Yersinia pestis biovar Microtus str. 91001 plasmid pPCP1, complete sequence', dbxrefs=[])
~~~
{: .output}
 -->

> ## Find out more about this sequence
>
> Use string methods and the `SeqRecord` attributes to get the length of the sequence
> and the species name.
>
> > ## Solution
> > 
> > Get the length using `len()`. 
> > ~~~
> > len(record.seq)
> > ~~~
> > {: .python}
> > ~~~
> > 9609
> > ~~~
> > {: .output}
> >
> > The species name is given in the description of this FASTA file
> > ~~~
> > record.description
> > ~~~
> > {: .python}
> > ~~~
> > 'gi|45478711|ref|NC_005816.1| Yersinia pestis biovar Microtus str. 91001 plasmid pPCP1, complete sequence'
> > ~~~
> > {: .output}
> > 
> {: .solution}
{: .challenge}

Using `SeqIO` we can read in several sequences from a file and store 
them in a list of `SeqRecord` objects from a file. The file 
[`example.fasta`](https://github.com/EEOB-BioData/BCB546-Spring2021/blob/master/course-files/biopython/example.fasta) looks like this:

```
>EAS54_6_R1_2_1_413_324
CCCTTCTTGTCTTCAGCGTTTCTCC
>EAS54_6_R1_2_1_540_792
TTGGCAGGCCAAGGCCGATGGATCA
>EAS54_6_R1_2_1_443_348
GTTGCTTCTGGCGTGGGTGGGGGGG
```

With Biopython, we can use the `SeqIO.parse()` function to obtain the three sequences in this file

~~~
handle = open("example.fasta", "r") 
seq_list = list(SeqIO.parse(handle, "fasta"))
handle.close()
print(seq_list[0].seq)
~~~
{: .python}
~~~
CCCTTCTTGTCTTCAGCGTTTCTCC
~~~
{: .output}

In the example above, we open the file and assign it to the variable `handle` 
which acts as a pointer to the file contents. 

## Direct Access to GenBank

BioPython has modules that can directly access databases over the Internet using
the `Entrez` module. This uses the NCBI Efetch service,
which works on many NCBI databases including protein and PubMed
literature citations.
With a few tweaks, this code could be used to download a list of
GenBank ID’s and save them as FASTA or GenBank files.

Before using the online NCBI resources, it is important to be aware of the user 
requirements. If you abuse their system (whether on purpose or on accident), 
they will block your access for some time. 
You can find the requirements in the 
[NCBI E-utilities guide](https://www.ncbi.nlm.nih.gov/books/NBK25497/#chapter2.Usage_Guidelines_and_Requiremen). 

First, you are required to provide NCBI with 
your identity so that you can be contacted 
if there is a problem. This also limits abuse of this system so that 
their servers aren't overwhelmed. 
If you are identified as someone who is excessively using the E-utilities,
NCBI will contact you before you are blocked.

The quote below from the 
[NCBI guide](https://www.ncbi.nlm.nih.gov/books/NBK25497/#chapter2.Usage_Guidelines_and_Requiremen) 
gives you a sense of what constitutes appropriate usage of 
the E-utility servers:

>In order not to overload the E-utility servers, NCBI recommends that users post no more than three URL requests per second and limit large jobs to either weekends or between 9:00 PM and 5:00 AM Eastern time during weekdays.


Enter _your own email address_ in place of `<enter your email address>`:
~~~
from Bio import Entrez
Entrez.email = "<enter your email address>"
~~~
{: .python}

Now we can fetch a Genbank record:
~~~
handle = Entrez.efetch(db="nucleotide", id="DQ137224", rettype="gb", retmode="text")
record = SeqIO.read(handle, "genbank")
print(record)
~~~
{: .python}
~~~
ID: DQ137224.1
Name: DQ137224
Description: Megadyptes antipodes voucher JD64A cytochrome b (cytb) gene, partial cds; mitochondrial
Number of features: 3
/molecule_type=DNA
/topology=linear
/data_file_division=VRT
/date=26-JUL-2016
/accessions=['DQ137224']
/sequence_version=1
/keywords=['']
/source=mitochondrion Megadyptes antipodes (Yellow-eyed penguin)
/organism=Megadyptes antipodes
/taxonomy=['Eukaryota', 'Metazoa', 'Chordata', 'Craniata', 'Vertebrata', 'Euteleostomi', 'Archelosauria', 'Archosauria', 'Dinosauria', 'Saurischia', 'Theropoda', 'Coelurosauria', 'Aves', 'Neognathae', 'Sphenisciformes', 'Spheniscidae', 'Megadyptes']
/references=[Reference(title='Multiple gene evidence for expansion of extant penguins out of Antarctica due to global cooling', ...), Reference(title='Direct Submission', ...)]
Seq('ACACAAATTCTAACTGGCCTCCTACTGGCCGCCCACTACACTGCAGACACAACC...AGC', IUPACAmbiguousDNA())
~~~
{: .output}

## BLAST

BioPython makes it easy to work with NCBI's BLAST. To run 
blast over the internet, we can use `qblast()`. 
For this we must import the `NCBIWWW` module:

~~~
from Bio.Blast import NCBIWWW
~~~
{: .python}

You can call the `help()` function on `NCBIWWW.qblast` to inspect how this 
function works. This will return all of the parameters of `qblast` so that
you can understand how to specify your query correctly.
~~~
help(NCBIWWW.qblast)
~~~
{: .python}


Next we can read in a sequence that is stored in a FASTA file called [`test.fasta`](https://github.com/EEOB-BioData/BCB546-Spring2021/blob/master/course-files/biopython/test.fasta).
~~~
query = SeqIO.read("test.fasta", format="fasta")
~~~
{: .python}

The variable we created called `query` is a sequence stored in a `SeqRecord`
object. 

<!-- ~~~
query.description
~~~
{: .python}
 -->

To run a BLAST search on the sequence from our FASTA file, we simply
have to specify the search program (`blastn`) and the database (`nt`). 
The last argument is the `Seq` object stored in our `query`. 
~~~
result_handle = NCBIWWW.qblast("blastn", "nt", query.seq)
~~~
{: .python}

Note that this might not work for everyone in class. It is possible
for NCBI to throttle non-interactive users.

Once we have the results of our BLAST, we can store them in an XML file.
~~~
blast_file = open("my_blast.xml", "w")
blast_file.write(result_handle.read())
~~~
{: .python}

Once we have stored the results, it's best to close all of our open 
file handles:
~~~
blast_file.close()
result_handle.close()
~~~
{: .python}


We created an XML file containing our BLAST results. This is now easy to 
parse using the `NCBIXML` tools:
~~~
from Bio.Blast import NCBIXML
handle = open("my_blast.xml")
blast_record = NCBIXML.read(handle)
~~~
{: .python}


Now that we have read in the file, we can 
print each of the hits:
~~~
for hit in blast_record.descriptions: 
    print(hit.title)
    print(hit.e)
~~~
{: .python}

~~~
gi|1105484513|ref|XM_002284686.3| PREDICTED: Vitis vinifera cold-regulated 413 plasma membrane protein 2 (LOC100248690), mRNA
0.0
gi|1420088022|gb|MG722853.1| Vitis vinifera cold-regulated 413 inner membrane protein 2 mRNA, complete cds
0.0
gi|123704572|emb|AM483681.1| Vitis vinifera, whole genome shotgun sequence, contig VV78X045699.9, clone ENTAV 115
0.0

...

gi|1027107741|ref|XM_008238505.2| PREDICTED: Prunus mume cold-regulated 413 plasma membrane protein 1-like (LOC103335494), mRNA
1.04564e-118
~~~
{: .output}

We can also view the alignments for each of the BLAST hits:
~~~
for hit in blast_record.alignments:
    for hsp in hit.hsps:
      print(hit.title) 
      print(hsp.expect)
      print(hsp.query[0:75] + '...')
      print(hsp.match[0:75] + '...') 
      print(hsp.sbjct[0:75] + '...')
~~~
{: .python}

~~~
gi|1105484513|ref|XM_002284686.3| PREDICTED: Vitis vinifera cold-regulated 413 plasma membrane protein 2 (LOC100248690), mRNA
0.0
TACTCTACAGTCTCTGACTTTGTAAGCTTCGCGCTTCTTCTCCTTTTTCTCTCTGGGGAAAGATTTTCCCTTTCT...
|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||...
TACTCTACAGTCTCTGACTTTGTAAGCTTCGCGCTTCTTCTCCTTTTTCTCTCTGGGGAAAGATTTTCCCTTTCT...

...

gi|1027107741|ref|XM_008238505.2| PREDICTED: Prunus mume cold-regulated 413 plasma membrane protein 1-like (LOC103335494), mRNA
1.04564e-118
ATTGAAGATGGGGAAAAAGGGTTACTTGGCGATGAGGACTGACACTGATACTACTGATTTGATCAGTTCTGATCT...
|||| ||||||  ||| || | ||||||   |||| ||||||  | ||| | | ||| ||||||   || |||||...
ATTGGAGATGGCAAAACAGAGCTACTTGAAAATGATGACTGAACCAGATGCAAATGAATTGATCCACTCCGATCT...
~~~
{: .output}

Often a BLAST search will return many matches for a single query, as is the 
case for this example. This is why it is best to save them in an XML file.
Using `NCBIXML.parse()` enables us to evaluate each of the BLAST records.
We can specify a threshold so that we can easily inspect the closest 
matches.
~~~
E_VALUE_THRESH = 1e-150
for record in NCBIXML.parse(open("my_blast.xml")):
    for align in record.alignments: 
        for hsp in align.hsps:
            if hsp.expect < E_VALUE_THRESH: 
                print("MATCH: %s " % align.title[:60]) 
                print(hsp.expect)
~~~
{: .python}

~~~
MATCH: gi|1105484513|ref|XM_002284686.3| PREDICTED: Vitis vinifera  
0.0
MATCH: gi|1420088022|gb|MG722853.1| Vitis vinifera cold-regulated 4 
0.0
MATCH: gi|123704572|emb|AM483681.1| Vitis vinifera, whole genome sh 
0.0
MATCH: gi|1217007653|ref|XM_021787586.1| PREDICTED: Hevea brasilien 
9.7761e-151
MATCH: gi|1217007651|ref|XM_021787585.1| PREDICTED: Hevea brasilien 
9.7761e-151
~~~
{: .output}


Our BLAST search has matched our sequence with _Vitis vinifera_. 
Let's check to see if it got it right:
~~~
query.description
~~~
{: .python}


> ## Take-Home Challenge: Biopython
>
> Experiment with features of biopython using the sequence in the variable `query` that we loaded from `test.fasta`.
>
> 1. Print the reverse complement of the sequence to screen.
>
> 2. Write your own function to calculate the GC content of this sequence to screen.
>
> > ## Solutions
> >
> > The solutions will be posted in 4 days. Feel free to use the `#scripting_help` channel in Slack to discuss these exercises. 
> {: .solution}
{: .challenge}

<!-- 
~~~

~~~
{: .python}
 -->

