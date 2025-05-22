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

# 🔹 Step 2: Quality Control and Read Filtering

In this step, we used [`fastp`](https://github.com/OpenGene/fastp) to filter out low-quality reads and generate basic quality control reports.

---

## 🧪 Tool: `fastp`

Fastp is an ultra-fast all-in-one FASTQ preprocessor.  
It performs quality filtering, trimming, and generates both HTML and JSON QC reports.

---

## ⚙️ Command

```bash
fastp \
  -i SRS2584474_1.fastq.gz -I SRS2584474_2.fastq.gz \
  -o filtered_R1.fastq.gz -O filtered_R2.fastq.gz \
  -q 30 -u 10 \
  -h fastp_report.html -j fastp_report.json


# 🔹 Step 3: Viral Genome Assembly

In this step, we used [`SPAdes`](https://github.com/ablab/spades) to assemble the filtered paired-end reads into contigs.

---

## 🧪 Tool: `SPAdes`

SPAdes (St. Petersburg genome assembler) is a popular assembler optimized for small genomes, metagenomics, and single-cell sequencing.

---

## ⚙️ Command

```bash
spades.py \
  --careful --only-assembler \
  -1 filtered_R1.fastq.gz -2 filtered_R2.fastq.gz \
  -o spades_output

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
