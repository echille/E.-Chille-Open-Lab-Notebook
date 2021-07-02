---
layout: post  
title: Testing different methods for extracting symbiont reads from a *M. capitata* holobiont transcriptome  
Author: Erin Chille  
Last Updated: 2021/07/02  
tags: [ Protocol, de novo, RSEM, RNASeq, Trinity, psyTrans, BUSCO]  
---

### Alignment to a concatenated reference transcriptome compared to extraction from a *de novo* transcriptome assembly

#### Overview

**Goal:** To extract symbiont reads from a *M. capitata* holobiont transcriptome for differential gene expression analysis and subsequent functional enrichment analysis.

**Methods:** Here, I outline three pipelines that I used to extract symbiont reads from a *M. capitata* holobiont transcriptome, **1)** alignment to a concatenated reference genome, **2)** alignment to a concatenated reference transcriptome, and **3)** extraction from a *de novo* transcriptome assembly. RNA was extracted using a Zymo-Duet Extraction Kit; see my notebook post, [Protocol for DNA/RNA Extractions of Montipora Coral Larvae Using Zymo-Duet Extraction Kit](https://echille.github.io/E.-Chille-Open-Lab-Notebook/Protocol-for-DNA-RNA-Extractions-of-Montipora-Coral-Larvae-Using-Zymo-Duet-Extraction-Kit/), for the extraction protocol. The reads were then sent to Genewiz for library preparation (TruSeq Stranded mRNA Sample Preparation Protocol) and sequencing on a HiSeq instrument targeting 15 million reads per sample. Sequences were then quality assessed and cleaned using the pipeline outlined in [M. capitata RNAseq QC, Alignment, Assembly Bioinformatic Pipeline](https://echille.github.io/E.-Chille-Open-Lab-Notebook/Mcap-RNAseq-QC-Align-Assemble-pipeline/).

**Results:** While I found that symbiont read coverage from the *de novo* assembly extraction exceded those from alignment to a concatenated reference transcriptome and genome, ultimately, coverage was too low for robust downstream analysis. However, coverage may be improved using other methods such as [GC-content peak inference](https://link.springer.com/article/10.1186/s12864-017-4090-y), as used by Frazier et al. 
(2017) or through other *de novo* assembly programs. Additionally, steps to enrich the symbiont reads during the RNA extraction process that were not taken here (e.g., through pelleting) could additionally lead to higher symbiont coverage. If working with coral tissues, this may be an extra measure you will want to take to ensure adequate symbiont RNA concentrations for differential expression analysis.

**Necessary Programs and Equipment**

- A high performace computer (i.e. the URI bluewaves server; Instructions to obtain server access [here](https://github.com/Putnam-Lab/Lab_Management/blob/master/Bioinformatics_%26_Coding/Bluewaves/Bluewaves_Setup.md)) with the following programs:

***Method 1:** Extraction from concatenated reference genome:*  
- [HISAT2](https://ccb.jhu.edu/software/hisat2/index.shtml) v2.1.0  
- [SAMtools](http://www.htslib.org/doc/samtools.html) v1.9   
- [StringTie](https://ccb.jhu.edu/software/stringtie) v2.1.3
- [GffCompare](https://ccb.jhu.edu/software/stringtie/gffcompare.shtml) v0.12.1
    
***Method 2:** Extraction from concatenated reference transcriptome:*
    
    
***Method 3:** Extraction from a* de novo *transcriptome:*
   
   
#### References  
Frazier, M., Helmkampf, M., Bellinger, M. R., Geib, S. M., & Takabayashi, M. (2017). De novo metatranscriptome assembly and coral gene expression profile of Montipora capitata with growth anomaly. *BMC Genomics, 18*(1). https://doi.org/10.1186/s12864-017-4090-y

:fa-Dolphin:

