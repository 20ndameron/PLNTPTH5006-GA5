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

*Answer*: The number of transcripts in the file: `28624`

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
du -sh "$outdir"/salmon_index
```

The output of the command was:

```
1.1G    results/salmon_index
```

*Answer*: How long it took to create the index, and how you found out:

- It took 23 seconds to run the `salmon_index.sh` script. To check this, I ran `scontrol show job <job-id>` in the terminal and looked at the `RunTime` section output. I further confirmed this by looking at the slurm output log and subtracting the completion time from the start time. 

*Answer*: The size of the index dir: `1.1 Gb`

### 8: Run Salmon for all samples

### 9: The transcripts with the most reads

*Answer*: What the `NumReads` column contains:

```bash
# Your command:
tail -n +1 results/salmon_quant/S63/quant.sf | sort -k5 -rn | head -n 3
```

The output of the command was:

```
XM_039591534.2  6566    6315.086        14518.656821    24004.889
XM_039573735.1  6445    6194.086        5342.073324     8663.254
XM_039573733.1  6591    6340.086        2876.480244     4774.749
```

### 10: The gene of the top transcript

```bash
# Your command:
zcat data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz | grep ">XM_039591534.2"
```

The output of the command was:

```
>XM_039591534.2 PREDICTED: Culex pipiens pallens vitellogenin-A1-like (LOC120426760), mRNA
```

*Answer*: The gene that the top transcript belongs to: `vitellogenin-A1-like gene`

### 11: Mapping rates

```bash
# Your command:
grep "mapping rate" results/salmon_quant/S*/logs/salmon_quant.log | sort -k3 -n
```

The output of the command was:

```
results/salmon_quant/S83/logs/salmon_quant.log:mapping rate: 76.1504%
results/salmon_quant/S85/logs/salmon_quant.log:mapping rate: 76.9540%
results/salmon_quant/S74/logs/salmon_quant.log:mapping rate: 78.2753%
results/salmon_quant/S82/logs/salmon_quant.log:mapping rate: 78.7291%
results/salmon_quant/S68/logs/salmon_quant.log:mapping rate: 78.8070%
results/salmon_quant/S65/logs/salmon_quant.log:mapping rate: 79.1055%
results/salmon_quant/S71/logs/salmon_quant.log:mapping rate: 79.9047%
results/salmon_quant/S75/logs/salmon_quant.log:mapping rate: 80.3915%
results/salmon_quant/S79/logs/salmon_quant.log:mapping rate: 80.4061%
results/salmon_quant/S64/logs/salmon_quant.log:mapping rate: 81.9152%
results/salmon_quant/S66/logs/salmon_quant.log:mapping rate: 81.9350%
results/salmon_quant/S80/logs/salmon_quant.log:mapping rate: 82.1721%
results/salmon_quant/S69/logs/salmon_quant.log:mapping rate: 82.1824%
results/salmon_quant/S70/logs/salmon_quant.log:mapping rate: 82.3060%
results/salmon_quant/S77/logs/salmon_quant.log:mapping rate: 82.3281%
results/salmon_quant/S76/logs/salmon_quant.log:mapping rate: 82.3396%
results/salmon_quant/S63/logs/salmon_quant.log:mapping rate: 82.4560%
results/salmon_quant/S67/logs/salmon_quant.log:mapping rate: 83.0459%
results/salmon_quant/S86/logs/salmon_quant.log:mapping rate: 83.0532%
results/salmon_quant/S84/logs/salmon_quant.log:mapping rate: 83.2207%
results/salmon_quant/S81/logs/salmon_quant.log:mapping rate: 83.6795%
results/salmon_quant/S78/logs/salmon_quant.log:mapping rate: 84.2490%
```

*Answer*: The sample with the lowest mapping rate: `Sample S83`

------------------------------------------------------------------------

## Part D: Summarizing all steps with MultiQC

### 12: Update your MultiQC script

### 13: Run MultiQC

*Answer*: For each program, the number of reports that MultiQC found,
and why it found this number:

- The output below shows the total reports that MultiQC found for each program run. No reports were found for the salmon_index folder as this folder only contains the reference index used for aligning our transcript file. For salmon_quant, MultiQC found 22 meta reports, 22 fragment length distributions, and 22 library format count reports. Salmon performs pseudo-alignment on our trimmed reads by mapping them to our reference transcriptome file and estimates how many reads match this transcript. It outputes three reports; a meta report (summarizes salmon run and key parameters), a fragment length report (the quantification report of the gene fragment lengths), and  a library format count report (summarizes library format). There are 22 reports for each report type because there are 22 total samples. However, when looking at the MultiQC html output, MultiQC only shows the summary for the fragment lengths as this is the only report that has key result data. The other two reports are summaries of what salmon did. For trim_galore, MultiQC found 44 reports, 2 reports (for forward and reverse reads) for each sample (22 samples). These reports were the summary results from read trimming performed by trim_galore. This script also had trim_galore perform fastqc on each sample with fastqc performing quality control on the trimmed reads. Again, this was done on each read pair (forward and revere) for each sample (22 samples) resulting in 44 total reports. 

```
       file_search | Search path: /fs/ess/PAS3493/people/ndameron/GA5/results
            salmon | Found 22 meta reports
            salmon | Found 22 fragment length distributions
            salmon | Found 22 library format counts reports
       trim_galore | Found 44 reports
            fastqc | Found 44 reports
     write_results | Data        : results/multiqc/multiqc_data
     write_results | Report      : results/multiqc/multiqc_report.html
           multiqc | MultiQC complete

