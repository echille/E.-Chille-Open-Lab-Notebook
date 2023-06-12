---  
layout: post  
title: Validation of 515F and 806R primers for M. capitata V4 microbiome analyses  
date: '2023-05-24'  
categories: Notebook Post  
tags: Bhattacharya Lab, PCR, microbiome  
projects: Bhattacharya Lab, HI22 Mcap Microbiome Biogeography  
---  

The workflow below provides step-by-step instructions for how the Bhattacharya Lab validated the 515F and 8069 (targeted the V4 region of 16S rDNA in bacteria) for microbiome analyses in *Montipora capitata*. These primers, while working for most other taxa, have proven troublesome in *M. capitata*, amplifying what seems to mostly be coral DNA. For the below samples, we performed a DNA extraction protocol that enriches for bacterial DNA, in hopes that this would alleviate the issues observed previously. For the validation, 3 *Montipora capitata* samples, 2 sediment samples, and 2 *Galaxea fascicularis* samples were tested. We find that while the primers do amplify the mitochondrial genome in *M. capitata*, enough 16S DNA was sequenced to perform the QIIME2 workflow for microbiome analysis.

## Sample information

*To fill in*

## Contamination analysis

*To fill in*

## QIIME2 Workflow

### Overview

1. Installation of QIIME2  
2. Import data as a QIIME2 artifact  
3. Denoise and declutter  
4. Taxonomic classification  
5. Alpha and beta diversity analyses

### 1. Installation of QIIME2  

We will install QIIME2 through Miniconda, [as recommended](https://docs.qiime2.org/2023.2/install/). To do so, we must download the YAML file containing the list of conda libraries associated with QIIME2. In order to install QIIME2, you must have Miniconda or Anaconda installed first. For more information on conda, see [https://docs.conda.io/](https://docs.conda.io/).  
```  
wget https://data.qiime2.org/distro/core/qiime2-2023.2-py38-osx-conda.yml #download the YAML file  
conda env create -n qiime2-2023.2 --file qiime2-2023.2-py38-osx-conda.yml #Install the libraries as a conda environment called "qiime2-2023.2"  
rm qiime2-2023.2-py38-osx-conda.yml #delete the YAML file  
```  
Now, anytime we want to use QIIME2, we have to first activate the QIIME2 conda environment. This only has to be done anytime you start a new session in terminal. Before you close the session, deactivate the conda environement  
```  
conda activate qiime2-2023.2  #to activate
conda deactivate #to deactivate  
```  

### 2. Import data as a QIIME2 artifact  

#### Sample data  
We are working with demultiplexed paired-end data with quality information (e.g. FastQ files), so we will import our data in the Casava 1.8 format. To import our sample information, we need two  files:  
1) A **sample manifest** (csv format) providing the sample ID, filepath, and sequencing direction for each file  

|sample-id|absolute-filepath|direction|  
|---|---|---| 
|Microbio1|/path_to/Microbio1_S103_R1_001.fastq.gz|forward|  
|Microbio1|/path_to/Microbio1_S103_R2_001.fastq.gz|reverse|  
|Microbio3|/path_to/Microbio3_S105_R1_001.fastq.gz|forward|  
|Microbio3|/path_to/Microbio3_S105_R2_001.fastq.gz|reverse|  
|Microbio4|/path_to/Microbio4_S106_R1_001.fastq.gz|forward|  
|Microbio4|/path_to/Microbio4_S106_R2_001.fastq.gz|reverse|  
|Microbio5|/path_to/Microbio5_S107_R1_001.fastq.gz|forward|  
|Microbio5|/path_to/Microbio5_S107_R2_001.fastq.gz|reverse|  
|Microbio6|/path_to/Microbio6_S108_R1_001.fastq.gz|forward|  
|Microbio6|/path_to/Microbio6_S108_R2_001.fastq.gz|reverse|  
|Microbio8|/path_to/Microbio8_S109_R1_001.fastq.gz|forward|  
|Microbio8|/path_to/Microbio8_S109_R2_001.fastq.gz|reverse|  
|Microbio9|/path_to/Microbio9_S110_R1_001.fastq.gz|forward|  
|Microbio9|/path_to/Microbio9_S110_R2_001.fastq.gz|reverse| 

2) A **metadata file** (tsv format) providing the sample ID and any relevant sample metadata, like species, extraction date, etc...  

||sample-id|extraction-id|source|collection-date|site-id|site-name|colony-id|microenvironment|rep-number|  
|---|---|---|---|---|---|---|---|---|---| 
|#q2:types|categorical|categorical|categorical|categorical|categorical|categorical|categorical|categorical|categorical|  
||Microbio1|S5|Reef_sediment|20220511|S2|KBay_Reef11_West|C3|Soil|R1|  
||Microbio3|S11|Reef_sediment|20220511|S1|KBay_Reef12_West|C3|Soil|R3|  
||Microbio4|C61|Montipora_capitata_tissue|20220511|S1|KBay_Reef12_West|C1|Middle|R2|  
||Microbio5|C76|Montipora_capitata_tissue|20220511|S2|KBay_Reef11_West|C1|Top|R2|  
||Microbio6|C33|Montipora_capitata_tissue|20220511|S2|KBay_Reef11_West|C3|Bottom|R2|  
||Microbio8|G94|Galaxea_fascicularis_tissue|20221006|T3|Dbtank|G94|Top|R1|  
||Microbio9|G95|Galaxea_fascicularis_tissue|20221006|T3|DBtank|G95|Top|R1|  

##### Code to import the sample manifest and metadata  
```  
MANIFEST="metadata/sample_manifest.csv"
qiime tools import \
    --type 'SampleData[PairedEndSequencesWithQuality]' \
    --input-path $MANIFEST \
    --input-format PairedEndFastqManifestPhred33 \
    --output-path qimme2/sequences.qza
```

### 3. Denoise and declutter  

First, we will remove adapter contamination. We know that our adapters content is about 0.1-0.3% because of the QC we did on the raw reads when assessing the amount of Mcap contamination. So, before we do anything else, we need to remove adapter contaminatio using the following code:  
```  
qiime cutadapt trim-paired --verbose \
--i-demultiplexed-sequences qimme2/sequences.qza \
--p-anywhere-f CTGTCTCTTATACACATCT \
--p-anywhere-r AGATGTGTATAAGAGACAG \
--o-trimmed-sequences qimme2/trimmed_sequences.qza
```  

Next, we will denoise our data. In this part, we will trim off our primers. Our primers are 52 and 54 bp long, so we will truncate that part of the sequence. Because our sequences are 150 bp long and they don't ever fall into a lower-quality threshold, we will use 150 bp as our other truncating length. What *denoiseing* does is dereplicate our sequences to reduce repetition and file size/memory requirements in downstream steps.  
```  
qiime dada2 denoise-paired --verbose \  
  --i-demultiplexed-seqs qimme2/sequences.qza \
  --p-trunc-len-r 150 --p-trunc-len-f 150 \
  --p-trim-left-r 54 --p-trim-left-f 52 \
  --o-table  qimme2/table.qza \
  --o-representative-sequences qimme2/rep-seqs.qza \
  --o-denoising-stats qimme2/denoising-stats.qza \
  --p-n-threads 20
```

The last step of decluttering our data is clustering our sequences into ASVs, or a single representative sequence for sequences with 97% similarity to each other. These ASVs will be stored as a *FeatureTable* along with the total count of their abundances in each sample.  
```
METADATA="metadata/sample_metadata.txt"

```

**TO BE CONTINUED...**
