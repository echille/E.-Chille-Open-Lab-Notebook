---
layout: post
title: M. capitata functional annotation pipeline 
Author: Erin Chille 
Last Updated: 2020/10/19 
tags: [ Protocol, annotation, RNASeq, GO, KEGG ]
---

## Overview

**Functional annotation of a *Monitpora capitata* reference genome**

Functional annotation tags putative genes in a reference genome or transcriptome with the known functions of homologous genes in other organisms. Homologous sequences are first found using the program BLAST, which searches the reference sequence against a database of reviewed protein sequences. Once homologous sequences are found, genes are tagged with known Gene Ontology or Kegg Pathway terms for the homologous sequences. Annotation allows us to better understand the biological processes that are linked to genes of interest.

**Necessary Programs and Equipment**

- A high performace computer (i.e. the URI bluewaves server; Instructions to obtain server access [here](https://github.com/Putnam-Lab/Lab_Management/blob/master/Bioinformatics_%26_Coding/Bluewaves/Bluewaves_Setup.md)) with the following programs:
    - [DIAMOND](http://www.diamondsearch.org/) Search Program v2.0.0
    - [InterProScan](https://github.com/ebi-pf-team/interproscan) v.5.46-81.0
        - Requires
            - Java v11.0.2
    - [KofamScan](https://github.com/takaram/kofam_scan) v1.3.0
        - Requires
            - Ruby v2.7.1
            - parallel
            - util-linux v2.34
            - HMMER v3.3.1
- A computer/laptop with  with the following programs:
    - [Blast2GO Basic](https://www.blast2go.com/) GUI v5.2.5
    - R v4.0.2
    - RStudio v1.3.959

---

### Step 1: Find homologous sequences:

#### i) Download/Update nr database

The nr, or non-redundant, database is a comprehensive collection of protein sequences that is compiled by the National Center for Biotechnology Information (NCBI). It contains non-identical sequences from GenBank CDS translations, PDB, Swiss-Prot, PIR, and PRF, and is updated on a daily basis.

**If you are in the Putnam Lab and on bluewaves**  
Go to the *sbatch_executables* subdirectory in the Putnam Lab *shared* folder and run the script, ```make_diamond_nr_db.sh```. This script, created by Erin Chille on August 6, 2020, downloads the most recent nr database in FASTA format from [NCBI](ftp://ftp.ncbi.nlm.nih.gov/blast/db/FASTA/nr.gz) and uses it to make a Diamond-formatted nr database.   

**If you are not in the Putnam Lab**  
Download the nr database from [NCBI](ftp://ftp.ncbi.nlm.nih.gov/blast/db/FASTA/nr.gz). Then, use Diamond's ```makedb``` command to format the database in a Diamond-friendly format. You can also use the command ```dbinfo``` to find version information for the database.
```
wget ftp://ftp.ncbi.nlm.nih.gov/blast/db/FASTA/nr.gz #download nr database in fasta format
diamond makedb --in nr.gz -d nr
diamond dbinfo -d nr.dmnd
```

#### ii) Run DIAMOND Search

With your updated nr database, you can run DIAMOND. DIAMOND, like NCBI's BLAST tool, is a sequence aligner for protein and translated DNA searches. The tool is optimized for large datasets (>1 million sequences), and is 100x-20,000x faster BLAST without losing sensitivity. 

The commands that I used are below, however, the script I used to execute this on bluewaves is available on my project [repository](https://github.com/echille/Montipora_OA_Development_Timeseries/blob/master/Scripts/Mcap_diamond.sh). As input, DIAMOND requires your reference sequences (either protein or CDS nucleotides), and a path to your nr database. Below, I output the results to DIAMOND format and then convert to XML. I highly suggest the DIAMOND export format, as it can easily be converted into any other format that you may need for your analysis. 

*It may take a few days depending on the number of sequences you have the M. cap genome (~63,000 genes) took 4.5 days*


**Blastx: Align translated DNA query sequences against a protein reference database**

*Options:*
- **-d** - Path to nr database  
- **-q** - Path to reference fasta file  
- **-o** - Base output name  
- **-f** - Output format. **100**=DIAMOND output.     
- **-b** - Block size in billions of sequence letters to be processed at a time. Larger block sizes increase the use of memory and temporary disk space, but also improve performance. Set at **20**. 20 is the highest recommended value. CPU is about 6x this number (in GB).  
- **--more-sensitive** - slightly more sensitive than the --sensitive mode.  
- **-e** - Maximum expected value to report an alignment. **1E-05** is the cut-off typically used for sequence alignments.  
- **-k** - Maximum top sequences. Set at **1** because I only wanted the top sequence reported for each gene.  
- **--unal** - Report unaligned queries (yes=**1**, no=0). 

**View: Generate formatted output from DAA files**

*Options:*  
- **-a** - Path to input file  
- **-o** - Base output name  
- **-f** - Output format. **5**=XML output. 

```
#Run sequence alignment against the nr database
diamond blastx -d /data/putnamlab/shared/databases/nr.dmnd -q ../data/ref/Mcap.mRNA.fa -o Mcap.annot.200806 -f 100  -b 20 --more-sensitive -e 0.00001 -k1 --unal=1

#Converting format to XML format for BLAST2GO
diamond view -a Mcap.annot.200806.daa -o Mcap.annot.200806.xml -f 5
```
---

### Step 2: Map Gene ontology terms to genome  
*Can be done concurrently with Steps 1 and 3*

#### i) InterProScan

InterProScan searches the database InterPro database that compiles information about proteins' function multiple other resources. I used it to map Kegg and GO terms to my Mcap reference protein sequences.

The commands that I used are below, however, the script I used to execute this on bluewaves is available on my project [repository](https://github.com/echille/Montipora_OA_Development_Timeseries/blob/master/Scripts/IPS.sh). As input, InterProScan requires reference protein sequences. Below, I output the results to XML format, as it is the most data-rich output file and can be used as input into Blast2GO.

*Note: Many fasta files willl use an asterisk to denote a STOP codon. InterProScan does not accept special characters within the sequences, so I removed them prior to running the program using the code below:*

```
cp Mcap.protein.fa ./Mcap.IPSprotein.fa
sed -i 's/*//g' Mcap.IPSprotein.fa
```

**Interproscan.sh: Executes the InterProScan Program**

*Options:*

- **-version** - displays version number
- **-f** - output format
- **-i** - the input data
- **-b** - the output file base
- **-iprlookup** - enables mapping
- **-goterms** - map GO Terms
- **-pa** - enables Kegg term mapping

```
interproscan.sh -version
interproscan.sh -f XML -i ../data/ref/Mcap.IPSprotein.fa -b ./Mcap.interpro.200824  -iprlookup -goterms -pa 
```

#### ii) Blast2GO

Blast2GO is a powerful annotation tool that takes the output from BLAST, and matches the IDs of the homologous sequences identified to gene ontology terms in the GO database. To map GO terms to our identified sequences, I used the DIAMOND output XML file as input for Blast2GO mapping. I then combined the output of Blast2GO with the XML file generated from InterProScan. Blast2GO mapping took about 16 hours to complete on Mcap's 63,227 Blasted sequences and about 45 minutes for Pacuta's Blasted 4,760 sequences.

First, open Blast2GO. Click the down arrow next to "Start" and select "Load BLAST XML".  
![Blast1](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/BLAST1.png)

Once opened, your screen should look something like this:  
![Blast2](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/BLAST2.png)

Next, click mapping to open a pop-up window where you can specify run parameters. I used all default options. Note that the below photo was taken after running all of these steps so the example sequence appears blue and includes the blast, InterProScan and Annotated Tags. Your sequences should still appear Orange.  
![Blast3](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/BLAST3.png)

Once Blast is finished running, you can see the number of sequences that returned GO terms on the top right corner. The sequences with mapped GO terms generally appear at the top of the table while un-mapped sequences are towards the bottom. Finally, you can annotate your sequences from the GO terms obtained via the mapping step using the Annotation tool.  
![Blast7](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/BLAST7.png)

**Now we can use Blast2GO to merge (add and validate) all GO terms retrieved via InterProScan to the GO mappings retrieved from Blast2GO**

To merge your annotation results, navigate to "File > Load InterproScan Results". Selecting this will open another pop-up window where you can upload your InterProScan XML file to the software. Be sure to specify that the InterProScan results are proteins.    
![Blast4](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/BLAST4.png)

Now that your InterProScan results have been loaded, you can easily merge your Blast2GO results and InterProScan results by navigating to the Purple Interpro button and selecting "Merge InterProScan GOs to Annotation".  
![Blast5](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/BLAST5.png)

The results of the merge will appear in the bottom right window. You can see that InterProscan retrieved many more GO terms than Blast2GO (red versus blue).  
![Blast6](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/BLAST6.png)



#### iii) Uniprot

### Step 3: Map Kegg terms to genome  
*Uses KofamScan. Can be done concurrently with Steps 1 and 2. Currently Troubleshooting*



### Step 4: Compilation and assessment of the output of different methods

Done in RStudio. See the RMarkdown [page](https://github.com/echille/Montipora_OA_Development_Timeseries/blob/master/RNAseq_Analyses/annot/Mcap_annot_compile.Rmd), that these instructions are heavily based on. This script takes the results of my functional annotation (following the steps outlined above with the exception of KofamScan) and combines the results.


Load libraries
```{r, message=FALSE, warning=FALSE}
library(tidyverse)
library(dplyr)
```

#### Add Blast (DIAMOND) results
```{r}
blast <- read_tsv("0-BLAST-GO-KO/1-DIAMOND/Mcap.annot.200806.tab", col_names = FALSE)
colnames(blast) <- c("seqName", "top_hit", "pident", "length", "mismatch", "gapopen", "qstart", "qend", "sstart", "send", "evalue", "bitscore",  "qlen", "slen")
head(blast)
dim(blast)
```

#### Add Unitprot results

Mapping with Uniprot was done multiple times to search their different libraries. Therefore, multiple output files from the Uniprot search will be compiled before adding to the final out file.

First, load in the different files and see how many results were obtained from each. Additionally, make sure that they all contain the same columns and column names.
```{r}
u1 <- read_tsv("0-BLAST-GO-KO/4-Uniprot/Mcap_uniprot1.tab", col_names = TRUE)
u1 <- u1[,c(1,4:12)]
colnames(u1) <- c("top_hit", "uniprotkb_entry", "status", "protein_names", "gene_names", "organism", "length", "gene_ontology", "go_ids", "ko")
head(u1)
dim(u1)

u2a <- read_tsv("0-BLAST-GO-KO/4-Uniprot/uniparc-to-uniprot/Mcap_uniprot2.tab", col_names = TRUE)
u2a <- u2a[,c(1,3:11)]
colnames(u2a) <- c("uniparc_entry", "uniprotkb_entry", "status", "protein_names", "gene_names", "organism", "length", "gene_ontology", "go_ids", "ko")

u2b <- read_tsv("0-BLAST-GO-KO/4-Uniprot/uniparc-to-uniprot/uniparc-to-uniprot.tab", col_names = TRUE)
colnames(u2b) <- c("top_hit", "uniparc_entry")
head(u2b)
u2 <- merge(u2a, u2b, by="uniparc_entry")
u2 <- u2[,c(11,2:10)]
head(u2)
dim(u2)

u3 <- read_tsv("0-BLAST-GO-KO/4-Uniprot/EMBL-CDS-to-uniprot.tab", col_names = TRUE)
u3 <- u3[,c(1,3:11)]
colnames(u3) <- c("top_hit", "uniprotkb_entry", "status", "protein_names", "gene_names", "organism", "length", "gene_ontology", "go_ids", "ko")
head(u3)
dim(u3)

u4 <- read_tsv("0-BLAST-GO-KO/4-Uniprot/EMBL-prot-to-uniprot.tab", col_names = TRUE)
u4 <- u4[,c(1,3:11)]
colnames(u4) <- c("top_hit", "uniprotkb_entry", "status", "protein_names", "gene_names", "organism", "length", "gene_ontology", "go_ids", "ko")
head(u4)
dim(u4)
```

Then, merge the files and calculate how many 1) unique GO terms were retrieved in total, and 2) How many genes were annotated.
```{r}
Uniprot_results <- bind_rows(u1, u2, u3, u4)
Uniprot_results <- unique(Uniprot_results)
Uniprot_results$go_ids <- gsub(" ", "", Uniprot_results$go_ids)
head(Uniprot_results)
dim(Uniprot_results)
nrow(filter(Uniprot_results, grepl("GO",go_ids))) #Genes with GO terms
```

#### Blast2GO

Load in Blast2GO/InterProScan results. Remember, these output from these two methods were merged in Blast2GO.

```{r}
B2G_results <- read_tsv("0-BLAST-GO-KO/3-BLAST2GO/Mcap_blast_200806_GO_200811_Interpro_200824.txt", col_names = TRUE)
B2G_results <- B2G_results[,c(3:5, 7:8,10:11)]
colnames(B2G_results) <- c("seqName", "top_hit", "length", "eValue", "simMean", "GO_IDs", "GO_names")
head(B2G_results)
dim(B2G_results)
#nrow(filter(B2G_results, grepl("GO",GO_IDs))) #Genes with GO terms... Commented out because all have go terms
```

#### Primary Assessment: Find unique and overlapping GO terms between the different terms

Generate lists of GO terms for each method
```{r, warning=FALSE, message=FALSE}
Uniprot_GO <- select(Uniprot_results, top_hit, go_ids)
splitted <- strsplit(as.character(Uniprot_GO$go_ids), ";") #split into multiple GO ids
gene_ontology <- data.frame(v1 = rep.int(Uniprot_GO$top_hit, sapply(splitted, length)), v2 = unlist(splitted)) #list all genes with each of their GO terms in a single row
colnames(gene_ontology) <- c("gene_id", "GO.ID")
gene_ontology <- separate(gene_ontology, GO.ID, into = c("GO.ID", "ontology", "term"), sep=" ") #Split GO.ID, terms and ontologies into separate columns
Uniprot.GOterms <- select(gene_ontology, gene_id, GO.ID)
Uniprot.GOterms$GO.ID<- as.character(Uniprot.GOterms$GO.ID)
Uniprot.GOterms[Uniprot.GOterms == 0] <- "unknown"
Uniprot.GOterms$GO.ID <- replace_na(Uniprot.GOterms$GO.ID, "unknown")
Uniprot.GOterms$GO.ID <- as.factor(Uniprot.GOterms$GO.ID)
Uniprot.GOterms$gene_id <- as.factor(Uniprot.GOterms$gene_id)
Uniprot.GOterms$GO.ID <- gsub(" ", "", Uniprot.GOterms$GO.ID)
Uniprot.GOterms <- unique(Uniprot.GOterms)
nrow(Uniprot.GOterms)

B2G_GO <- select(B2G_results, top_hit, GO_IDs)
splitted <- strsplit(as.character(B2G_GO$GO_IDs), ";") #split into multiple GO ids
gene_ontology <- data.frame(v1 = rep.int(B2G_GO$top_hit, sapply(splitted, length)), v2 = unlist(splitted)) #list all genes with each of their GO terms in a single row
colnames(gene_ontology) <- c("gene_id", "GO.ID")
gene_ontology <- separate(gene_ontology, GO.ID, into = c("GO.ID", "ontology", "term"), sep=" ") #Split GO.ID, terms and ontologies into separate columns
B2G.GOterms <- select(gene_ontology, gene_id, GO.ID)
B2G.GOterms$GO.ID<- as.character(B2G.GOterms$GO.ID)
B2G.GOterms[B2G.GOterms == 0] <- "unknown"
B2G.GOterms$GO.ID <- replace_na(B2G.GOterms$GO.ID, "unknown")
B2G.GOterms$GO.ID <- as.factor(B2G.GOterms$GO.ID)
B2G.GOterms$gene_id <- as.factor(B2G.GOterms$gene_id)
B2G.GOterms$GO.ID <- gsub(" ", "", B2G.GOterms$GO.ID)
B2G.GOterms <- unique(B2G.GOterms)
nrow(B2G.GOterms)
```

Find intersections and unique results for each methods
```{r}
UB <- intersect(B2G.GOterms, Uniprot.GOterms) #Blast2Go and Uniprot intersection
nrow(UB)

Uunique <- setdiff(Uniprot.GOterms, B2G.GOterms) #Uniprot unique
nrow(Uunique)

Bunique <- setdiff(B2G.GOterms, Uniprot.GOterms) #Blast unique
nrow(Bunique)
```

#### Merge Annotations

Match top_hits with description
```{r}
Mcap_annot <- left_join(blast, B2G_results, by="seqName")
Mcap_annot <- select(Mcap_annot, seqName, top_hit.x, length.x, evalue, bitscore, simMean, GO_IDs, GO_names)
Mcap_annot <- rename(Mcap_annot, "top_hit"="top_hit.x")
Mcap_annot <- left_join(Mcap_annot, Uniprot_results, by="top_hit")
Mcap_annot$GO <- paste(Mcap_annot$GO_IDs, Mcap_annot$go_ids, sep=';') #generate new column with concatenated GO IDs
Mcap_annot$GO_terms <- paste(Mcap_annot$GO_names, Mcap_annot$gene_ontology, sep=';') #generate new column with concatenated GO IDs
Mcap_annot <- select(Mcap_annot,-c("GO_IDs", "GO_names", "gene_ontology", "go_ids", "length"))
colnames(Mcap_annot) <- c("gene_id", "description", "length","eValue", "bitscore","simMean", "UniProtKB_entry", "status", "protein_names", "gene_names","organism","ko","GO_IDs","GO_terms")
names(Mcap_annot)
head(Mcap_annot)
tail(Mcap_annot)
dim(Mcap_annot)
```

#### Final Assessment: Compare new and old annotation (if applicable). In this portion, we also make a table with that provides an overall summary of our annotation success.

Load old annotation
```{r}
old_annot <- read.csv("0-BLAST-GO-KO/OLD_200306_Mcap_annotations.csv", sep=",")
head(old_annot)
nrow(old_annot)
```

Find
  1) Number of genes with significant alignments
  2) Number of genes with Kegg mappings
  3) Number of genes with GO mappings
  4) Total number of GO terms
  5) Number of unique GO terms
  6) Total number of Kegg terms
  7) Number of unique Kegg terms
  8/9) Avg/med evalue
  10/11) Avg/med bitscore
  