```

### 14: The General Statistics table

*Answer*: The sample with the lowest Salmon mapping rate,
and whether this matches your answer to question 11:

- According to the MultiQC general stats table, sample `S83` had the lowest salmon mapping rate with a rate of `76.2`. This matches the result from sample 11 which also found that sample `S83` had the lowest mapping rate with a rate of `76.1504%`. The only difference between the two reports is the level of significant digits in its answer. 

results/salmon_quant/S83/logs/salmon_quant.log:mapping rate: 76.1504%


### 15: Adapter lengths

*Answer*: The most common adapter lengths, whether this surprised you,
and why you think these lengths are the most common:

- The most common adapter lengths trimmed from the reads based on the Trim Galore adapter length distribution plot was `1 bp` with almost 140,000 reads having this adapter length distribution. This is not suprising as Trim Galore is used to trim off sequencer primers and clean up reads. This process removes the original sequence pattern for the adapter leaving a "random" nucleotide at the begining (with this nucleotide being whatever the nucleotide actually starts the read). An average length distribution of 1 bp helps validate that trim galore properly removed the sequence adapters and only trimmed reads remain. The next most common adapter length distribution is 2 bp and 3 bp which corresponds to reads that happen to start with the same two or three nucleotides. If the average adapter length was in the tens of base pairs, then this could signal that the adapters were not properly trimmed off by trim galore. 

### 16: Ask Claude about the MultiQC report

*Answer*: What you asked, and what you learned from Claude's answer (in your own words):

- From the MultiQC report, I asked Claude to better explain what the status check section and per base sequence content section of fastqc were displaying and provide me with a list of follow up steps that may be needed. Based on Claude's response, the status check section of fastqc shows whether each fastq file passed a set of checks for various parameters and reports out a pass, warn, or fail. Every sample was reported as failing the per base sequence content check and the per sequence GC content check. This is most likely not a meaningful fail due to FastQC being more designed for gDNa rather than RNA. Claude further explained that if a fail or warning is consistent across all fastq files it is more likely an artifact rather than a meaningful issue. Next steps should be to check these failures to further confirm if they show meaningful results. Claude also explained what the per base sequence content chart showed. This plot displays the percentage of each nucleotide at each bp posistion. Plots for each sample should show percentage lines for each nucleotide relatively parrallel to each other. If there was a sharp spike in one nucleotide at a postion, this could signal an issu that needs further investigation. Claude did not recommend any further steps to take for our samples given that salmon was run next, mitigating any issues shown. Claude also stated that many of the "fails" we saw in the MultiQC report were do to the fastq files being RNA rather than DNA. 

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
