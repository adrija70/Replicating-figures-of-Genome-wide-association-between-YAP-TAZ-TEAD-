# Home
Replicating genome-wide YAP/TAZ/TEAD figures — a step-by-step guide for biologists

This repository is written for life science students and wet-lab researchers who want to understand and reproduce published genomics figures, but do not come from a computational background.

The tutorial walks through the replication of key figures from the paper  
*“Genome-wide association between YAP/TAZ/TEAD and AP-1 at enhancers drives oncogenic growth”*,  
focusing on how ChIP–seq data are processed, analyzed, and interpreted to support the biological conclusions.

Rather than presenting a collection of scripts or a dense README, this project is structured as a linear course.
Each step explains **why** a computational operation is performed before showing **how** it is done.

The tutorial is organized into sequential steps that mirror the logic of a ChIP–seq analysis:

- understanding the data and where it comes from  
- assessing data quality  
- aligning reads to the genome  
- generating signal tracks  
- calling peaks  
- connecting computational outputs back to published figures  

You are encouraged to move through the pages in order.
Each step builds conceptually on the previous one, and skipping ahead may make later sections harder to follow.

To begin, start with the workflow overview, which provides a high-level picture of the full analysis before diving into details.
