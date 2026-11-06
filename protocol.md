# RNA-seq analysis of _Culex pipiens_

- Author: Noah Dameron
- Date created: 2026-11-06
- Date last modified: 2026-11-06
- This was run at the Pitzer cluster of the Ohio Supercomputer Center (<https://www.osc.edu>)
- OSC working dir: /fs/ess/PAS3493/people/ndameron/GA5
- GitHub repository: https://github.com/20ndameron/PLNTPTH5006-GA5

## Prerequisites

- **Input files**:
  - Paired-end RNA-seq reads are in `../garrigos/fastq`

- **Software**:
  - The scripts run programs using Apptainer containers,
    with links contained inside the scripts.
  - If running at OSC, no software installations are needed.
  - If you're not at OSC, you need to have Apptainer and Slurm installed.

- Run all code from the working dir listed above.
  You don't need to create the output dirs, because the scripts will do so.

- Each script writes a Slurm log file (`slurm-<script>-<jobID>.out`) to the working dir.
  After the jobs of a step have finished, check these logs for errors,
  then move them into a `logs/` dir within that step's output dir.

## Set up: Download the reference transcriptome FASTA file from NCBI

```bash
# Downloaded on 2026-11-06
wget -P data/ref https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/016/801/865/GCF_016801865.2_TS_CPP_V2/GCF_016801865.2_TS_CPP_V2_rna.fna.gz
```

## Set up: Set file paths

```bash
# Inputs:
fastq_dir=/fs/ess/PAS3493/people/ndameron/garrigos/fastq
ref_transcripts=data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz

# Base output dir:
outdir=results
```

## Step 1: Read trimming with TrimGalore

```bash
# `for` loop
for r1 in "$fastq_dir"/*_R1.fastq.gz; do
  sample=$(basename "$r1" _R1.fastq.gz)
  r2="$fastq_dir"/"$sample"_R2.fastq.gz
  sbatch scripts/trimgalore.sh "$r1" "$r2" "$outdir"/trimgalore/"$sample"
done

# move logs of successfule runs
mkdir "$outdir"/trimgalore/logs 
mv slurm*.out "$outdir"/trimgalore/logs
```

## Step 2: Creating a transcriptome index with Salmon

<???>

## Step 3: Pseudo-alignment and quantification with Salmon

<???>

## Step 4: Summarizing the results of all steps with MultiQC

<???>