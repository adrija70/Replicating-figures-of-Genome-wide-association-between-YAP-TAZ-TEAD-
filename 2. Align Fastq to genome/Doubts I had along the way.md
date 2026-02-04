### Doubts

w**hat is the difference between (genomics) adrija@Islingr:~/projects/genomics$ and (base) adrija@Islingr:~/projects/genomics$**

The `(genomics)` prefix means you're using the "genomics" Conda environment (your project-specific setup with Bowtie2). The `(base)` prefix means you're in Conda's default "base" environment.[anaconda+1](https://anaconda.org/bioconda/bowtie2)

## Environment prefixes explained

Conda environments isolate software versions to prevent conflicts:

- `(base)`: Default environment with basic Python—**don't install Bowtie2 here**.[conda](https://docs.conda.io/projects/conda/en/stable/user-guide/install/linux.html)
- `(genomics)`: Your custom environment—**Bowtie2 and genomics tools go here**.youtube

## Switch environments

`conda activate genomics    # changes (base) → (genomics)
conda deactivate           # changes (genomics) → (base)
conda env list             # lists all environments`

Stay in `(genomics)` for your project—run `bowtie2 --version` there to confirm it works.[anaconda](https://anaconda.org/bioconda/bowtie2)

~/miniconda3/          # ← Miniconda lives here (permanent)
├── bin/conda          # ← Manager (shared by all envs)
├── envs/
│   ├── base/         # ← Default env
│   └── genomics/     # ← Your project env (Bowtie2 here)

**How can I view everything present in the genomics env?**

Use `conda list` to see all packages installed in the current "genomics" environment.[conda+1](https://docs.conda.io/projects/conda/en/stable/user-guide/install/linux.html)

## View packages in genomics env

First ensure you're in the right environment, then list contents:

`textconda activate genomics          # switch to genomics env
conda list                       # shows all packages + versions
conda list bowtie2               # shows only bowtie2 details`

Example output:

`text# Name                    Version                   Build  Channel
bowtie2                    2.5.4               hdf5ab88_0    bioconda
python                     3.10.12         h7a1cb2a_0    conda-forge`

## Explore environment files

`textconda info --envs              # all envs + paths
conda env export > env.txt     # save full list to file
ls ~/miniconda3/envs/genomics  # raw folder contents`

**how can I create new folders?**

mkdir new_folder          # creates "new_folder" here
mkdir folder1 folder2     # creates multiple folders
mkdir -p path/to/deep/folder  # creates nested folders (no error if exists)

**how can I delete data/reference folder?**

Delete folders with `rm -r` (recursive) command—safe for empty or full directories.[datacarpentry.github+1](https://datacarpentry.github.io/shell-genomics/aio.html)

## Delete data/reference folder

From `~/projects/genomics`:

`textrm -r data/reference          # deletes folder + all contents
ls data/                      # verify: reference/ gone`

## Safety first

`textrm -ri data/reference         # interactive: prompts before delete
ls -la data/reference/        # list contents first (optional)`

- `r`: Recursive (deletes folders + files inside).
- `i`: Interactive (asks "delete? y/n" for each file).
- **Never** use `rm -rf ~/*` (deletes everything!).

**where in the laptop can I find the folders?** 

Your **`~/projects/genomics/`** folder lives in WSL's Linux filesystem, accessible from Windows Explorer at **`\\wsl$\Ubuntu\home\adrija\projects\genomics`**.

## Access from Windows

**Method 1: Network path**

- Open File Explorer
- Type **`\\wsl$\Ubuntu`** in address bar (or **`\\wsl.localhost\Ubuntu`**)
- Navigate: **`home`** → **`adrija`** → **`projects`** → **`genomics`**
- See **`data/reference/GRCh38_noalt_as/`** + **`data/fastq/`**

<q>~/projects/genomics/
├── data/
│   ├── reference/
│   │   └── GRCh38_noalt_as/    # ← 3.5GB index files here
│   └── fastq/
│       └── YAP.fastq.gz        # ← your reads (download next)</q> where in my laptop can I find these folders?

**how to find the contents of a folders?** 

ls                # current folder
ls -l             # detailed view (permissions, sizes, dates)
ls -la            # detailed + hidden files (.git, .bashrc)
ls data/          # specific folder
ls data/reference/GRCh38_noalt_as/  # nested path
