
# Replicating published ChIP–seq figures — a guided walkthrough

This repository documents a step-by-step attempt to understand and replicate key figures from the paper  
*“Genome-wide association between YAP/TAZ/TEAD and AP-1 at enhancers drives oncogenic growth”*.

---

## Recommended path

1. [Workflow overview](workflow.md)  
   A conceptual map of how raw sequencing data turn into figures in a paper.

2. [Step 1 – Data acquisition](1_Data%20Acquisition.md)  
   What the raw data are, where they come from, and why there are many files.

3. [Step 2 – Quality control](2_Running%20FastQC%20on%20the%20data%20to%20ensure%20quality%20control.md)  
   How to assess whether sequencing data look reasonable before analysis.

4. [Step 3 – Alignment](3_Aligning%20reads%20to%20the%20genome.md)  
   Placing sequencing reads into a genomic coordinate system.

5. [Step 4 – Signal generation](4_Generating%20signal%20tracks.md)  
   Turning aligned reads into continuous signal tracks.

6. [Step 5 – Peak calling](5_Peak%20calling.md)  
   Reducing continuous signal to discrete genomic regions.

7. [Step 6 – Replicating Figure 1a](6_Replicating%20figure1a.md)  
   Connecting the computational workflow back to a published figure.

Each page focuses on *why* a step exists and *how it fits into the larger analysis*, rather than on memorizing commands.