Find metrics for old annotation
```{r}
old_Sig_Alingments=nrow(filter(old_annot, Accession!="#N/A")) #Number of genes with significant alignments
old_Genes_with_Kegg=nrow(filter(old_annot, KEGG!="0")) #Number of genes with Kegg mappings
old_Genes_with_GO=nrow(filter(old_annot, Annotation.GO.ID!="0")) #Number of genes with GO mappings

old.metrics <- old_annot %>% filter(Accession!="#N/A")
old.metrics$E.value <- as.numeric(old.metrics$E.value)
old.metrics$Bitscore <- as.numeric(old.metrics$Bitscore)
old.avg.Eval <-  mean(old.metrics$E.value)
old.median.Eval <- median(old.metrics$E.value)
old.avg.bit <- mean(old.metrics$Bitscore)
old.median.bit <- median(old.metrics$Bitscore)

# total GO terms
old_GO <- select(old_annot, Name, Annotation.GO.ID)
splitted <- strsplit(as.character(old_GO$Annotation.GO.ID), ";") #split into multiple GO ids
old.GOterms <- data.frame(v1 = rep.int(old_GO$Name, sapply(splitted, length)), v2 = unlist(splitted)) #list all genes with each of their GO terms in a single row
colnames(old.GOterms) <- c("gene_id", "GO.ID")
old_totGO_narm <- filter(old.GOterms, GO.ID!="0")
old_totGO <- nrow(old_totGO_narm)
# total unique GO terms
old_uniqueGO <- unique(old_totGO_narm$GO.ID)
old_uniqueGO <- length(old_uniqueGO)

# total Kegg terms
old_Kegg <- select(old_annot, Name, KEGG)
splitted <- strsplit(as.character(old_Kegg$KEGG), ";") #split into multiple Kegg ids
old.Keggterms <- data.frame(v1 = rep.int(old_Kegg$Name, sapply(splitted, length)), v2 = unlist(splitted)) #list all genes with each of their Kegg terms in a single row
colnames(old.Keggterms) <- c("gene_id", "Kegg.ID")
old_totKegg <- nrow(filter(old.Keggterms, Kegg.ID!="0"))
# total unique GO terms
old_uniqueKegg <- filter(old.Keggterms, Kegg.ID!="0")
old_uniqueKegg <- unique(old_uniqueKegg$Kegg.ID)
old_uniqueKegg <- length(old_uniqueKegg)
```

