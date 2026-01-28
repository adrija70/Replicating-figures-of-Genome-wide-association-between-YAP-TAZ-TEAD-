# Step 5 – Peak calling

Up to this point, the data have been summarized as continuous signal across the genome.
While this representation is useful for visualization and exploration, it is still difficult to work with analytically.

Peak calling is the step where continuous signal is converted into a set of discrete genomic regions.
These regions are meant to capture places where binding is consistently enriched above background.

## Why peaks are introduced at all

Working with continuous signal everywhere in the genome is unwieldy.
Most downstream questions require us to talk about *specific regions* rather than vague patterns.

Peak calling provides a way to say:
“Here are the locations where something reproducible seems to be happening.”

This simplification is not about finding the one true answer.
It is about creating a manageable representation of the data that can be compared, summarized, and linked to other genomic features.

## What a “peak” actually represents

A peak is not a direct observation.
It is the result of applying a statistical model and a set of thresholds to the signal data.

In practical terms, a peak marks a region where:
- the signal is higher than expected by chance
- the enrichment is consistent enough to be flagged
- the region stands out relative to local background

Different tools and parameters can produce different peak sets from the same data.
This does not mean one is right and the others are wrong—it reflects the fact that peak calling involves choices.

## Thresholds, statistics, and uncertainty

Peak calling relies on thresholds: significance cutoffs, enrichment levels, or model assumptions.
These thresholds are necessary, but they are not absolute.

What helped me was to treat peak calling as a filtering step rather than a discovery moment.
We are narrowing the data to regions that are *worth paying attention to*, not declaring definitive binding sites.

Uncertainty does not disappear here; it is simply compressed into a smaller set of regions.

## Why peak calling comes after signal generation

Peak calling depends on having a stable, summarized representation of the data.
Calling peaks directly from raw reads would make the process overly sensitive to noise and local fluctuations.

Signal tracks provide the context that allows peaks to be defined relative to background.
In that sense, peak calling is less about detecting spikes and more about interpreting structured signal.

## How peaks are used downstream

Once peaks are defined, they can be:
- compared across samples or conditions
- associated with genes or enhancers
- overlapped with other datasets
- used to recreate specific panels in published figures

At this stage, peaks function as anchors.
They allow us to connect computational results back to biological questions.

## Outcome of this step

By the end of this step, the continuous signal has been reduced to a set of discrete genomic regions that represent candidate binding sites.
These regions are not final answers, but they are a practical summary of the data.

In the next section, we will use these outputs to connect the analysis back to the specific figures presented in the original paper.
