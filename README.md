# GA5 answers - Noah Dameron

------------------------------------------------------------------------

## Part A: Your project dir, Git repo, and protocol

### 1: Fill in the top of the protocol

### 2: Markdown formatting

### 3: Create subdirs, and copy the scripts

```bash
# Your mkdir command:
mkdir -p scripts data/ref results
```

### 4: Initialize a Git repository, and commit your first files

### 5: Download the reference transcriptome

Code to count the number of transcripts:

```bash
# Your command:
zcat data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz | grep -c ">"
```

The output of the command was:

```
28624
```

*Answer*: The number of transcripts in the file: 28624

------------------------------------------------------------------------

## Part B: Read trimming

### 6: Run TrimGalore

*Answer*: Whether all batch jobs succeeded, and how you checked that
(and any problems you encountered, and how you solved them):

- When running my loop, I encountered issues with the `$fastq_dir` not properly working and calling the correct files. The main issue with this was that I did not define the variable in my terminal first. I had taken a break between part A and part B and needed to start a new VS Code session. I did not think to rerun the code setting the filepaths first before running the loop causing my loop to not properly call the fastq input files. After resolving this issue, I ran in to no other issues with running the loop. To ensure I was properly calling the correct `r2` file that corresponded with the `r1` file, I refered back to my loop from `GA4` for reference. To confirm all batch jobs succeeded, I first checked my emial to ensure there were no notifications informing me of a failed job (given the script included the `#SBATCH --mail-type=FAIL` which notifies users if their job fails). I next checked the queue (using `squeue -u $USER -l`) to confirm all jobs had successful finished and the only job running was my VS Code job. Finally, I checked each slurm log file outputted to confirm that the script was successful for each sample. 

------------------------------------------------------------------------

## Part C: Transcript quantification with Salmon

### 7: Create the Salmon index

Code to check the size of the index dir:

```bash
# Your command:
```

The output of the command was:

```
```

*Answer*: How long it took to create the index, and how you found out:

*Answer*: The size of the index dir:

### 8: Run Salmon for all samples

<!-- No answer needed here -- we check `protocol.md` directly. -->

### 9: The transcripts with the most reads

*Answer*: What the `NumReads` column contains:

```bash
# Your command:
```

The output of the command was:

```
```

### 10: The gene of the top transcript

```bash
# Your command:
```

The output of the command was:

```
```

*Answer*: The gene that the top transcript belongs to:

### 11: Mapping rates

```bash
# Your command:
```

The output of the command was:

```
```

*Answer*: The sample with the lowest mapping rate:

------------------------------------------------------------------------

## Part D: Summarizing all steps with MultiQC

### 12: Update your MultiQC script

<!-- No need to paste the script: we'll look at scripts/multiqc.sh and its Git history directly. -->

### 13: Run MultiQC

*Answer*: For each program, the number of reports that MultiQC found,
and why it found this number:

### 14: The General Statistics table

*Answer*: The sample with the lowest Salmon mapping rate,
and whether this matches your answer to question 11:

### 15: Adapter lengths

*Answer*: The most common adapter lengths, whether this surprised you,
and why you think these lengths are the most common:

### 16: Ask Claude about the MultiQC report

*Answer*: What you asked, and what you learned from Claude's answer (in your own words):

------------------------------------------------------------------------

## Part E: Improving the workflow with genAI

### 17: Claude's review of your protocol and scripts

*Answer*: At least three of Claude's suggestions,
and whether you agree or disagree with each one, and why:

### 18: Address a suggestion

*Answer*: Which suggestion(s) you addressed, and what you changed:

------------------------------------------------------------------------

## Part F: Publish your repo on GitHub

### 19: Create a GitHub repository and push

<!-- No answer needed here -- we check your GitHub repo directly. -->

### 20: Get a (test) DOI with Zenodo

*Answer*: Your Zenodo Sandbox DOI:
