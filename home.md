---
layout: default
title: Home
nav_order: 1
permalink: /
---

# Phantasus: visual and interactive gene expression analysis

This documentation describes Phantasus -- a web-application 
for visual and interactive gene expression analysis.
Phantasus is based on 
[Morpheus](https://software.broadinstitute.org/morpheus/) -- a web-based software
for heatmap visualisation and analysis, which was integrated with an R
environment via [OpenCPU API](https://www.opencpu.org/). 


The main object in Phantasus is a gene expression matrix.
It can either be uploaded from a local text or Excel file 
or loaded from Gene Expression Omnibus (GEO) database by the series identifier
(both microarray and RNA-seq datasets are supported).
Aside from basic visualization and filtering methods as implemented in Morpheus,
R-based methods such as k-means clustering, principal component analysis, 
differential expression analysis with limma package are supported.  
