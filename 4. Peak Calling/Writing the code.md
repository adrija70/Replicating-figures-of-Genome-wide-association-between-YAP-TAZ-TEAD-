### 4. Peak Calling

[`MACS`](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2008-9-9-r137) is the most popular peak caller for ChIPseq. 

```bash
pip install --upgrade macs3
```

The MACS3 callpeak subcommands have many paramters and you want to read https://macs3-project.github.io/MACS/docs/callpeak.html

This is the main function in MACS3. It will take alignment files in various format (please check the detail below) and call the significantly enriched regions in the genome as ‘peaks’

```bash
macs3 callpeak -t YAP.sorted.bam -c IgG.sorted.bam -f BAM -n YAP -g hs --outdir YAP_peak
```

- What does this code mean?
    
    ### The full command
    
    ```
    macs3 callpeak \
      -t YAP.sorted.bam \
      -c IgG.sorted.bam \
      -f BAM \
      -n YAP \
      -g hs \
      --outdir YAP_peak
    
    ```
    
    This is **narrow-peak calling for a ChIP-seq–like experiment**, using a control.
    
    ---
    
    ## What MACS is trying to do (big picture)
    
    MACS is asking one question:
    
    > “Where in the genome is YAP binding more than expected by background noise?”
    > 
    
    Everything in this command supports answering that.
    
    ---
    
    ## `macs3 callpeak`
    
    You are invoking **MACS3’s peak-calling engine**.
    
    - `callpeak` means: identify genomic regions with statistically significant enrichment
    - By default, this is **narrow peak calling** (typical for transcription factors like YAP)
    
    ---
    
    ## `t YAP.sorted.bam`
    
    **Treatment file**.
    
    This is your *signal*:
    
    - BAM file containing aligned reads from YAP ChIP (or CUT&Tag / CUT&RUN)
    - “sorted” matters because MACS assumes coordinate-sorted BAM
    
    Conceptually:
    
    > “These reads may contain real YAP binding + noise.”
    > 
    
    ---
    
    ## `c IgG.sorted.bam`
    
    **Control file**.
    
    This is your *background*:
    
    - IgG pull-down
    - Represents nonspecific binding, chromatin bias, sequencing artifacts
    
    MACS uses this to:
    
    - model local background
    - reduce false positives
    - correct genomic biases
    
    Without `-c`, MACS would rely only on a global background model.
    
    ---
    
    ## `f BAM`
    
    **Input format**.
    
    You are telling MACS:
    
    - “These files are BAMs with aligned reads”
    
    Other formats exist (BED, SAM, etc.), but BAM is standard.
    
    This flag is boring but required.
    
    ---
    
    ## `n YAP`
    
    **Name prefix** for output files.
    
    MACS will generate:
    
    - `YAP_peaks.narrowPeak`
    - `YAP_summits.bed`
    - `YAP_model.r`
    - `YAP_callpeak.log`
    
    This is bookkeeping, not biology.
    
    ---
    
    ## `g hs`
    
    **Effective genome size**.
    
    Here:
    
    - `hs` = human genome (~2.9 × 10⁹ bp)
    
    This tells MACS:
    
    - how large the *statistical search space* is
    - how to scale background expectations
    
    Important reminder:
    
    > This does not filter reads or regions.
    > 
    > 
    > It only affects background modeling and p/q-values.
    > 
    
    ---
    
    ## `-outdir YAP_peak`
    
    Output directory.
    
    All results will be written into:
    
    ```
    YAP_peak/
    
    ```
    
    Prevents clutter and keeps analysis reproducible.
    
    ---
    
    ## What MACS actually does internally (simplified)
    
    1. Reads treatment and control BAMs
    2. Estimates fragment size from strand shift
    3. Extends reads to fragment length
    4. Builds genome-wide pileups
    5. Models background (control + Poisson)
    6. Calls enriched regions
    7. Outputs peaks and statistics
    
    ---
    
    ## What kind of peaks you will get
    
    Because you did **not** specify:
    
    - `-broad`
    - `-nomodel`
    - `-extsize`
    
    MACS assumes:
    
    - **narrow peaks**
    - transcription factor–like binding
    - classic ChIP-seq geometry
    
    This is **appropriate for YAP**.
    
    ---
    
    ## One warning (important, practical)
    
    YAP binding is often:
    
    - context-dependent
    - enhancer-associated
    - sometimes broad or clustered
    
    You may later want to explore:
    
    - `-broad` (for coactivator-like behavior)
    - tuning `-qvalue`
    - adjusting `min-length` / `max-gap`
    
    But this command is a **clean, correct baseline**.
    
    ---
    
    ## One-sentence summary
    
    This command tells MACS to compare YAP ChIP signal against IgG background across the human genome and report statistically significant YAP binding sites as narrow peaks.
    
    Once you understand that intent, every flag becomes obvious rather than mysterious.
    