Find metrics for new annotation
```{r}
new_Sig_Alingments=nrow(Mcap_annot)
new_Genes_with_GO <- nrow(filter(Mcap_annot, grepl("GO",GO_IDs))) #Genes with GO terms...
new_Genes_with_Kegg <- nrow(filter(Mcap_annot, grepl("K",ko))) #Genes with Kegg terms...

new.avg.Eval <- mean(Mcap_annot$eValue)
new.median.Eval <- median(Mcap_annot$eValue)
new.avg.bit <- mean(Mcap_annot$bitscore)
new.median.bit <- median(Mcap_annot$bitscore)

# total GO terms
new_GO <- select(Mcap_annot, gene_id, GO_IDs)
splitted <- strsplit(as.character(new_GO$GO_IDs), ";") #split into multiple GO ids
new.GOterms <- data.frame(v1 = rep.int(new_GO$gene_id, sapply(splitted, length)), v2 = unlist(splitted)) #list all genes with each of their GO terms in a single row
colnames(new.GOterms) <- c("gene_id", "GO.ID")
new_totGO_narm <- filter(new.GOterms, GO.ID!="NA")
new_totGO <- nrow(new_totGO_narm)
# total unique GO terms
new_uniqueGO <- unique(new_totGO_narm$GO.ID)
new_uniqueGO <- length(new_uniqueGO)

# total Kegg terms
new_Kegg <- select(Mcap_annot, gene_id, ko)
splitted <- strsplit(as.character(new_Kegg$ko), ";") #split into multiple Kegg ids
new.Keggterms <- data.frame(v1 = rep.int(new_Kegg$gene_id, sapply(splitted, length)), v2 = unlist(splitted)) #list all genes with each of their Kegg terms in a single row
colnames(new.Keggterms) <- c("gene_id", "Kegg.ID")
new.Keggterms$Kegg.ID <- replace_na(new.Keggterms$Kegg.ID, "NA")
new_totKegg_narm <- filter(new.Keggterms, Kegg.ID!="NA")
new_totKegg <- nrow(new_totKegg_narm)
new_uniqueKegg <- unique(new_totKegg_narm$Kegg.ID)
new_uniqueKegg <- length(new_uniqueKegg)
```

