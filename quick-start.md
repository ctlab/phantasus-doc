---
layout: default
title: Quick Start
nav_order: 2
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc }

## Introduction

In this vignette we show example usage of Phantasus for analysis of public gene
expression data from GEO database.
It starts from loading data, normalization and filtering outliers, 
to doing differential gene expression analysis and downstream analysis.

To illustrate the usage of Phantasus let us consider public dataset from Gene Expression Omnibus (GEO) database
[GSE53986](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE53986). 
This dataset contains data from experiments, where bone marrow derived macrophages were
treated with three stimuli: LPS, IFNg and combined LPS+INFg.

## Opening Phantasus

The simplest way to try Phantasus application is to go to web-site
<https://alserglab.wustl.edu/phantasus>
where the latest versions are deployed. 
Alternatively, Phantasus can be installed locally (see [Installation](installation.html)).

When Phantasus opens the starting screen should appear:

<img src="images/start_screen.jpg" width="750px" />



## Preparing the dataset for analysis

### Opening the dataset

Let us open the dataset. To do this, select _GEO Datasets_ option
in _Choose a file..._ dropdown menu. There, a text field will appear
where `GSE53986` should be entered. Clicking the _Load_ button
(or pressing _Enter_ on the keyboard) will start the loading.
After a few seconds, the corresponding heatmap should appear. 

<img src="images/dataset_loaded.jpg" width="750px" />

On the heatmap, the rows correspond to genes (or microarray probes).
The rows are annotated with _Gene symbol_ and _Gene ID_ annotaions
(as loaded from GEO database). 
Columns correspond to samples. 
They are annotated with titles, GEO sample accession identifiers and treatment field.
The annotations, such as treatment, are loaded from user-submitted GEO annotations 
(they can be seen, for example, in _Charateristics_
section at <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM1304836>).
We note that not for all of the datasets in GEO such proper annotations are supplied.

### Adjusting expression values

By hovering at heatmap cell, gene expression values can be viewed.
The large values there indicate that the data is not log-scaled,
which is important for most types of gene expression analysis.


<img src="images/huge_value.jpg" width="500px" />

For the proper further analysis it is recommended to normalize the 
matrix. To normalize values go to _Tools/Adjust_ menu and
check _Log 2_ and _Quantile normalize_ adjustments.

<img src="images/adjust_tool.jpg" width="500px" />

The new tab with adjusted values will appear. All operations that modify gene expression
matrix (such as adjustment, subsetting and several others) create a new tab. This allows 
to revert the operation by going back to one of the previous tabs.

### Removing duplicate genes

Since the dataset is obtained with a mircroarray, a single gene can be represented by
several probes. This can be seen, for example, by sorting rows 
by _Gene symbol_ column (one click on column header), entering `Actb` in the search 
field and going to the first match by clicking down-arrow next to the field. 
There are five probes corresponding to Actb gene in the considered microarray.

<img src="images/duplicates.jpg" width="600px" />

To simplify the analysis it is better to have one row per gene 
in the gene expression matrix. 
One of the easiest ways is to chose only one row that has
the maximal median level of expression
across all samples. Such method removes the noise of lowly-expressed probes.
Go to _Tools/Collapse_ and choose _Maximum Median Probe_ as the method and 
_Gene ID_ as the collapse field. 

<img src="images/collapse_tool.jpg" width="500px" />

The result will be shown in a new tab. 


### Filtering lowly-expressed genes

Additionally, lowly-epxressed genes can be filtered explicitly. It helps to reduce noise and increase
power of downstream analysis methods. 

First, we calculate mean expression of each gene using 
_Tools/Create Calculated Annotation_ menu. 
Select `Mean` operation, optionally enter a name for the resulting column,
and click _OK_. The result will appear as an additional column in row annotations.

<img src="images/calculated_annotation_tool.jpg" width="500px" />


<img src="images/calculated_annotation_loaded.jpg" width="600px" />

