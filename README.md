# Nudivirus_genome-assembly

# 🦠 Trypoxylus dichotomus Nudivirus (TdNV-Korea) Genome Project

This project presents a bioinformatic pipeline for the identification and assembly of the **nudivirus genome** from **raw SRA data** of the Korean rhinoceros beetle (*Trypoxylus dichotomus*). The work is based on the study published in *Journal of Virological Methods*:

📄 **Reference**  
Kim JY, et al. (2023). *Genomic analysis of a nudivirus isolated from Trypoxylus dichotomus in Korea reveals features of a novel OrNV strain.*  
DOI: [10.1016/j.jviromet.2023.114768](https://www.sciencedirect.com/science/article/pii/S0168170223001296?via%3Dihub)

---

## 📂 Data Availability

- **SRA Accession**: [`SRS2584474`](https://www.ncbi.nlm.nih.gov/sra/SRS2584474)

---

## 🔬 Summary

- **Host**: *Trypoxylus dichotomus* (Korean rhinoceros beetle)  
- **Virus**: Trypoxylus dichotomus Nudivirus (TdNV-Korea)  
- **Genome size**: 126,408 bp  
- **Key findings**:
  - Genomic structure conserved with other *Oryctes rhinoceros nudiviruses* (OrNV)
  - Lowest number of ORFs among known OrNVs
  - Three hypothetical genes absent only in TdNV-Korea
  - Core genes contain SNPs and indels affecting amino acid sequences

---

## 🧬 Bioinformatics Workflow

The pipeline is implemented using standard bioinformatics tools and structured in the following steps.

---

### 🔹 Step 1: Download Raw SRA Data

We used the NCBI SRA Toolkit to download the raw sequencing data and convert to FASTQ format.

```bash
# Install SRA Toolkit if not already installed
# Download data using prefetch and fastq-dump
prefetch SRS2584474
fastq-dump --split-3 --gzip SRS2584474

### 🔹 Step 2: Quality Control and Read Filtering
Use fastp to remove low-quality reads (Q < 30) and generate quality reports.

```bash
fastp \
  -i SRS2584474_1.fastq.gz -I SRS2584474_2.fastq.gz \
  -o filtered_R1.fastq.gz -O filtered_R2.fastq.gz \
  -q 30 -u 10 \
  -h fastp_report.html -j fastp_report.json
-q 30: trim reads with quality < Q30

-u 10: remove reads with >10% low-quality bases

-h/-j: generate HTML and JSON quality reports

🔹 Step 3: Viral Genome Assembly
Assemble filtered reads into contigs using SPAdes genome assembler.

bash

spades.py \
  --careful --only-assembler \
  -1 filtered_R1.fastq.gz -2 filtered_R2.fastq.gz \
  -o spades_output
The --careful option reduces mismatches and short indels.
The resulting contigs will be stored in spades_output/contigs.fasta.

🔹 Step 4: Viral Contig Identification
Use BLASTx to identify viral contigs from the assembled sequences by comparison to the NCBI non-redundant protein database.

bash
blastx -query spades_output/contigs.fasta \
  -db nr \
  -evalue 1e-5 \
  -outfmt 6 \
  -num_threads 8 \
  -out blastx_results.txt
-evalue 1e-5: set significance threshold

-outfmt 6: tabular output

-num_threads: number of CPU threads

Tip: Filter BLAST results using awk, grep, or import into R/Python for downstream annotation.