It takes a couple of minutes. output:

```bash
ls YAP_peak/
YAP1_model.r          YAP1_peaks.narrowPeak YAP1_peaks.xls        YAP1_summits.bed
```

Do it for all samples.

```bash
for bam in *sorted.bam
do
  if [[ "$bam" != "IgG.sorted.bam" ]]; then
    echo macs3 callpeak -t $bam -c IgG.sorted.bam -f BAM -n ${bam%.sorted.bam} -g hs --outdir ${bam/.sorted.bam/_peak}
  fi
done

macs3 callpeak -t TAZ.sorted.bam -c IgG.sorted.bam -f BAM -n TAZ -g hs --outdir TAZ_peak
macs3 callpeak -t TEAD4.sorted.bam -c IgG.sorted.bam -f BAM -n TEAD4 -g hs --outdir TEAD4_peak
macs3 callpeak -t YAP.sorted.bam -c IgG.sorted.bam -f BAM -n YAP -g hs --outdir YAP_peak
```

- What does this code mean?
    
    Good—this is shell logic, not genomics, so let’s demystify it cleanly. I’ll explain **what the shell is doing first**, then **why this is useful in a ChIP-seq context**.
    
    ---
    
    ## What this code does (one sentence)
    
    It **automatically generates a MACS3 peak-calling command for every sorted BAM file except the IgG control**, using IgG as the background for all of them.
    
    Right now it **prints** the commands; it does not execute them.
    
    ---
    
    ## Line-by-line, slowly
    
    ### `for bam in *sorted.bam`
    
    This tells the shell:
    
    > “Loop over every file in the current directory whose name ends with sorted.bam.”
    > 
    
    Example files it might match:
    
    - `YAP.sorted.bam`
    - `TAZ.sorted.bam`
    - `IgG.sorted.bam`
    
    Each filename is stored, one at a time, in the variable `bam`.
    
    ---
    
    ### `do`
    
    Start of the loop body.
    
    ---
    
    ### `if [[ "$bam" != "IgG.sorted.bam" ]]; then`
    
    This is a **conditional filter**.
    
    It says:
    
    > “If the current BAM file is not IgG.sorted.bam, then do the following.”
    > 
    
    Why?
    
    - You want to **call peaks for ChIP samples**
    - You do **not** want to call peaks on the control itself
    
    So IgG is excluded.
    
    ---
    
    ### `echo macs3 callpeak ...`
    
    This is crucial.
    
    Because of `echo`, the command is **printed to the terminal**, not run.
    
    This is intentional and good practice:
    
    - You can inspect the commands
    - Catch naming mistakes
    - Then later remove `echo` to execute safely
    
    ---
    
    ### `t $bam`
    
    Uses the current BAM file as the **treatment**.
    
    If:
    
    ```
    bam=YAP.sorted.bam
    
    ```
    
    then:
    
    ```
    -t YAP.sorted.bam
    
    ```
    
    ---
    
    ### `c IgG.sorted.bam`
    
    Always uses the same **control** for every treatment.
    
    This is standard when:
    
    - all ChIPs share the same background sample
    - same cell type, same sequencing run
    
    ---
    
    ### `n ${bam%.sorted.bam}`
    
    This is **string manipulation**, not biology.
    
    `${bam%.sorted.bam}` means:
    
    > “Take the value of $bam and remove .sorted.bam from the end.”
    > 
    
    Examples:
    
    - `YAP.sorted.bam` → `YAP`
    - `TAZ.sorted.bam` → `TAZ`
    
    This becomes the MACS output prefix.
    
    ---
    
    ### `-outdir ${bam/.sorted.bam/_peak}`
    
    Another string substitution.
    
    `${bam/.sorted.bam/_peak}` means:
    
    > “Replace .sorted.bam with _peak.”
    > 
    
    Examples:
    
    - `YAP.sorted.bam` → `YAP_peak`
    - `TAZ.sorted.bam` → `TAZ_peak`
    
    Each sample gets its own output directory.
    
    ---
    
    ### `fi`
    
    Ends the `if` block.
    
    ---
    
    ### `done`
    
    Ends the loop.
    
    ---
    
    ## What gets printed (example)
    
    If your directory contains:
    
    ```
    YAP.sorted.bam
    TAZ.sorted.bam
    IgG.sorted.bam
    
    ```
    
    The output will be:
    
    ```
    macs3 callpeak -t YAP.sorted.bam -c IgG.sorted.bam -f BAM -n YAP -g hs --outdir YAP_peak
    macs3 callpeak -t TAZ.sorted.bam -c IgG.sorted.bam -f BAM -n TAZ -g hs --outdir TAZ_peak
    
    ```
    
    No peak calling has happened yet.
    
    ---
    
    ## Why this is good practice
    
    This script:
    
    - avoids copy-paste errors
    - enforces consistent parameters
    - scales to many ChIP samples
    - makes analysis reproducible
    
    Once you’re confident the commands look correct, you remove `echo`:
    
    ```bash
    macs3 callpeak ...
    
    ```
    
    and the analysis runs.
    
    ---
    
    ## Final mental model
    
    This is **batch peak calling with one control**, written defensively so you can verify commands before execution.
    
    If you understand this loop, you understand 80% of practical shell scripting used in omics pipelines.
    

