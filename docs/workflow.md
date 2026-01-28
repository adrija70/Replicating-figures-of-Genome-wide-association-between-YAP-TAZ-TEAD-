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


