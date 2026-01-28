# Connecting the analysis to Figure 1a

Up to this point, the tutorial has focused on individual processing steps: data acquisition, QC, alignment, signal generation, and peak calling.
This page is where those steps come together and start to resemble something from the paper.

The goal here is not to perfectly recreate Figure 1a pixel by pixel.
Instead, it is to understand how the computational outputs we generated relate to what the figure is actually communicating.

## What Figure 1a is trying to show

Figure 1a in the paper presents a genome-wide view of binding patterns for YAP, TAZ, TEAD, and associated factors.
Rather than focusing on individual loci, the figure emphasizes broader trends in where these proteins bind and how their binding relates to regulatory elements.

At a conceptual level, the figure is making an argument about *co-localization and enrichment*, not about any single peak.

Keeping that in mind helped me avoid over-focusing on small visual differences during replication.

## How the previous steps contribute to this figure

Each step in the workflow plays a specific role in making Figure 1a possible.

Raw sequencing data provide the starting material, but contain no positional information.
Alignment places reads into a shared genomic coordinate system.
Signal tracks summarize binding intensity across the genome.
Peak calling identifies regions that are consistently enriched.

Figure 1a is built on top of these abstractions.
It does not display raw reads or QC metrics, but it depends on all of them being reasonable.

## From peaks and signal to visualization

The figure is ultimately a visualization.
That visualization is constructed using signal tracks and peak information to highlight patterns across genomic regions.

What matters here is not the exact numerical values, but the relative distribution of signal:
where binding tends to occur, how sharply it is localized, and how different factors overlap.

This is one of the moments where it became clear to me that figures are summaries of decisions, not direct readouts of data.

## Why replication is never exact

Even when using the same raw data, replication rarely produces figures that look identical to the published ones.
Differences in software versions, parameters, normalization choices, and visualization settings all contribute small variations.

This does not mean the replication failed.
What matters is whether the same qualitative patterns emerge and whether the biological conclusions remain supported.

Thinking of replication as *reproducing reasoning* rather than *reproducing images* made this step much less frustrating.

## What to focus on when comparing figures

When comparing your outputs to Figure 1a, it helps to ask:

- Do the same types of regions show enrichment?
- Are the relative relationships between factors preserved?
- Does the figure support the same biological interpretation?

If the answers to these questions are broadly yes, then the replication has served its purpose.

## Outcome of this section

By this point, the computational workflow has been connected back to a concrete figure from the paper.
The individual steps are no longer isolated procedures, but parts of a coherent argument.

This closes the main loop of the tutorial: from raw data to biological interpretation.

