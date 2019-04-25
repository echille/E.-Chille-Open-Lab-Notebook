---
layout: post
title: Protocol for DNA/RNA Extractions of Montipora Coral Larvae Using Zymo-Duet Extraction Kit
tags: [DNA, RNA, larvae]
---


### Materials:
- Zymo Duet DNA/RNA Extraction Kit [HERE](https://files.zymoresearch.com/protocols/_d7003t_d7003_quick-dna-rna_miniprep_plus_kit.pdf)**  
- Thermoixer
- Centrifuge capable of 16,000 rcf spin (Eppendorf)**

_Make sure ethanol has been added to the wash buffer, and that enzymes have been re-hydrated before starting_

### Sample Preparation

1. Take sample tube with larvae 1 at a time out of the -80 to minimize amount of thawing
2. Add 300µl DNA/RNA shield directly to the sample tube
3. Record tube number
4. Add 30µl of PK digestion buffer to each sample tube
5. Add 15µl Proteinase K to each sample tube
6. Vortex and spin down sample tubes
7. Place in Thermoixer for ~3 hours at 55 degrees C, shaking at 1100 rpm. Check periodically to monitor digestion progress.
8. After digestion proceed with DNA and RNA Extraction

### DNA Extraction
1. Set up yellow DNA spin columns and collection tubes, label appropriately
2. Warm elution liquids to 70 degrees C (10mM Tris HCl pH. 8.0 and RNase free water)
3. Add equal volume (345µl) DNA/RNA lysis buffer to each sample tube
4. Finger flick to mix tubes
5. Add 700µl (total volume) of sample gently to the yellow DNA spin column
6. Centrifuge at 16,000 rcf (g) for 30 seconds
7. **Important** Save the flow through from this step: transfer to a new 1.5mL tube labeled for RNA
8. Add 400µl DNA/RNA Prep Buffer gently to the yellow DNA spin columns
9. Centrifuge at 16,000 rcf (g) for 30 seconds
10. Discard flow through (Zymo kit waste)
11. Add 700µl DNA/RNA Wash Buffer gently to the yellow DNA spin columns
12. Centrifuge at 16,000 rcf (g) for 30 seconds
13. Discard flow through (Zymo kit waste)
14. Add 400µl DNA/RNA Wash Buffer genetly to the yellow DNA spin columns
15. Centrifuge at 16,000 rcf (g) for **2 minutes**
16. Discard flow through (Zymo kit waste)
17. Transfer yellow columns to new 1.5mL microcentrifuge tubes
18. Add 50µl warmed 10mM Tris HCl to each yellow DNA column by dripping slowly directly on the filer
19. Incubate at room temp for 5 minutes
20. Centrifuge at 16,000 rcf (g) for 30 seconds
21. Repeat steps 18-20 for a final elution volume of 100µl
22. Label tubes, store at 4 degrees C if quantifying the same day or the next, if waiting longer store in -20

### RNA Extraction
*Can do concurrently with DNA Extraction after DNA Extraction Step 7*
1. Add equal volume (700µl) 100% EtOH to the 1.5mL tubes labeled for RNA containing the original yellow column flow through
2. Vortex and spin down to mix
3. Add 700µl of that liquid to the green RNA spin columns
4. Centrifuge at 16,000 rcf (g) for 30 seconds
5. Discard flow through (Zymo kit waste)
6. Add 700µl to the green RNA spin columns (the rest from the 1.5mL RNA tubes)
7. Centrifuge at 16,000 rcf (g) for 30 seconds
8. Discard flow through (Zymo kit waste)
9. Add 400µl DNA/RNA Wash Buffer gently to each green RNA column
10. Centrifuge at 16,000 rcf (g) for 30 seconds
11. Discard flow through (Zymo kit waste)
12. Make DNase I treatment master mix:
    - 75µl DNA Digestion buffer x # of samples
    - 5µl DNase I x # of samples
13. Add 80µl DNase I treatment master mix directly to the filter of the green RNA columns
14. Incubate at room temp for 15 minutes
15. Add 400µl DNA/RNA Prep Buffer gently to each column
16. Centrifuge at 16,000 rcf (g) for 30 seconds
17. Discard flow through (Zymo kit waste)
18. Add 700µl DNA/RNA Wash Buffer gently to the yellow DNA spin columns
19. Centrifuge at 16,000 rcf (g) for 30 seconds
20. Discard flow through (Zymo kit waste)
21. Add 400µl DNA/RNA Wash Buffer genetly to the yellow DNA spin columns
22. Centrifuge at 16,000 rcf (g) for **2 minutes**
23. Discard flow through (Zymo kit waste)
24. Transfer green columns to new 1.5mL microcentrifuge tubes
25. Add 50µl warmed DNase/RNase free water to each green RNA column by dripping slowly directly on the filer
26. Incubate at room temp for 5 minutes
27. Centrifuge at 16,000 rcf (g) for 30 seconds
28. Repeat steps 25-27 for a final elution volume of 100µl
29. Label 1.5mL tubes on ice afterwards, and aliquot 5µl into PCR strip tubes to save for Qubit and Tape Station to avoid freeze-thaw of your stock sample
30. Store all tubes in the -80

### Extraction Content Analysis
*These steps analyze the quantity and quality of the DNA/RNA extracted and may be done on a separate day from the extraction*

#### RNA/DNA Quantity  
Follow Broad Range dsDNA and RNA Qubit [protocol](https://meschedl.github.io/MESPutnam_Open_Lab_Notebook/Qubit-Protocol/) to analyze sample ++quantity++. Read all samples twice.

#### DNA Quality  
If DNA quantity is sufficient (typically >10 ng/µL) follow the PPP Agarose Gel [Protocol](https://meschedl.github.io/MESPutnam_Open_Lab_Notebook/Gel-Protocol/) to determine DNA quality. "Good" DNA should form a distinct band a the very top of the gel. See example below:  
![annotated-biomin-gel-batches-4-5.png](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/annotated-biomin-gel-batches-4-5.png)

#### RNA Quality  
If RNA quantity is sufficient follow the Tape Station [Protocol](https://meschedl.github.io/MESPutnam_Open_Lab_Notebook/RNA-TapeStation-Protocol/) to determine RNA quality and obtain a RNA Integrity Number (RIN). "Good" RNA should have a RIN above 8.0 and form two distinct peaks at the 18S and 28S locations. See example below:

![TS-biomin-Ext-Batch-5-26.png](https://raw.githubusercontent.com/echille/E.-Chille-Open-Lab-Notebook/master/images/TS-biomin-Ext-Batch-5-26.png)


---


###### From Biomineralization Project