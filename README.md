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

The pipeline is implemented in **R Markdown** and uses standard bioinformatics tools for genome assembly and annotation. A brief overview is as follows:

### 1. Read Filtering

```bash
fastp -i input_R1.fastq.gz -I input_R2.fastq.gz \
      -o filtered_R1.fastq.gz -O filtered_R2.fastq.gz \
      -q 30 -u 10 -h fastp.html -j fastp.json
