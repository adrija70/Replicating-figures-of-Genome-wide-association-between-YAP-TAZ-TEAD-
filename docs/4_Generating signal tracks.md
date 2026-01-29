# Step 4 – Generating signal tracks

After alignment, the data consist of millions of reads placed at specific genomic positions.

## Why individual reads are hard to work with

Looking at reads one by one does not give much intuition about broader patterns. It is also not humanly possible across bulk data files. Also the focus is not on a single read, but whether many reads accumulate in the same region, and that is what I am trying to find out- which is the region where the reads accumulate.

## Why signal tracks are useful

Once reads are summarized into signal tracks, several things become easier.

Patterns of enrichment are more visible.
Different samples can be compared more directly.
The data can be visualized alongside genomic features such as genes or enhancers.

## Outcome of this step

By the end of this step, the aligned reads have been converted into signal tracks that summarize binding across the genome.
The data are now easier to visualize and compare, but still require further processing to define discrete regions of enrichment.
