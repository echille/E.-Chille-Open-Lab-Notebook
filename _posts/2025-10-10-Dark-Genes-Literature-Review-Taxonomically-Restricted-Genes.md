# Dark Genes Literature Review – Taxonomically Restricted Genes
**Traits of new, lineage-specific genes:**
* Have higher tissue-specific expression in Green tea cultivar (Zhao & Ma 2021).
* In mangrove plants, new genes had lower overall expression and higher tissue-specific expression (Ma et al. 2021).
* Shorter (Lipman et al. ~[2002](https://genome.cshlp.org/content/13/10/2213.full#ref-18)~ found in a comparison between two prokaryotes, yeast, *Drosophila*, and humans), lower GC-content, lower codon usage bias, fewer exons (Domazet-Loso & Tautz 2003 orphan genes in *Drosophila*).
  * LSGs have shorter gene lengths, fewer exons, higher GC content and higher isoelectric point (Green tea Zhao & Ma 2021).
    * Paper linking gene length, GC content, and transcriptional plasticity: ~[Genetically encoded transcriptional plasticity underlies stress adaptation in Mycobacterium tuberculosis - Nature Communications](https://www.nature.com/articles/s41467-024-47410-5)~.
* More orphan genes expressed in *Drosophila* embryos (Domazet-Loso & Tautz 2003).
* Orphans often show **conditional expression**, with ~10% only expressed under specific conditions (Seçkin et al. 2025).
* Many dynamically regulated in specific tissues, developmental stages, tumors, or in response to stressors like COVID-19 (Seçkin et al. 2025).
* Orphans often exhibit **tissue- or stage-specific expression profiles** supporting roles in lineage-specific traits and ecological adaptations (Seçkin et al. 2025).

**Functional annotation**
* Ma et al. 2021 used guilt-by-association to putatively annotate lineage-specific genes by the genes in their coexpression module.

**Origins**
* Duplication: Rapid sequence divergence, often neutral or accessory; may become important adaptations under environmental change, then stabilize (Domazet-Loso & Tautz 2003).
* Important for microevolution between lineages, specific ecological adaptations (Domazet-Loso & Tautz 2003).
* De novo origin from noncoding regions is more common than previously thought (Khalturin et al. 2009; Seçkin et al. 2025).
  * Orphans cluster in **telomeric “rummage regions”** rich in noncoding DNA and duplications, favorable for recombination, mutation, and new gene birth (Seçkin et al. 2025).
* Emergence and classification depend on depth of phylogenetic sampling; presence in only one or a few closely related species can help date origin (Seçkin et al. 2025).
* Rapid turnover: de novo genes tend to be short-lived, evolving quickly or lost, unlike more stable duplicated genes (Seçkin et al. 2025).
* Incomplete lineage sorting suggests some de novo genes persist as neutral polymorphisms before fixation (Seçkin et al. 2025).

⠀
**Functions**
* “OGs have been identified in nearly every species, and that they play a critical part in major growth and developmental pathways through interacting with conserved transcription factors, central regulators, and receptors [**~[5](https://www.mdpi.com/2221-3759/11/2/27#B5-jdb-11-00027)~**, **~[20](https://www.mdpi.com/2221-3759/11/2/27#B20-jdb-11-00027)~**, **~[148](https://www.mdpi.com/2221-3759/11/2/27#B148-jdb-11-00027)~**]” (Fakhar et al. 2023).
* Reproduction: Frequently expressed in gonads; contribute to male reproductive success and sperm function, under strong selection (Seçkin et al. 2025).
* Brain & cognition: Some de novo genes contribute to cortical development, brain expansion, and are dysregulated in Alzheimer’s disease (Seçkin et al. 2025).
* Disease: In humans, *PBOV1* and *SMIM45* linked to cancer and brain development (Seçkin et al. 2025).
* Ecology: In fungi, orphan genes mediate host–pathogen interactions (*Osp24* in *F. graminearum*) and symbiosis in ectomycorrhizal fungi (Seçkin et al. 2025).
* General role: Orphans evolve specialized functions addressing unique ecological, developmental, or reproductive challenges, making them a **source of evolutionary novelty** (Seçkin et al. 2025).

⠀
**Thoughts** Many works show that genes with low pleiotropy and weak gene-gene interactions (~[Papakostas et al. 2014](https://www.nature.com/articles/ncomms5071#auth-Spiros-Papakostas-Aff1)~, ~[Hao-Chih et al. 2023](https://link.springer.com/article/10.1186/s12915-023-01558-6#auth-Hao_Chih-Kuo-Aff1)~) tend to be the most plastic and adaptive. Since de novo genes are not highly connected and often locally/conditionally expressed, this lack of connectivity may facilitate their ability to become adaptive.
* Fits with the **rapid turnover hypothesis**: weak evolutionary constraints allow de novo genes to adapt quickly or be lost (Seçkin et al. 2025).

⠀
**Workflow notes**
* **WGCNA**
  * Testing for stability of modules:
    * “The stability of the modules was examined with 100 bootstrap replicates to assess the overlap of the module labels between nonsampled and resampled data sets. The overlap was estimated using Fisher’s exact test based on the module assignments. If the proportion of resampled data sets had significant overlap (*P* < 0.01) >70%, then the module was considered statistically robust.” (~[Mäkinen et al. 2018](https://academic-oup-com.proxy.libraries.rutgers.edu/gbe/article/10/1/77/4774499)~).
* **GOseq**
  * Summarizing results:
    * “The GO categories were summarized using SimRel semantic similarity measure to avoid interpretation of redundant categories. The merging of semantically similar GO categories was based on hierarchical clustering with a user-specified cutoff value *C*. A cutoff value 0.5 was used to merge similar categories, corresponding to 1% chance of merging two randomly generated categories (Supek et al. 2011). The *P*-values of the initial enrichment analyses were used to select a representative GO term for each merged category. Thus, the lowest *P*-value among the merged categories was selected as the representative GO term.” (~[Mäkinen et al. 2018](https://academic-oup-com.proxy.libraries.rutgers.edu/gbe/article/10/1/77/4774499)~).

⠀
**Key papers:**
* ~[Functional innovation through new genes as a general evolutionary process - Nature Genetics](https://doi.org/10.1038/s41588-024-02059-0)~ – review. Good explanation of mechanisms and evolutionary trends underlying new genes: the Out of Testis expression pattern, sexual selection, innovation under reduced pleiotropy.
* Seçkin et al. 2025 (EcoEvoRxiv) – overview of orphan and de novo genes in fungi and animals; emphasizes conditional expression, genomic context (rummage regions), rapid turnover, and functions in reproduction, cognition, disease, and ecological adaptation.

Peng & Zhao 2024
- “we found that de novo genes are under faster sequence evolution compared to other protein-coding genes (Fig. ~[2e](https://www.nature.com/articles/s41467-024-45028-1#Fig2)~). The adaptation rates, nonadaptation rates, and proportions of adaptive changes of de novo genes are higher than other genes (Fig. ~[2e](https://www.nature.com/articles/s41467-024-45028-1#Fig2)~), indicating that both adaptive evolution and relaxation of purifying selection contribute to the elevated evolutionary rates of de novo genes. “
- more likely to be transmembrane or secretory proteins (Fig. ~[2c](https://www.nature.com/articles/s41467-024-45028-1#Fig2)~) and male-specific or tissue-specific 
- although high rate of sequence evolution in de novo genes, protein structure largely remains the same
- We noticed that de novo genes have increased strength of purifying selection (decreased ωna) with de novo gene ages, while the adaptation rates change little, which suggests that older de novo genes might be related to more important functions
- areas with open chromatin formation are sources for de novo gene origination
- Intriguingly, our research highlights the potential for the immune system (GO enrichment *P*-value = 7e-4) to serve as another hotspot for de novo gene origination.