We learned something new here!

`if [[ "$bam" != "IgG.bam" ]]; then:` Checks if the current file is not IgG.bam.

remove the “echo” and run it:

```bash
for bam in *sorted.bam
do
  if [[ "$bam" != "IgG.sorted.bam" ]]; then
    macs3 callpeak -t $bam -c IgG.sorted.bam -f BAM -n ${bam%.sorted.bam} -g hs --outdir ${bam/.sorted.bam/_peak}
  fi
done
```

How many peaks we get for each transcription factor?

```bash
find . -name "*Peak"  | xargs wc -l
   11512 ./TEAD4_peak/TEAD4_peaks.narrowPeak
   10719 ./TAZ_peak/TAZ_peaks.narrowPeak
    9807 ./YAP_peak/YAP_peaks.narrowPeak
   32038 total
```

- What does this code mean?
    
    This is a classic Unix one-liner. It looks cryptic because it chains small tools, but each part is simple. Let’s unpack it mechanically and then conceptually.
    
    ---
    
    ## The full command
    
    ```bash
    find . -name "*Peak" | xargs wc -l
    
    ```
    
    This is a **pipeline**: the output of one command becomes the input of the next.
    
    ---
    
    ## Step 1: `find . -name "*Peak"`
    
    ### What `find` does
    
    `find` searches the filesystem.
    
    - `.` → start searching from the **current directory**
    - `name "*Peak"` → match files or directories whose name **ends with `Peak`**
    
    The `*` is a wildcard:
    
    - `Peak` = anything that ends in `Peak`
    
    Examples it might find:
    
    - `YAP_peak/`
    - `TAZ_peak/`
    - `H3K27acPeak`
    
    It prints **one path per line**.
    
    Important:
    
    `find` does not care whether these are files or directories. It just prints paths.
    
    ---
    
    ## Step 2: `|` (the pipe)
    
    The pipe means:
    
    > “Take whatever find prints and feed it as arguments to the next command.”
    > 
    
    No files are opened yet. This is just text flowing between commands.
    
    ---
    
    ## Step 3: `xargs wc -l`
    
    ### What `xargs` does
    
    `xargs` takes lines from standard input and converts them into **command-line arguments**.
    
    So:
    
    ```
    ./YAP_peak
    ./TAZ_peak
    
    ```
    
    becomes:
    
    ```bash
    wc -l ./YAP_peak ./TAZ_peak
    
    ```
    
    ---
    
    ### What `wc -l` does
    
    `wc` = word count
    
    - `l` = count **lines**
    
    For a file:
    
    - it counts the number of lines
    
    For a directory:
    
    - it throws an error (directories don’t have lines)
    
    So the behavior depends on what `*Peak` matches.
    
    ---
    
    ## What this command is *intended* to do
    
    In ChIP-seq / MACS workflows, people often have files like:
    
    - `YAP_peaks.narrowPeak`
    - `TAZ_peaks.narrowPeak`
    
    If the pattern were:
    
    ```bash
    -name "*Peak"
    
    ```
    
    and those were **files**, then:
    
    > This command counts how many peaks (lines) are in each peak file.
    > 
    
    Each line = one peak.
    
    ---
    
    ## But here is the subtle problem (important)
    
    MACS output directories are usually named:
    
    ```
    YAP_peak/
     attach files inside:
       YAP_peaks.narrowPeak
    
    ```
    
    If `*Peak` matches **directories**, then:
    
    - `wc -l` will fail or return `0`
    - The command is semantically wrong
    
    You probably want one of these instead.
    
    ---
    
    ## Correct, biologically meaningful versions
    
    ### Count peaks in narrowPeak files
    
    ```bash
    find . -name "*.narrowPeak" | xargs wc -l
    
    ```
    
    This tells you:
    
    > How many peaks were called per sample.
    > 
    
    ---
    
    ### Safer version (handles spaces, avoids errors)
    
    ```bash
    find . -name "*.narrowPeak" -exec wc -l {} +
    
    ```
    
    This avoids `xargs` pitfalls and is preferred in pipelines.
    
    ---
    
    ## Mental model (keep this)
    
    - `find` → *discover files*
    - `xargs` → *turn text into arguments*
    - `wc -l` → *count peaks*
    
    Nothing here is genomics-specific.
    
    This is Unix plumbing applied to genomics outputs.
    
    ---
    
    ## One-sentence takeaway
    
    This command is trying to **count how many peak entries exist across files or directories named `*Peak`**, but it only works correctly if `*Peak` matches actual peak **files**, not directories.
    
    Once you understand what each tool consumes and emits, these pipelines stop looking like magic and start looking inevitable.
    

