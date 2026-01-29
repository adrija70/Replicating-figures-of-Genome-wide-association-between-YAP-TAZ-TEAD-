# Step 5 – Peak calling

Peak calling is the step where continuous signal is converted into a set of discrete genomic regions. These regions are meant to capture places where binding is consistently enriched above background. Simply put, they are the locations where something reproducible seems to be happening.”

## What a “peak” actually represents

It is the result of applying a statistical model and a set of thresholds to the signal data.

In practical terms, a peak marks a region where:
- the signal is higher than expected by chance
- the enrichment is consistent enough to be flagged
- the region stands out relative to local background

Different tools and parameters can produce different peak sets from the same data. IT not mean one is right and the others are wrong—it reflects the fact that peak calling involves choices.

## Thresholds, statistics, and uncertainty

Peak calling relies on thresholds: significance cutoffs, enrichment levels, or model assumptions. These thresholds are necessary, but they are not absolute.

## Outcome of this step

In the next section, we will use these outputs to connect the analysis back to the specific figures presented in the original paper.
