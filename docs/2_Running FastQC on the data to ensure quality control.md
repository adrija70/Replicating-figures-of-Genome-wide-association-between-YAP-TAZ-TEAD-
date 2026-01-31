# Step 2 – Quality control (FastQC)

At this point, I had access to the raw sequencing files, but I was in the dark regarding the quality of the reads. 

## Quality Control in this context

Quality control, in this context, is a way of checking whether the sequencing data show any obvious technical issues that might affect downstream analysis. If there are strong technical biases or problems in the raw reads, they tend to propagate through every later step and become harder to diagnose. QC gives a baseline understanding of the data before we attach any genomic interpretation to it.

## What FastQC is doing

FastQC is commonly used at this stage. It is a tool for determining the quality of the reads.

Conceptually, FastQC looks at properties of the sequencing reads themselves:
- how read quality changes along their length
- whether certain bases are overrepresented
- whether there are unexpected patterns that suggest technical artifacts

## Interpreting QC results 

A feature of QC is that the output contains many plots and the information imparted by each of them is different.

QC plots are diagnostic, some warnings are common and do not necessarily invalidate the data.
What matters more is whether there are consistent, severe issues across files that would make later steps unreliable.

The goal is just to notice patterns and get a general sense of the data.

## Assessing the quality of multiple files

QC is often the first time it becomes obvious that we are dealing with multiple files rather than a single dataset.

Running QC on one file is straightforward.
Running it on many files quickly raises practical questions about organization, naming, and comparison, and running loops!!

## Outcome of this step

By the end of this step, the goal is to have a basic sense of the technical quality of the raw sequencing files.

In the next step, we move from raw reads to genomic coordinates by aligning the data to a reference genome.

---

**Previous:** [Step 1 – Data acquisition](1_Data%20Acquisition.md)  
**Next:** [Step 3 – Alignment](3_Aligning%20reads%20to%20the%20genome.md)

