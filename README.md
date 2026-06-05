# 🧬 Differential Gene Expression Analysis in Breast Cancer
> RNA-Seq based transcriptomic analysis to identify key genes in breast cancer vs. normal tissue

---

## 📌 Project Overview

This project performs **Differential Gene Expression Analysis (DGEA)** on RNA-Seq data from breast cancer tumor and adjacent normal tissue samples. The goal is to identify significantly upregulated and downregulated genes that may serve as **biomarkers or therapeutic targets** for breast cancer.

**Key Finding:** Genes including **COL11A1, MMP11, and MMP13** were strongly differentially expressed — all previously linked to extracellular matrix remodeling, tumor invasion, and metastasis.

---

## 📂 Dataset

- **Source:** NCBI Gene Expression Omnibus (GEO) / Sequence Read Archive (SRA)
- **Samples:** 12 total — 6 tumor + 6 normal (adjacent tissue)
- **Sequencing type:** Paired-end RNA-Seq

| Sample Type | SRR IDs |
|-------------|---------|
| Tumor | SRR37164810, SRR37164811, SRR37164818, SRR37164819, SRR37164820, SRR37164822 |
| Normal | SRR37164812, SRR37164813, SRR37164814, SRR37164815, SRR37164816, SRR37164817 |

---

## 🔧 Tools & Technologies

| Step | Tool |
|------|------|
| Quality Control | FastQC |
| Trimming | Trimmomatic v0.39 |
| Read Alignment | Bowtie2 |
| SAM→BAM Conversion & Processing | SAMtools |
| Gene Quantification | StringTie + prepDE.py |
| Differential Expression | DESeq2 (R) |
| Visualization | ggplot2, pheatmap (R) |
| Reference Genome | hg19 / GRCh37 |

---

## 🔄 Workflow

```
Raw FASTQ files (SRA)
       ↓
Quality Control (FastQC)
       ↓
Trimming (Trimmomatic)
       ↓
Post-trim QC (FastQC)
       ↓
Read Alignment (Bowtie2 → SAM)
       ↓
SAM → BAM conversion (SAMtools)
       ↓
Sorting & Indexing (SAMtools)
       ↓
Gene Quantification (StringTie)
       ↓
Count Matrix (prepDE.py)
       ↓
Differential Expression Analysis (DESeq2)
       ↓
Visualization (PCA, Volcano Plot, Heatmap)
```

---

## ⚙️ Key Commands

**Quality Control**
```bash
/home/BIB/Software/FastQC/fastqc sample_1.fastq.gz sample_2.fastq.gz
```

**Trimming**
```bash
java -jar trimmomatic-0.39.jar PE -threads 8 \
  sample_1.fastq.gz sample_2.fastq.gz \
  sample_1p.fastq.gz sample_1u.fastq.gz \
  sample_2p.fastq.gz sample_2u.fastq.gz \
  ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 \
  SLIDINGWINDOW:4:20 MINLEN:50
```

**Alignment (Bowtie2)**
```bash
bowtie2 -x hg19_bowtie_index \
  -1 sample_1p.fastq.gz -2 sample_2p.fastq.gz \
  -S sample_aligned.sam -p 4
```

**SAM to BAM + Sort + Index**
```bash
samtools view -S -b sample_aligned.sam -o sample_aligned.bam
samtools sort sample_aligned.bam -o sample_sorted.bam
samtools index sample_sorted.bam
```

---

## 📊 Results

### PCA Plot
- PC1 (28%) and PC2 (14%) together explain **42% of total variance**
- Clear **separation between tumor and normal** sample groups confirmed distinct expression profiles

### Volcano Plot
- Identified both **upregulated** (positive log2FC) and **downregulated** (negative log2FC) genes
- Significance threshold: padj < 0.05, |log2FC| > 1

### Heatmap
- Top 50 differentially expressed genes show **distinct clustering** between tumor and normal samples

### Key Differentially Expressed Genes

| Gene | Direction | Biological Role |
|------|-----------|-----------------|
| COL11A1 | Upregulated | Extracellular matrix remodeling |
| MMP11 | Upregulated | Tumor invasion & metastasis |
| MMP13 | Upregulated | Cancer progression |

---

## 📁 Repository Structure

```
├── scripts/
│   ├── deseq2_analysis.R       # Full DESeq2 workflow
│   └── prepDE.py               # Gene count matrix generation
├── results/
│   ├── DESeq2_results.csv      # Full DEG results
│   ├── pca_plot.pdf
│   ├── volcano_plot.pdf
│   └── heatmap_top50.pdf
└── README.md
```

---

## 🧪 How to Run DESeq2 Analysis

```r
library(DESeq2)
library(ggplot2)
library(pheatmap)

# Load count matrix
counts <- read.csv("gene_count_matrix.csv", row.names = 1)

# Sample metadata
sample_info <- data.frame(
  row.names = colnames(counts),
  condition = c(rep("tumor", 6), rep("normal", 6))
)

# Create DESeq2 object and run analysis
dds <- DESeqDataSetFromMatrix(countData = counts,
                               colData = sample_info,
                               design = ~ condition)
dds <- DESeq(dds)
res <- results(dds)
write.csv(as.data.frame(res), "DESeq2_results.csv")
```

---

## 📝 Conclusion

This study successfully identified significant gene expression differences between breast cancer and normal tissues. The findings provide insights into the molecular mechanisms of breast cancer and highlight candidate genes (**COL11A1, MMP11, MMP13**) as potential biomarkers or drug targets.

---

## 👩‍🔬 Author

**Nandini Chittipaka**  
B.Sc. Biotechnology, MG University (2022–2025)  
📧 nandinichittipaka26@gmail.com

---

## 🙏 Acknowledgements

Mentor: **Dr. Neelima Chitturi**  
Institution: Bruhaspathi Institute of Biosciences (OPC) Pvt. Ltd.
