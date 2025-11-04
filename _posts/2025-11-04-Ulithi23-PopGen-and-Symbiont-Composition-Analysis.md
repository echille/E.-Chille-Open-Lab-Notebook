# Ulithi23 PopGen and Symbiont Composition Analysis

## Population genomic analysis
#### November 4, 2025
Today, I started using pyani-plus to determine which reference genome – the more complete KBHIv3 genome (97% completeness) or the ULFMv1 genome (85% completeness). Pyani will give us metrics of how closely related the two genomes are by nucleotide similarity. I am currently running a “simplified workflow” with just those two genomes (`screen -r pyani-simple`) and a more complex experimental design (`screen -r pyani`) that includes *Acropora millepora* (JSIDv2.1) as an outgroup, congeners *M. efflorescens* (SLJPv2) and *M. cactus* (SLJPv2), and conspecific from the Big Island, *M. capitata* WTHIv1.1. The simplified workflow is using the ANI algorithm that is considered the “gold standard” ANIb, which uses BLAST+. The complex design uses fastANI, which is faster and perhaps less accurate.

These analyses are being run in `/scratch/erin/Ulithi23/01_Compare_McapKBHIv3_and_McapULFMv1/`

The command I used for the complex analysis is: `pyani-plus fastani ../pyani_input/ --database Montipora.db --name "fastANI test run"`

The command I used for the simple analysis is: `pyani-plus anib ./ --database Mcap.db --create-db --name "ANIb simple comparison"`

**Next steps:** Once we get some idea about the relatedness of these genomes, we can get a better idea of which would be the best to use for the AllSites VCF formation. I will ask Anthony Geneva for advice next Wednesday.

## Symbiont composition analysis
#### November 4, 2025
I started reviewing my proposed methodology from my dissertation proposal today and I plan to start this analysis soon. I can’t decide whether I should do the coral popgen analysis first or this. I probably can’t do both at the same time because of high server traffic.

In my proposal, I wrote that I wanted to use the workflow outlined in [Contaminant or goldmine? In silico assessment of Symbiodiniaceae community using coral hologenomes](https://www.frontiersin.org/journals/protistology/articles/10.3389/frpro.2024.1376877/full), which extracts non-coral reads from bam files (tentatively here: `/storage/timothy/Storage/projects/0050_Ulithi_Trip_2023/03_Analysis/2023-07-17/02_Genotype_Samples/results/mapping_merged/Montipora_capitata_KBHIv3/`), utilizes k-mer counting and frequency computation to compute distances between samples. The scripts to run this analysis are available on GitHub [hisatakeishida/Symb-SHIN: Computational workflow to identify Symbiodiniaceae diversity by Scrapping Hologenome data with IN-sillico methods](https://github.com/hisatakeishida/Symb-SHIN). 

![](https://private-user-images.githubusercontent.com/95674651/300316034-477de686-85c1-4411-81c8-6dc1a373f552.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzMDAzODYsIm5iZiI6MTc2MjMwMDA4NiwicGF0aCI6Ii85NTY3NDY1MS8zMDAzMTYwMzQtNDc3ZGU2ODYtODVjMS00NDExLTgxYzgtNmRjMWEzNzNmNTUyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTExMDQlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMTA0VDIzNDgwNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPThkZGMwNTg1MmEzOTllYTE2ZmJkMjM4MjcxMjRiYWJlODRkNmI3OTM5YmYyNWZjN2Q2ZjE3N2RmNzM2MWRjMDImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.arrCapCCfYV9WktqKVYQAnR1K8VERiZOmEgajeKXoCQ)

**Next steps:**  Confirm that the BAM files I found should contain symbiont reads, if not, figure out where the unmapped reads are stored or re-rerun that part of the analysis. Then, I can test if I can run this analysis at the same time or before running the PopGen analysis.
