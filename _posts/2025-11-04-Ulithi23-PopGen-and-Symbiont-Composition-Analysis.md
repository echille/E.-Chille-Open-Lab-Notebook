## Population genomic analysis
### November 4, 2025
Today, I started using pyani-plus to determine which reference genome – the more complete KBHIv3 genome (97% completeness) or the ULFMv1 genome (85% completeness). Pyani will give us metrics of how closely related the two genomes are by nucleotide similarity. I am currently running a “simplified workflow” with just those two genomes (`screen -r pyani-simple`) and a more complex experimental design (`screen -r pyani`) that includes *Acropora millepora* (JSIDv2.1) as an outgroup, congeners *M. efflorescens* (SLJPv2) and *M. cactus* (SLJPv2), and conspecific from the Big Island, *M. capitata* WTHIv1.1. The simplified workflow is using the ANI algorithm that is considered the “gold standard” ANIb, which uses BLAST+. The complex design uses fastANI, which is faster and perhaps less accurate.

These analyses are being run in `/scratch/erin/Ulithi23/01_Compare_McapKBHIv3_and_McapULFMv1/`

The command I used for the complex analysis is: `pyani-plus fastani ../pyani_input/ --database Montipora.db --name "fastANI test run"`

The command I used for the simple analysis is: `pyani-plus anib ./ --database Mcap.db --create-db --name "ANIb simple comparison"`

**Next steps:** Once we get some idea about the relatedness of these genomes, we can get a better idea of which would be the best to use for the AllSites VCF formation. I will ask Anthony Geneva for advice next Wednesday.

### November 10, 2025
Last week I was trying to determine which *Montipora* reference genome I should use for one of my analyses. One of the Oahu Montipora genomes we have is 97% complete (as opposed to the 85% complete Ulithi one), so I wanted to see if they were closely related enough that I could get away with using the more complete genome.

What is really cool is that we now have two full genomes from Kanoehe Bay in Hawaii, and one of the genomes that is the same species, but from Waiopae Tide Pools on the Big Island. I figured, if the outbreak Montipora were introduced from Hawaii during WWII, the distance should be about the same (or smaller) as the distance between the Big Island genome and the Oahu genome.

I used a "quick-and-dirty" approach to compare the whole genomes that we have, called sourmash. The sourmash approach takes breaks the genomes up into lots of small fragments, converts those into a numeric representation, keeps just the bits that differ between the genomes, and then summarizes the overlap between the genomes using a metric called a Jaccard Index, where 1 is identical and 0 is zero overlap. 

As you can see below, the Ulithi genome is not within that cluster of *Montipora* genomes from Hawaii. Also, the Hawaii *Montipora*, likely due to their isolation from other reefs, are very distantly related to the other *Montipora*. That doesn't necessarily mean that the outbreak *Montipora* wasn't introduced from elsewhere, but it doesn't seem like it was introduced from Hawaii.

These results agree with a previous analysis ran by Tim, where he used the available *Montipora* and *Acropora* 18S, ITS1, 5.8S, ITS2, and 28S voucher sequences from NCBI to build a tree.

All this means that I'll have to use the Ulithi *Montipora* as my reference genome, even though it is less complete, it is the closest match we've got.

**Next steps:** Because the original SNP calling was done using the KBHIv3 genome, I'll have to redo the individual SNP calling using the ULFM genome before generating the AllSites VCF.

![sourmash results](https://github.com/echille/E.-Chille-Open-Lab-Notebook/blob/master/images/sourmash_compare.results.matrix.png?raw=true)
*Sourmash results. Ulithi Montipora called ULFMv1*

![ITS results](https://github.com/echille/E.-Chille-Open-Lab-Notebook/blob/master/images/coral_marker_region.combined.fasta.mafft.aln.iqtree.contree.png?raw=true)
*ITS results. Ulithi Montipora marked with magenta star. Hawaii Montipora marked with blue bracket.*

## Symbiont composition analysis
### November 4, 2025
I started reviewing my proposed methodology from my dissertation proposal today and I plan to start this analysis soon. I can’t decide whether I should do the coral popgen analysis first or this. I probably can’t do both at the same time because of high server traffic.

In my proposal, I wrote that I wanted to use the workflow outlined in [Contaminant or goldmine? In silico assessment of Symbiodiniaceae community using coral hologenomes](https://www.frontiersin.org/journals/protistology/articles/10.3389/frpro.2024.1376877/full), which extracts non-coral reads from bam files (tentatively here: `/storage/timothy/Storage/projects/0050_Ulithi_Trip_2023/03_Analysis/2023-07-17/02_Genotype_Samples/results/mapping_merged/Montipora_capitata_KBHIv3/`), utilizes k-mer counting and frequency computation to compute distances between samples. The scripts to run this analysis are available on GitHub [hisatakeishida/Symb-SHIN: Computational workflow to identify Symbiodiniaceae diversity by Scrapping Hologenome data with IN-sillico methods](https://github.com/hisatakeishida/Symb-SHIN). 

![](https://private-user-images.githubusercontent.com/95674651/300316034-477de686-85c1-4411-81c8-6dc1a373f552.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzMDAzODYsIm5iZiI6MTc2MjMwMDA4NiwicGF0aCI6Ii85NTY3NDY1MS8zMDAzMTYwMzQtNDc3ZGU2ODYtODVjMS00NDExLTgxYzgtNmRjMWEzNzNmNTUyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTExMDQlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMTA0VDIzNDgwNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPThkZGMwNTg1MmEzOTllYTE2ZmJkMjM4MjcxMjRiYWJlODRkNmI3OTM5YmYyNWZjN2Q2ZjE3N2RmNzM2MWRjMDImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.arrCapCCfYV9WktqKVYQAnR1K8VERiZOmEgajeKXoCQ)

**Next steps:**  Confirm that the BAM files I found should contain symbiont reads, if not, figure out where the unmapped reads are stored or re-rerun that part of the analysis. Then, I can test if I can run this analysis at the same time or before running the PopGen analysis.

### November 10, 2025

Last week, I developed my plan for the analyzing the symbiont composition for the Ulithi *Montipora* samples. Essentially, my plan is to utilize the workflows set up in place by the Cooke lab which are extremely well documented [here](https://github.com/bakeronit/acropora_digitifera_wgs/blob/main/23.symbionts.md0 from the paper by [Zhang et al. 2022](https://doi.org/10.1093/molbev/msac201). Their repository includes both a markdown file explanation of the workflow and well-organized scripts for running the analyses.

First, I wanted to use Kraken to do a genus-level analysis of the data and this would be probably what would be published in the data paper - I'd use more recent/complete genomes than the ones they have listed there. Then, I wanted to do an ITS2 profile like the one they have described to get more fine detail about the symbiont identities. Lastly, I would do the jellyfish and D2S statistics to get clusters to use as metadata for my drivers of proteome-wide variation analysis. It would also be cool to do the jellyfish and D2S statistics with the reads that map to the microbiome (combined reads from the bacteria, archaea, and fungal Ref Seq databases).

Before, I was just going to do the jellyfish and D2S statistics, but I figured I should probably also do the other two analyses for due diligence and because reviewers are going to ask.

Also, update with the .bam files that they probably only contain *Montipora* reads, but Tim could not give definitive confirmation. That's ok though because I think for most of this we can start from FASTQ files.