Compile into table for comparison
```{r}
Old_annotation=c(old.avg.Eval, old.median.Eval, old.avg.bit, old.median.bit, old_Sig_Alingments, old_Genes_with_GO, old_totGO, old_uniqueGO, old_Genes_with_Kegg, old_totKegg, old_uniqueKegg)
New_annotation=c(new.avg.Eval, new.median.Eval, new.avg.bit, new.median.bit, new_Sig_Alingments, new_Genes_with_GO, new_totGO, new_uniqueGO, new_Genes_with_Kegg, new_totKegg, new_uniqueKegg)
oldVSnew <- data.frame(Old_annotation, New_annotation)
str(oldVSnew)
rownames(oldVSnew) <-  c("Average Evalue", "Median Evalue", "Average bitscore", "Median bitscore", "Number of genes with significant alignments", "Number of genes with GO mappings", "Total number of GO terms", "Number of unique GO terms", "Number of genes with Kegg mappings", "Total number of Kegg terms", "Number of unique Kegg terms")
oldVSnew
```

#### Finally, save your annotations!

```{r}
write_tsv(Mcap_annot, "0-BLAST-GO-KO/Output/200824_Mcap_Blast_GO_KO.tsv")
```

---

Your annotation pipeline is now complete! Based on the results of your final assessment, you may choose to re-run portions of your annotation pipeline or stray from what I've done and try something new. Whatever you decide to do, best of luck!
