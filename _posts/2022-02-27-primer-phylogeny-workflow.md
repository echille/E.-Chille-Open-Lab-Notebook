# Testing if canditate amplicon regions can reconstitute the full phylogeny with all SNPs

**The following protocol describes the process that I used to create a phylogeny of our *P. acuta* samples from a candidate amplicon region, using the profilin region as an example.**

## 1. Extract region of interest from full SNP vcf and export as a fasta file

**Input:** As input, we require **1)** the full VCF file of all SNPs called during the genotyping process (GVCFall_SNPs.vcf.gz), and **2)** a BED file containing the region of interest (amplicon_regions.bed).

++BED file example:++

|CHROM|START|END|ANNOTATION|  
|---|---|---|---|  
|Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968|463903|464598|profilin|  

**Code:** All code was performed on the **coral** Rutgers server in the ```Triploid-Pacu-Gene-Dosage/primer_pcr/amplicon_phylogeny``` directory. I used [tabix](http://www.htslib.org/doc/tabix.html), a tool in the samtools package, to extract the region of interest and used the [vcf2phylip](https://github.com/edgardomortiz/vcf2phylip) python script to export it to fasta format. 

1. Activate the samtools/bcftool conda environment, which uses *samtools v1.15*.  
```
conda activate sam_bcf_toolpak #samtools-v1.15, #bcftools-v1.15
```

2. Index the VCF file containing all SNPs with *tabix*.  
```
tabix -p vcf input_files/GVCFall_SNPs.vcf.gz #index the vcf
```

3. Use *tabix* to extract the SNPs in the profilin protein. Takes as input the BED file described above (```--regions```) and the full SNP VCF file. I used the flag ```--print-header``` to keep all information in the file, most importantly, the sample names.  
```
tabix --print-header --regions input_files/amplicon_region_profilin.bed input_files/GVCFall_SNPs.vcf.gz > output_files/Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968.463903-464598.profilin.vcf
```
4. Use the *vcf2phylip* script to export the profilin SNP vcf file to   
```
./scripts/vcf2phylip.py --input output_files/Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968.463903-464598.profilin.vcf --output-folder output_files --phylip-disable --fasta --write-used-sites --outgroup Pacuta_ATAC_TP7_1445 #export vcf to fasta for phylogeny
```
5. Deactive the conda environment.  
```
conda deactivate
```


**We will use Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968.463903-464598.profilin.min4.fasta as input into raxmlGUI**


## 2) Make phylogeny in raxmlGUI to verify region reconstitutes our full SNP phylogeny

1. Exit the Rutgers **coral** server and ```scp``` the fasta file, Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968.463903-464598.profilin.min4.fasta, to your local computer.

2. Open raxmlGUI. 

3. Load alignment file (Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968.463903-464598.profilin.min4.fasta)

4. Ensure that the GTR model (for nucleotides) is selected

5. Run ML Tree Inference with 500 Runs.

6. Select an outgroup. I selected Pacuta_ATAC_TP7_1445 because it was the weird triploid that reverted back to the diploid state.

7. Click "Run Model Test"

8. Exit RAxML and open FigTree v1.4.4

9. Reroot the tree by preference.

8. Import clade and ploidy .txt annotation file

9. Color tree labels by clade/ploidy and export results as a pdf

**Resulting Tree:**  
![RAxML_GUI_ModelTest_Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968.463903-464598.profilin.min4.tree2](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/RAxML_GUI_ModelTest_Pocillopora_acuta_HIv1___Scaffold_000107F___length_1282968.463903-464598.profilin.min4.tree2.png)
