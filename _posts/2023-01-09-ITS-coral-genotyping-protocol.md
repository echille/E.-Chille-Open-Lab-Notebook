---
layout: post  
title: Protocol for coral colony genotyping using the 18S-28S rDNA marker  
date: '2023-01-09'. 
categories: Protocols  
tags: Bhattacharya Lab, PCR, genotyping  
projects: Bhattacharya Lab, Dark Genes    
---


**The protocol detailed below provides step-by-step instructions for how the Bhattacharya Lab genotyped the *Galaxea spp.* corals in our coral culturing facility and is based on the methodology outlined in [Takabayashi et al. 1998](https://www.researchgate.net/publication/37622411_A_coral-specific_primer_for_PCR_amplification_of_the_internal_transcribed_spacer_region_in_ribosomal_DNA). In short, the 18S-28S region was amplified from extracted DNA by PCR with forward and reverse primers A18S and ITS4, respectively. The resulting amplicons were sequenced at Azenta Life Sciences using the Sanger method. The resulting sequencies were quality-trimmed and aligned using MUSCLE. Pairwise distances between samples were used to inform relatedness.**

---

## Materials  
- Pipettes and pipette tips (P20, P200, P1000)
- 96 well plate and plate sealers OR 0.2mL 8-strip PCR tubes 
- 5 mL Eppendorf tubes
- 1.5 mL Eppendorf tubes
- Primer pair (order from IDT; 25 nmol DNA oligo, Standard desalting, LabReady Formulation)  
    - Forward: A18S-GATCGAACGGTTTAGTGAGG (Takabayashi et al. 1998)  
    - Reverse: ITS4-TCCTCCGCTTATTGATATGC (White et al. 1990)  
- NEB Q5 High-Fidelity 2X Master Mix (aka MasterMix) 
- Molecular grade water 
- Extracted gDNA from each sample (see ["DNA Extraction Protocol for Adult Corals"](https://echille.github.io/E.-Chille-Open-Lab-Notebook/Bhattacharya-Lab-Adult-Coral-DNA-Extraction-Protocol/)  for details)  
- Thermocycler   
- Centrifuge capable of handling a 96-well plate
- Wizard® SV Gel and PCR Clean-Up System
- Supplies to run an Agarose Gel (see ["Bhattacharya Lab Gel Electrophoresis Protocol"](https://echille.github.io/E.-Chille-Open-Lab-Notebook/Bhattacharya-Lab-Gel-Electrophoresis-Protocols/)) and dsDNA HS Qubit Assay
- Computer with WiFi capability

## Protocol

### Tips and Tricks

- Be sure to start with a clean and sterile work bench and pipettes.
- Aliquot primers and MasterMix into several smaller tubes. They can be stored several weeks at 4°C while the rest remains at -20°C.
- Plan out how many wells you will use beforehand and plan to use a few more wells than you need to allow for pipetting error. The number of wells that you'll use can be calculated using the formula below:
```katex
 N wells = (N samples + 1 Blank) x 3 Replicates
```
- Make a diagram of your samples in your 96-well plate beforehand so you can double-check how many wells you need to use and have a template for when you pipette your working solution and samples.  
- When adding samples to the plate, it is easiest to start with a fresh patch of pipette tips and work from left-right, top-bottom; typewriter style. Taking a fresh tip each time you load a sample will help you keep track of which well you just left off on.  
- Before beginning the protocol, I highly recommend thoroughly reading through the Q5 High-Fidelity 2X Master Mix ([link](https://www.protocols.io/view/pcr-with-q5-high-fidelity-2x-master-mix-m0492-4rm7vzo5gx1w/v2)) and Wizard® SV Gel and PCR Clean-Up System ([link]()) manuals

### Step 1: PCR

1. Thaw MasterMix, samples, and primers to thaw over ice. 
2. Dilute each primer to 10 µM, using molecular grade water as the buffer. The final volume should be at least (1.25 µL x 3 replicates x (N samples + 2)).     
3. Dilute the gDNA from each sample to 1 ng/uL, using molecular grade water as the buffer aiming for a final volume of 100uL.   
4. Prepare PCR Reaction Working Solution. Aim to have enough for 20 µL per well, plus a little extra. 
    - 12.5 µL MasterMix x #wells
    - 1.25 µL A18S primer x #wells  
    - 1.25 µL ITS4 primer x #wells  
    - 5.00 µL molecular grade water x #wells
5. Transfer 20 µL of the PCR Reaction into each well.    
6. Transfer 5 µL of gDNA to each well. Each well should now have a total volume of 25 µL of PCR solution. 
7. Spin down your 96-well plate until all liquid has collected at the bottom of the wells and there are no visible bubbles.  
8. Transfer your 96-well plate to the Thermocycler. If you haven't already done so, program your Thermocycler with the parameters below. Run the program. 
    - **Initial denaturation:** 98°C, 30 seconds
    - **35 cycles x**  
        - **Denaturation:** 98°C, 10 seconds  
        - **Annealing:** 53°C, 30 seconds
        - **Extension:** 72°C, 30 seconds  
    - **Final extension:** 72°C, 2 minutes
    - **Hold at 4°C upon completion**  

### Step 2: PCR Quality Check and Cleanup  

1. Taking care not to cross-contaminate your samples, aliquot 15 µL (5 µL from each replicate) of PCR product for each sample into a strip tube. This will be used for your quality checks.  
2. While not completely necessary, I prefer to run a Quibit assay on my samples before running the gel to determine whether my PCR reaction worked and if its worth putting the effort into running a gel. If you're PCR product concentration is very low (<10 ng/µL) you may not have enough to be visible on a gel, and it is very likely that the PCR reaction was unsuccessful.  
3. Run an [agarose gel]((https://echille.github.io/E.-Chille-Open-Lab-Notebook/Bhattacharya-Lab-Gel-Electrophoresis-Protocols/)) with 5-10 µL amplicon to check that you have a single clean band for each sample. The size of the band may vary between species or genuses, but in our Galaxea it was ~1000 bp.  
    - **Tip:** If you get multiple banding from a sample, run each replicate from that sample on the gel again and see the issue is the sample itself or contamination from a single well. Multiple  bands may be the result of primer dimers or secondary priming, which means you may need to troubleshoot the PCR reaction. However, spurious banding may be ok and can be removed through purification.   
4. Aliquot 30 µL (10 µL from each replicate). Follow the Wizard® SV Gel and PCR Clean-Up System protocol ([link](file:///Users/erinchille/Downloads/Wizard%20SV%20Gel%20and%20PCR%20Clean-Up%20System%20TB308.pdf)) for DNA Purification by Centrifugation (5A). If you have multiple banding for a sample, prepare the sample by running the full 30 µL of amplicon on the gel and excising the band of interest, then proceed to use the purification from the gel protocol (4B). Otherwise, proceed using option 4C, which is much easier and faster. Also of note is that if primer dimer is a concern, you may purify the amplicon directly from the PCR product by modifying the protocol according to Note 4 under Step 4C (p6).
5. Re-run the Qubit and gel assay to determine final concentration and purity of the amplicon. Compare this with the initial pre-cleanup concentration and purity to determine success of the purification.
6. **Optional:** Repeat Steps 4 and 5 with the remainder of the PCR product. Note that the PCR product does not store well unless purified.

### Preparing for sequencing

1. If sequencing with Azenta Life Sciences, follow instructions provided on their Sample Submission Guidelines page ([link](https://www.genewiz.com/en/Public/Resources/Sample-Submission-Guidelines/Sanger-Sequencing-Sample-Submission-Guidelines)). 
    - To reduce sequencing cost, I typically choose the following parameters:
        - **Sample Type:** Purified PCR Product  
        - **Primer Type:** Your primer (use the A18S primer as your sequencing primer)  
        - **Will you premix the primer and template?:** Yes  
        - **PCR Product Length (bp):** 1000
2. When samples are ready, place an order for Sanger sequencing of a Purified Template with Azenta Life Sciences ([link](https://www.genewiz.com/en/Public/Services/Sanger-Sequencing/Purified-Templates)) and schedule a pickup time.  

### Sample processing

**Under construction. Please check back in a couple days for updates.**