According to the manual page:

> NAME_peaks.narrowPeak is BED6+4 format file which contains the peak locations together with peak summit, p-value, and q-value. If you plan to load it to the UCSC genome browser, please make sure that you turn on –trackline option. Definition of some specific columns are:
> 

> 5th: integer score for display. It’s calculated as int(-10log10pvalue) or int(-10log10qvalue) depending on whether -p (pvalue) or -q (qvalue) is used as score cutoff. Please note that currently this value might be out of the [0-1000] range defined in UCSC ENCODE narrowPeak format. You can let the value saturated at 1000 (i.e. p/q-value = 10^-100) by using the following 1-liner awk: awk -v OFS=“ ‘{$5=$5>1000?1000:$5} {print}’ NAME_peaks.narrowPeak
> 

> 7th: fold-change at peak summit
> 

> 8th: -log10pvalue at peak summit
> 

> 9th: -log10qvalue at peak summit
> 

> 10th: relative summit position to peak start
> 

### What is Model Building in MACS?

Model building in MACS (Model-Based Analysis of ChIP-Seq) is a step that attempts to determine the average fragment size of DNA fragments from sequencing data. It uses the shifted positions of the tags (forward and reverse reads) to estimate the peak signal. The estimated fragment size is then used to build a model of the peak shape, which is important for identifying and refining peak regions.

### Model Building: Single-End vs. Paired-End

- Single-End Data:

Model building is used because the fragment size is not directly available from single-end reads. MACS shifts the reads by half the estimated fragment size to align them to the putative binding sites.

- Paired-End Data:

Model building is usually not needed because the fragment size is directly available from the paired-end read alignment. In paired-end mode, MACS calculates the actual insert size between read pairs, bypassing the need for model building.

### When to Use –no-model and –extend-size

For single-end data, you might use –no-model and –extend-size in specific scenarios where you want to skip model building and manually specify the fragment size:

–no-model: 

Disables the model building step. Use this option if you already know the approximate fragment size from your library preparation.

–extend-size:

Extends reads to the specified fragment size (e.g., 200 bp). This mimics the actual fragment length in the absence of paired-end information.

### Best Practices for Single-End Data

- With Model Building (Default Behavior):

Let MACS build the model unless there are specific reasons to disable it.

macs2 callpeak -t treatment.bam -c control.bam -f BED -g hs -n sample –outdir results

- Without Model Building (–no-model):

Use when: You know the fragment size (e.g., from library preparation or empirical testing). The library is not suitable for accurate model building (e.g., very low read depth or noisy data). Specify –extend-size to set the fragment size.

`macs2 callpeak -t treatment.bam -c control.bam -f BED -g hs -n sample --outdir results --no-model --extend-size 200`

# Why Use –no-model and –extend-size for Single-End?

Consistency: You can ensure the fragment size is consistently applied across all datasets. Noise Reduction: Avoid errors from noisy or low-quality data affecting model estimation. Speed: Skipping model building can make peak calling faster.

Model building is relevant for single-end data and used by default unless –no-model is specified. For paired-end data, model building is typically unnecessary since the fragment size is directly calculated. Use –no-model –extend-size for single-end data when you want to bypass model building and directly set the fragment size.
