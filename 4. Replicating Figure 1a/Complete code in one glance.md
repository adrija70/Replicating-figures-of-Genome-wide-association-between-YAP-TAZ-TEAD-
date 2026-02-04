```python
conda activate deeptools
conda install bedtools
cd /home/adrija/projects/genomics/Data/fastq
bedtools intersect -a TAZ_peak/TAZ_peaks.narrowPeak -b YAP_peak/YAP_peaks.narrowPeak -wa | wc -l
bedtools intersect -a TAZ_peak/TAZ_peaks.narrowPeak -b YAP_peak/YAP_peaks.narrowPeak -wa | sort | uniq |  wc -l
```

Transfer you peakcalling and big wig files to windows system. 

The location is: C:\Users\adrij\genomics_project

Then R studio, install packages

```python
install.packages("here")
install.packages("BiocManager")
BiocManager::install(c("GenomicRanges", "rtracklayer"))
```

```python
library(GenomicRanges)
library(rtracklayer)
library(here)

TAZ_peaks <- import(here("fastq_R/TAZ_peak/TAZ_peaks.narrowPeak"))
YAP_peaks <- import(here("fastq_R/YAP_peak/YAP_peaks.narrowPeak"))
TEAD4_peaks <- import(here("fastq_R/TEAD4_peak/TEAD4_peaks.narrowPeak"))
TAZ_peaks
YAP_peaks

TAZ_overlap_YAP_peaks<- subsetByOverlaps(TAZ_peaks, YAP_peaks)
length(TAZ_overlap_YAP_peaks)

YAP_overlap_TAZ_peaks<- subsetByOverlaps(YAP_peaks, TAZ_peaks)
length(YAP_overlap_TAZ_peaks)

devtools::install_github("js229/Vennerable")
library(Vennerable)

n_YAP <- length(YAP_peaks)  # Total peaks 
n_TAZ <- length(TAZ_peaks)  # Total peaks 
n_overlap <- length(YAP_overlap_TAZ_peaks)

venn_data <- Venn(SetNames = c("YAP", "TAZ"),
                  Weight = c(
                    "10" = n_YAP, 
                    "01" = n_TAZ, 
                    "11" = n_overlap      
                  ))

# Plot the Venn diagram
plot(venn_data)
```
