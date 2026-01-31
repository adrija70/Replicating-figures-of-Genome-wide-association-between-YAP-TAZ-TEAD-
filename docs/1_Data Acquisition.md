# Step 1 – Data acquisition

This step is about getting oriented to the data itself. Before running any tools, I found it useful to pause and understand what the files actually represent and why there are so many of them.

## What does the raw data represent?

Before starting with an introduction to the files or type of files, I would like to refer a video for someone with computational science background who stumbled on this tutorial. This video is a simple, non-complicated insight into the biology of ChIP-seq- https://youtu.be/rlnN0DklF40?si=QIxhCXj0Lo3KwYTF

The data used here come from ChIP–seq experiments.
In practical terms, this means we are working with DNA fragments that were bound by specific proteins, sequenced, and saved as text files.

### Understanding SRR, SRX, and why there are multiple files per target

Public ChIP-seq data in GEO/SRA is organized hierarchically. 

At the lowest level is the SRR (Sequence Read Run). An SRR contains the raw sequencing reads produced by a single sequencing run. When downloaded, this is what expands into FASTQ files (single-end or paired-end). This is the unit that actually gets aligned and processed.

Single-end sequencing reads each DNA fragment from one end, producing one read per fragment.

Paired-end sequencing reads each DNA fragment from both ends, producing two reads per fragment with a known insert size relationship.

<img width="1852" height="573" alt="image" src="https://github.com/user-attachments/assets/cb1fad72-be7e-42ff-869d-0a11f015fcc8" />


Above this is the SRX (Experiment). An SRX defines the experimental context under which one or more sequencing runs were generated: assay type (e.g. ChIP-seq), target (e.g. YAP or TAZ), antibody, cell type, and library preparation. An SRX does not contain sequencing reads itself; instead, it groups together all SRRs that belong to the same experimental setup.

### Why are there multiple files?

One thing that can feel confusing at first is that the data do not come as a single file.
Instead, there are usually many files associated with one experiment.

ChIP–seq experiments are performed with replicates and often include control samples. Because sequencing is often performed across multiple lanes or runs, a single ChIP experiment commonly maps to multiple SRRs.

Each replicate or condition is stored as a separate sequencing files. In such cases, the multiple SRRs represent technical replicates of the same experiment rather than independent biological samples.

This explains why targets like YAP or TAZ often appear with several SRR accessions but a shared SRX, BioSample, and GEO accession. The biological sample and experimental conditions are the same; only the sequencing runs differ.

Thinking of these files as *raw material* rather than results helped me keep expectations in check.

## Where does the data come from?

For published genomics studies, raw sequencing data are usually deposited in public repositories such as GEO (Gene Expression Omnibus) and the Sequence Read Archive (SRA).
These repositories act as archives for the primary data behind the figures shown in papers.

For this project, the idea is to start from the same raw ChIP–seq data used in the original study.
That way, whatever differences or similarities we see later can be traced back to the analysis itself rather than to differences in input data.

## Outcome of this step

By the end of this step, the goal is simply to have access to the raw sequencing files associated with the study.

In the next step, I assess the basic technical quality of these files before doing anything more involved with them.

---

**Previous:** [Workflow overview](workflow.md)  
**Next:** [Step 2 – Quality control](2_Running%20FastQC%20on%20the%20data%20to%20ensure%20quality%20control.md)

