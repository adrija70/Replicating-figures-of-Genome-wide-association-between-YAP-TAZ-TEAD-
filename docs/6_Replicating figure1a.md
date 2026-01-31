# Connecting the analysis to Figure 1a

Up to this point, the tutorial has focused on individual processing steps: data acquisition, QC, alignment, signal generation, and peak calling. Figure 1a in the paper presents a genome-wide view of overlap between YAP and TAZ.

Each step in the workflow plays a specific role in making Figure 1a possible.

Raw sequencing data provide the starting material, but contain no positional information.
Alignment places reads into a shared genomic coordinate system.
Signal tracks summarize binding intensity across the genome.
Peak calling identifies regions that are consistently enriched.

## What to focus on when comparing figures

When comparing your outputs to Figure 1a, it helps to ask:

- Do the same types of regions show enrichment?
- Are the relative relationships between factors preserved?
- Does the figure support the same biological interpretation?

## Outcome of this section

By this point, the computational workflow has been connected back to a concrete figure from the paper.

**Previous:** [Step 5 – Peak calling](5_Peak%20calling.md)
