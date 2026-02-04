A BigWig file is a binary file format commonly used in bioinformatics to efficiently store and visualize continuous data over genomic coordinates, such as read coverage or signal intensity. It is derived from the Wiggle (WIG) format but optimized for faster access and reduced file size, making it suitable for large-scale genomic data.

For this purpose, we will use deeptools

We will use [bamCoverage](https://deeptools.readthedocs.io/en/develop/content/tools/bamCoverage.html) to generate bigwig files.

Problem 1: deeptools doesnt work for python version 3.14
Solution:  

```python
conda create -n deeptools -c conda-forge -c bioconda python=3.10 deeptools
conda activate deeptools
```

**Generating bigwig files for all samples**

```python
for bam in *sorted.bam
do
  bamCoverage --bam $bam --normalizeUsing RPKM --extendReads 200 -o ${bam/sorted.bam/bw}
  done
```

- What does the code mean?
    
    This is a **deepTools** command that converts an aligned BAM file into a **normalized genome-wide coverage track** (BigWig). It looks compact, but every flag is doing real biological and computational work.
    
    I’ll unpack it cleanly, left to right.
    
    ---
    
    ### `bamCoverage`
    
    A deepTools utility that:
    
    - Reads **aligned sequencing reads** from a BAM file
    - Computes **per-base (or per-bin) coverage**
    - Writes the result as a **BigWig (`.bw`)** file for visualization or downstream analysis
    
    BigWig is efficient, indexed, and designed for genome browsers (IGV, UCSC).
    
    ---
    
    ### `-bam $bam`
    
    - `$bam` is a **shell variable**
    - It expands to the path of your input BAM file (typically sorted and indexed)
    
    Example:
    
    ```bash
    bam=/data/sample1.sorted.bam
    
    ```
    
    So this flag tells bamCoverage:
    
    > “Use this BAM file as input.”
    > 
    
    ---
    
    ### `-normalizeUsing RPKM`
    
    This is **library-size normalization**.
    
    RPKM = **Reads Per Kilobase per Million mapped reads**
    
    What this does:
    
    - Corrects for **sequencing depth**
    - Allows **comparisons between samples**
    - Scales coverage so that:
        - Longer regions don’t dominate
        - Larger libraries don’t look artificially stronger
    
    Mathematically:
    
    [
    
    \text{RPKM} = \frac{\text{read count}}{\text{region length (kb)} \times \text{total mapped reads (millions)}}
    
    ]
    
    Important implication:
    
    - This is **not** ideal for all use cases (e.g., differential binding in ChIP-seq often prefers CPM or RPGC)
    - It *is* common for RNA-seq–style coverage visualization
    
    ---
    
    ### `-extendReads 200`
    
    This flag **extends each read to 200 bp** in the 3′ direction.
    
    Why this exists:
    
    - Many sequencing protocols sequence only one end of a fragment
    - The real DNA fragment is longer than the read
    
    So this option:
    
    - Treats each read as if it represents a **200 bp fragment**
    - Produces smoother, more biologically realistic coverage
    
    This is especially relevant for:
    
    - ChIP-seq
    - ATAC-seq (though ATAC often uses fragment length inference instead)
    
    Caveat:
    
    - If your BAM already contains **proper paired-end fragments**, this can distort coverage
    - It is best suited to **single-end data**
    
    ---
    
    ### `o ${bam/sorted.bam/bw}`
    
    This is **bash parameter substitution**, not deepTools syntax.
    
    What it does:
    
    - Takes the value of `$bam`
    - Replaces the substring `sorted.bam` with `bw`
    
    Example:
    
    ```bash
    bam=sample.sorted.bam
    
    ```
    
    Expansion:
    
    ```bash
    ${bam/sorted.bam/bw}  →  sample.bw
    
    ```
    
    So this flag defines:
    
    > “Write the output BigWig file with a name derived from the input BAM.”
    > 
    
    This avoids hard-coding filenames and keeps pipelines reproducible.
    
    ---
    
    ### What this command does in one sentence
    
    It takes a BAM file, computes genome-wide read coverage normalized by RPKM, extends each read to 200 bp to approximate fragment size, and writes the result as a BigWig file suitable for genome browser visualization.
    
    ---
    
    ### Subtle but important assumptions baked into this command
    
    - Your BAM is **sorted and indexed**
    - Your data is **single-end or fragment length ≈ 200 bp**
    - RPKM is an appropriate normalization for your biological question
    - You want **continuous signal**, not peak calling
    
    If any of those assumptions are false, the command should change.
    
    ---
    
    If you want, the next logical step is to discuss **when this exact command is wrong**, and what to use instead for ChIP-seq vs RNA-seq vs ATAC-seq.