Now this annotation can be used to filter genes.
Open _Tools/Filter_ menu. Click _Add_ to add a new filter. Choose `mean_expression`
as a _Field_ for filtering. Then press _Switch to top filter_ button
and input the number of genes to keep. A good choice for a typical mammalian dataset
is to keep around 10--12 thousand most expressed genes. Filter is applied automatically,
so after closing the dialog with _Close_ button only the genes passing the filter 
will be displayed.


<img src="images/filter_tool.jpg" width="500px" />

<img src="images/filter_tool_result.jpg" width="500px" />

It is more convenient to extract these genes into a new tab. 
For this, select all genes (click on any gene and press _Ctrl+A_) and 
use _Tools/New Heat Map_ menu (or press _Ctrl+X_).

Now you have the tab with a fully prepared dataset for the further analysis. 
To easily distinguish it from other tabs, you can rename it
by right click on the tab and choosing _Rename_ option. Let us rename it to `GSE53986_norm`.

It is also useful to save the current result to be able to return to it later.
In order to save it use _File/Save Dataset_ menu. Enter an appropriate file name (e.g. `GSE53986_norm`)
and press `OK`. A file in text [GCT format](https://software.broadinstitute.org/cancer/software/gsea/wiki/index.php/Data_formats#GCT:_Gene_Cluster_Text_file_format_.28.2A.gct.29)
will be downloaded.

## Exploring the dataset

### PCA Plot

One of the ways to asses quality of the dataset is to use
principal component analysis (PCA) method. This can be done
using _Tools/Plots/PCA Plot_ menu.

<img src="images/pcaplot_tool_clean.png" width="550px" />

You can customize color, size and labels of points on the chart 
using values from annotation. Here we set color to come from
_treatment_ annotation.


<img src="images/pcaplot_tool_finished.png" width="550px" />

It can be seen that in this dataset the first replicates in each condition 
are outliers.

### K-means clustering

Another useful dataset exploration tool is k-means clustering.
Use _Tools/Clustering/k-means_ to cluster genes into 16 clusters.

<img src="images/kmeans_tool.jpg" width="500px" />

Afterwards, rows can be sorted by _clusters_ column. By using 
menu _View/Fit to window_ one can get a "bird's-eye view" on the dataset.
Here also one can clearly see outlying samples.

<img src="images/kmeans_result.jpg" width="600px" />

### Hierarchical clustering

_Tool/Hierarchical clustering_ menu can be used to cluster 
samples and highlight outliers (and 
concordance of other samples) even further. 

<img src="images/hierarchical_tool.jpg" width="500px" />

### Filtering outliers

Now, when outliers are confirmed and easily viewed with the dendrogram 
from the previous step, you can select the good samples 
and extract them into another heatmap (by clicking _Tools/New Heat Map_ or pressing _Ctrl+X_).

<img src="images/good_samples.jpg" width="600px" />

## Differential gene expression

### Apllying _limma_ tool

Differential gene expression analysis can be carried out
with _Tool/Diffential Expression/limma_ menu. 
Choose _treatment_ as a _Field_, with
_Untreated_ and _LPS_ as classes. Clicking _OK_
will call differential gene expression analysis method 
with [*limma*](https://bioconductor.org/packages/limma) R package.

<img src="images/limma_tool.png" width="500px" />

The rows can be ordered by decreasing *t*-statistic column to see which genes are the most
up-regulated upon LPS treatment. 

<img src="images/limma_results.png" width="600px" />

### Pathway analysis with FGSEA

The results of differential gene expression can be used for pathway enrichment analysis
with _FGSEA_ tool.

Open _Tools/Pathway Analysis/Perform FGSEA_, then select Pathway database, which corresponds specimen used in dataset
(Mus Musculus in this example), ranking column and column with ENTREZID or Gene IDs.

<img src="images/fgsea_tool.png" width="500px" />

Clicking OK will open new tab with pathways table.

<img src="images/fgsea_result.png" width="800px" />

Clicking on table row will provide additional information on pathway: pathway name, genes in pathway, leading edge.
You can save result of analysis in `TSV` format.



