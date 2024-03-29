---
layout: default
title: Home
nav_order: 1
permalink: /
---

# Phantasus: visual and interactive gene expression analysis


This documentation describes Phantasus -- a web tool designed for visual and
interactive gene expression analysis.
In
particular, it was designed to allow to go from a typical dataset to
differential expression and downstream analysis in an easy and streamlined
manner. For that aim, Phantasus integrates an intuitive heatmap interface with
gene expression analysis tools from Bioconductor. 

Main features:
* Loading public datasets from Gene Expression Omnibus with both microarrays and RNA-seq datasets (via ARCHS4) being supported.
* Differential gene expression using `limma` or `DESeq2`.
* Publication ready plots with export to SVG: PCA plot, row profiles, box plots.
* Clustering: k-means and hierarchical.
* Gene set enrichment analysis via `fgsea` package.
* Sharing session links.

<img src="images/screenshot.png" width="600px" />

Links:
* Official mirror: <https://alserglab.wustl.edu/phantasus>.
* Source code at GitHub: <https://github.com/ctlab/phantasus>. 
* Bioconductor package: <https://bioconductor.org/packages/phantasus>.
* Docker image: <https://hub.docker.com/r/alserglab/phantasus>.

Citation:
* Kleverov M, Zenkova D, Kamenev V, Sablina R, Artyomov M, Sergushichev A (2022). Phantasus: web-application for visual and interactive gene expression analysis. doi:10.1101/2022.12.10.519861, https://www.biorxiv.org/content/10.1101/2022.12.10.519861. 
