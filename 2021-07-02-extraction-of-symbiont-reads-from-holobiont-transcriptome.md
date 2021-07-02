---
layout: post  
title: Testing different methods for extracting symbiont reads from a *M. capitata* holobiont transcriptome  
Author: Erin Chille  
Last Updated: 2021/07/02  
tags: [ Protocol, de novo, RSEM, RNASeq, Trinity, psyTrans]  
---

### Alignment to a concatenated reference transcriptome compared to extraction from a *de novo* transcriptome assembly

**Overview**

Functional annotation tags putative genes in a reference genome or transcriptome with the known functions of homologous genes in other organisms. Homologous sequences are first found using the program BLAST, which searches the reference sequence against a database of reviewed protein sequences. Once homologous sequences are found, genes are tagged with known Gene Ontology or Kegg Pathway terms for the homologous sequences. Annotation allows us to better understand the biological processes that are linked to genes of interest.

**Necessary Programs and Equipment**

- A high performace computer (i.e. the URI bluewaves server; Instructions to obtain server access [here](https://github.com/Putnam-Lab/Lab_Management/blob/master/Bioinformatics_%26_Coding/Bluewaves/Bluewaves_Setup.md)) with the following programs:
   