# Workflow overview

## Starting point: raw sequencing data

The analysis begins with raw ChIP–seq data obtained from public repositories.
These data consist of short DNA fragments sequenced from chromatin regions bound by transcriptional regulators such as YAP, TAZ, TEAD, and AP-1.

At this stage, the data are simply collections of sequencing reads and do not yet convey any genomic or biological meaning.

## From raw reads to genomic signal

To extract biological insight, the raw sequencing reads are transformed through a series of conceptual steps:

1. **Quality assessment**  
   The integrity and technical quality of the sequencing reads are evaluated to identify potential issues that could bias downstream analysis.

2. **Alignment to the genome**  
   Sequencing reads are mapped to a reference genome, converting short DNA sequences into genomic coordinates.

3. **Signal representation**  
   Aligned reads are aggregated into continuous signal tracks that reflect the relative enrichment of binding across the genome.

4. **Peak identification**  
   Regions of statistically significant enrichment are identified, representing candidate regulatory elements bound by the proteins of interest.

## Biological interpretation of the outputs

Each transformation produces an output with a specific biological interpretation.

Aligned reads indicate where protein–DNA interactions occurred.
Signal tracks summarize binding intensity across genomic regions.
Peaks highlight discrete loci where binding is consistently enriched above background.

Together, these outputs allow us to link transcriptional regulators to specific genomic elements, such as enhancers, that influence gene expression.

## Connecting the workflow to published figures

The final goal of this workflow is not merely data processing, but figure interpretation.
By following these steps, we generate the intermediate data required to recreate the figures presented in the original publication.

Subsequent sections of this tutorial unpack each step in detail, gradually connecting computational outputs back to the biological claims made in the paper.
