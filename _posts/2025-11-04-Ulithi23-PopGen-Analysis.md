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

