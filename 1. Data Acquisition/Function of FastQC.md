FastQC is a quality-control tool for raw next-generation sequencing (NGS) data. It is one of the very first steps in any sequencing analysis pipeline, regardless of whether your downstream work involves RNA-seq, ChIP-seq, ATAC-seq, whole-genome sequencing, or variant calling. You need FastQC because raw FASTQ files always contain technical artefacts, and you cannot trust or interpret the data until you know its quality.

FastQC reads your **FASTQ** files and produces an HTML report with plots summarizing:

1. P*er-base sequence quality*
    
    Identifies whether your reads degrade toward the 3' end or show low-quality positions.
    
    This tells you if trimming is required.
    
2. P*er-sequence quality distribution*
    
    Shows whether some reads are consistently low quality, which may cause mapping failures.
    
3. *Per-base sequence content*
    
    Detects strong nucleotide biases or adaptor contamination.
    
4. *GC content distribution*
    
    Highlights contamination or library prep issues.
    
5. *Adaptor and overrepresented sequences*
    
    Detects adaptor sequences, primer dimers, or unexpected contamination (e.g., rRNA).
    
6. *Duplicated reads*
    
    High duplication levels indicate PCR artefacts or low library complexity.
    
7. *Sequence length distribution*
    
    Useful for confirming that trimming, fragmentation, or library prep worked as intended.
    

In simple terms:

FastQC checks whether your sequencing data is clean, usable, and biologically meaningful before you perform any analysis.

### 1. Per-base sequence quality

This is the most important plot.

**What you want:**

- The median (yellow line) should stay in the green zone (Q ≥ 28–30).
- Slight drop at the 3′ end is normal.

**What indicates a problem:**

- Sharp decline toward Q20 or lower at the 3′ end
- Any region with median quality < Q20

**Action:**

- Perform adapter/quality trimming (Trimmomatic, fastp, Cutadapt).
- Over-trimming is harmful, so only trim low-quality ends.

---

### 2. Per-sequence quality scores

This shows the distribution of mean read quality.

**What you want:**

- A single peak at high quality (Q30+).
- Narrow distribution.

**Problem indicators:**

- A second peak at low quality.
- Broad distribution extending into low quality.

**Action:**

- Filter out low-quality reads.
- Check sequencing run quality with the facility if the issue is global.

---

### 3. Per-base sequence content

In most RNA-seq, WGS, ChIP-seq datasets, each position should have roughly equal A/T/G/C content.

**What you want:**

- Four lines overlapping or close.
- Minor imbalance at the beginning is common due to random hexamer bias.

**Problem indicators:**

- Extreme nucleotide bias across the entire read.
- Divergent lines throughout (can indicate contamination or technical bias).

**Action:**

- If early-cycle bias only: ignore (common in RNA-seq).
- If persistent: investigate contamination or library prep issues.

---

### 4. GC content

FastQC compares your reads to a theoretical distribution for your organism.

**What you want:**

- A single smooth peak matching the reference GC curve.

**Problem indicators:**

- A significantly shifted peak (suggests contamination).
- Multiple peaks (mixed species, adapter overrepresentation, rRNA contamination).

**Action:**

- Reassess library prep; check for unexpected species.
- Remove obvious contaminants if identifiable.

---

### 5. Sequence duplication levels

Indicates library complexity.

**What you want:**

- Most reads appear only once or a few times.

**High duplication may mean:**

- PCR overamplification.
- Low input material.
- For ChIP-seq: biologically expected to some degree.
- For RNA-seq: high duplication is normal in highly expressed genes.

**Action:**

- If unexpected: review library prep or reduce PCR cycles.
- For ChIP-seq: interpret in context of enrichment levels.

---

### 6. Overrepresented sequences

Lists sequences appearing too frequently.

**Possible causes:**

- Adapter contamination
- rRNA contamination
- PCR primer dimers
- Highly expressed transcripts (RNA-seq)

**Action:**

- Use adapter trimming tools.
- For RNA-seq, abundant sequences may be biologically valid, not an issue.

---

### 7. Adapter content

Shows how much of your reads contain adapters.

**What you want:**

- Flat line at zero or near-zero.

**Problem indicators:**

- Rising or high adapter levels at the 3′ end.

**Action:**

- Always trim adapters before alignment.
- Use Cutadapt or fastp.

---

### 8. Per-base N content

N = ambiguous base call.

**What you want:**

- Close to zero.

**Problem indicators:**

- Rising N content at the 3′ end
- Sudden spikes (cycle failure)

**Action:**

- Trim affected regions.
- If widespread: the run quality itself is poor.

---

### 9. Sequence length distribution

Should match your expected library prep.

**What you want:**

- A single peak at your expected read length (e.g., 75 bp, 150 bp).

**Problem indicators:**

- Multiple peaks (mixed read lengths)
- Unexpectedly short reads (instrument or trimming issues)

**Action:**

- Confirm raw FASTQs are correct.
- Re-run library prep if the source is biological inconsistency.

### Practical Summary

If you see:

| FastQC Warning | Interpretation | Action |
| --- | --- | --- |
| Per-base quality drop | Low-quality sequencing cycles | Trim + reevaluate |
| High adapter content | Library not cleaned | Adapter trimming |
| High duplication | Low complexity | Reduce PCR cycles or accept if RNA-seq |
| GC shift | Contamination | Identify contaminant |
| Overrepresented sequences | Adapters or rRNA | Trimming or rRNA depletion |

---
