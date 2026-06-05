# Breast-cancer-gene-expression
RNA-Seq differential expression analysis of TCGA breast cancer data

PROJECT REPORT 
Differential Gene Expression Analysis In Brest Cancer 
Prepared by: 
Ch . Nandini
B.Sc. Biotechnology 
Bioinformatics Student 
Mentor  :
Dr. Neelima Chitturi 
Founder of Bruhaspathi Institute of Biosciences (OPC) pvt.ltd.
Submitted to: 
Bruhaspathi Institute of Biosciences (OPC) PVT.Ltd
   
Abstract:------
Differential Gene Expression (DEE) analysis is a powerful method used study how gene activity changes between different biological conditions. In this study, RNA sequencing (RNA -seq) data were obtained from the public repository Gene Expression Omnibus (GEO) and corresponding datasets from NCBI Sequence Read Archive. The dataset both normal ( control) and tumor sample of Brest cancer.
The analysis workflow began with downloading raw sequence data, followed quality assessment using FastQC. Low- quality reads and adapter sequence were removed using Trimmomatic to improve data quality. The cleaned reads were then aligned to the human reference genome (hg37 / GRCh37) using alignment tools such as Bowtie2 , and alignment files were processed using SAMtools.
Next, transcript assembly and gene expression quantification were performed using StringTie, which generated gene-level count data . These counts were used to create a gene expression matrix for all samples. Differential gene expression analysis was then carried out conditions based on metadata.
The result revealed significant difference in gene expression between normal and tumor samples. Several gene were found to be upregulated and downregulated in brest cancer, indicates change in important biological process. Visualization techniques such as a principal component analysis (PCA), Heatmap, and volcano plot were used to interpret the results and confirm the separation between sample groups.
Overall, this study provides insights into the molecular mechanisms of Brest cancer and highlights key genes that may serve as potential biomarkers or targets for further therapeutic research.








Introduction 
Breast cancer is one of the most common malignancies affecting women worldwide and remains a major cause of cancer-related mortality. The development and progression of breast cancer are driven by complex genetic and molecular alterations that influence cellular growth, differentiation, and survival. Understanding these molecular changes is essential for improving diagnosis, prognosis, and treatment strategies.
Differential Gene Expression Analysis (DGEA) is a powerful bioinformatics approach used to identify genes that exhibit significant differences in expression levels between two or more biological conditions. In breast cancer research, DGEA helps compare gene expression profiles between tumor and normal breast tissues, enabling the identification of upregulated and downregulated genes associated with cancer development and progression.
The advent of high-throughput sequencing technologies, particularly RNA sequencing (RNA-Seq), has revolutionized gene expression studies by providing comprehensive insights into transcriptomic changes. Through DGEA, researchers can uncover key biomarkers, signaling pathways, and molecular mechanisms involved in breast cancer. These findings contribute to the discovery of potential therapeutic targets and support the development of personalized medicine approaches.
This project aims to perform Differential Gene Expression Analysis on breast cancer RNA-Seq data to identify significantly altered genes and investigate their biological significance. The study involves data preprocessing, quality assessment, read alignment, expression quantification, statistical analysis, and functional interpretation of differentially expressed genes. The results are expected to provide valuable insights into the molecular landscape of breast cancer and highlight genes that may serve as diagnostic or therapeutic biomarkers.







Objectives 
•	1. To compare gene expression profiles between breast cancer tissues and healthy control tissues.
•	2. To identify differentially expressed genes (DEGs) that may play important roles in tumor initiation, progression, and metastasis.
•	3. To discover potential biomarkers for early diagnosis, prognosis, and therapeutic response in breast cancer patients.
•	4. To understand the molecular mechanisms and biological pathways involved in breast cancer progression through functional enrichment and pathway analysis.
•	5. To visualize gene expression patterns using bioinformatics visualization techniques such as heatmaps, volcano plots, Principal Component Analysis (PCA) plots, and hierarchical clustering analysis.
•	6. To support precision medicine approaches by identifying candidate target genes that may contribute to the development of personalized therapies for breast cancer treatment.

Workflow Overview 
Dataset Description 
The dataset used to in this project was obtained from the public databases Gene Expression Omnibus (GEO), and NCBI Sequence Read Archive (SRA). The is based on RNA sequencing (RNA -seq) data of Brest Cancer samples, which includes both normal (control), and tumor (disease) tissue.
The raw sequencing data consists of multiple SRA RUN ID’s such as ;
### Tumour Samples
* SRR37164810
* SRR37164811
* SRR37164818
* SRR37164819
* SRR37164820
* SRR37164822
### Adjacent Normal Tissue Samples
* SRR37164812
* SRR37164813
* SRR37164814
* SRR37164815
* SRR37164816
* SRR37164817

Step 1:
Quality Control Using FASTQC 
FastQC is a bioinformatics tool use to check the quality of sequencing data before starting any analysis. It helps research understand whether the RNA- seq data is good enough to use or if it contains errors or low- quality reads.
In simple words, fastqc is like Quality checking step for DNA / RNA sequencing data. It tells us if the data is clean , reliable , and suitable for further analysis.
Input data
The input for FastQC is raw sequencing data files ( Fasta bowtie2 -x /home/BIB/Databases/HumanGenome_HG37/hg19_bowtie_index -1 SRR37164820_1p.fastq.gz -2 SRR37164820_2p.fastq.gz -S SRR37164820_genome_aligned.sam -p 4files) obtained from NCBI Sequence Read Archive 
Inthis project, the input sample includu: 
• Tumor samples: SRR37164810,SRR37164811, SRR37164818, SRR37164819, SRR37164820, SRR37164822
• Normal samples: SRR37164812, SRR37164813, SRR37164814, SRR37164815,SRR37164816, SRR37164817.
The command use for this step: 
/home/BIB/Software/FastQC/fastqc  sample_1.fastq.gz  sample_2. Fastq.gz

Output Data 
FastQC produces a detailed quality report for each sample. The output includes: 
• HTML report ( visual summary of quality results)
• Text summary report
• Quality graphs and charts .
Step 2:
Trimming
Trimmomatic  is a bioinformatics tool use to clean raw sequencing date before analysis.
Trimmomatic removes all these unwanted parts so that only high -quality clean reads are used for further analysis like differential gene expression .
Input Files
Trimmomatic takes raw FASTQ files as input.
For
 example: 
• sample_1.fastq( Forward reads)
• sample_2. FastQC( Reverse reads )
If it is paired end sequencing 
Thesa Files contain: 
• nucleotide sequences ( A, T, G, C)
• quality scores for each base
Output files
After processing, you get : Clean/ Trimmed FASTQ files 
• sample_1_paired. Fastqc
• sample_2_paired . Fastqc
These are high quality reads used for analysis
Command used for trimming 
Java -jar /home/BIB/Software/Trimmomatic-0.39/trimmomatic-0.39.jar PE -threads 8 Sample_1.fastq.gz Sample_2.fastq.gz SRR37164820_1p.fastq.gz  Sample_1u.fastq.gz Sample_2p.fastq.gz Sample_2u.fastq.gz ILLUMINACLIP:/home/BIB/Software/Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:20 MINLEN:50
Step 3:
Quality Control After Trimming 
After trimming the RAW Sequencing data , quality control is done again to check whether the data is clean and suitable for further analysis. This step ensure that low quality bases and adapter sequence are successfully removed.
In this stage , the fastQC is used to check the quality of the trimmed FASTQ files.FastQC helps to verify important factors like overall reads, presence of any remaining adapter contamination, and sequence quality across all reads.
This step confirms that the trimming process has improved the data quality and the reads are ready for downstream analysis 
The command used for this step 
/home/BIB/Software/FastQC/fastqc Sample_1p.fastq.gz Sample_2p.fastq.gz





Step 4 
Read Alignment Using Bowtie2
Read Alignment is an important step in RNA -seq data analysis. After quality control, the clean sequencing reads are mapped( aligned )to a reference genome to find where each read comes from.
In this study, Bowtie2 is used for read alignment, Bowtie2 is a fast efficient tool that align short DNA/ RNA sequence reads to a reference genome. It helps in identifying the exact position of each read in the genome, which is essential for gene expression analysis.
Input files 
Bowtie2 takes the following inputs
• Trimmed FASTQ files ( clean reads from FastQC/ Trimmomatic)
•	Tumor and normal samples FASTQ files 
Output files 
Bowtie2 generates:: 
• SAM/ BAM/ files ( Alignment files)
• Contains mapped reads with their genomics positions 
• Alignment summer report 
° shows how many reads were aligned successfully 
° Shows overall alignment percentage 
Command used for this step:
Bowtie2 -x /home/BIB/Databases/HumanGenome_HG37/hg19_bowtie_index -1 Sample_1p.fastq.gz -2 Sample_2p.fastq.gz -S Sample_genome_aligned.sam -p 4

Step 5
Conversation of SAM to BAM
After read Alignment Using Bowtie2, the output files in SAM format. This files is usually very large and not efficient for storage or further analysis. So, it is converted into BAM formate
Input file
• SAM file
      ° Output from Bowtie2 alignment 
      ° Contains read Alignment information in text format 
What happens in this step means: 
• Converts SAM >>>BAM ( text – binary format)
• Compresses the file ( reduces size)
• Makes the file faster to process 
Output files 
• BAM files
•	Smaller in size 
•	Contains same alignment data as SAM
•	Used for downstream analysis 
Samtools  view -s -b sample_genome_aligned.sam -o sample_genome_aligned.bam
Sorting and Indexing of BAM files 
After converting SAM to BAM , the next step is sorting and Indexing of the BAM file.
This step is important for efficient data handling and analysis.
Step 6: Sorting 
Sorting means arranging the aligned reads in the BAM file based on their genomics positions ( coordinates).
Input file 
• BAM file ( from SAM to BAM conversion)
Output file
• Sorted BAM file 
•	Example: sample_sorted.bam
Command used for this step;
Samtools  sort sample_genome_aligned.bam -o sample_genome_ sorted.bam
Step 7: Indexing 
Index means, Indexing creates a small index file for sorted BAM file.
It allows quick access to specific regions of the genome without reading the entire file.
Input file 
Sorted BAM file 
Output file ( after Indexing)
Index file 
Example: sample_sorted.bam.bai
Command used for this step;
Samtools index sample_genome_sorted. bam
Step 8:
Estimation of Abundance using String Tie
After sorting and Indexing the BAM file, the next step is to estimate how much gene is expressed. This is called gene abundance estimation.
In this step, the tools string Tie is used . String Tie assemble the aligned reads and calculates the expression level of genes and transcripts.
Input files
•	Sorted BAM file 
•	Reference annotation file (GTF/GFF)
•	Contains gene information ( gene location, exon details)
Output files 
•	GTF file
Contains assemble transcripts with expression value
•	Abundance tables ( expression values) 
Shows expression level of each gene/ transcript

Gene Count Matrix using prepDE.py
After estimating gene expression using StringTie, the next step is to create a gene count matrix. This step is very important because the output from StringTie (GTF files) cannot be directly used for differential gene expression analysis. So, it needs to be converted into a proper count table format.
In this step, a Python script called prepDE.py (provided along with StringTie) is used. This Script reads all the StringTie output files and extracts the read count information for each Gene and transcript. Then, it combines the data from all samples into a single matrix, which Can be easily used for further analysis in R.
Input files
• StringTie output GTF files (from all samples)
• Sample list file
      Contains sample names and corresponding GTF file paths
       Helps the script to identify and process each sample correctly

What prepDE.py does?
• Reads all GTF files generated by StringTie
• Extracts gene-level and transcript-level read counts
• Combines data from multiple samples into one table
• Arranges data into matrix format (rows = genes, columns = samples)
• Ensures all samples are aligned based on gene IDs
• Prepares clean and structured data for downstream analysis
• Makes the data compatible for tools like DESeq2
Output Files
• gene_count_matrix.csv
Contains gene-wise read counts for all samples
 Each row represents a gene and each column represents a sample
• transcript_count_matrix.csv
 Contains transcript-wise counts
 Useful for transcript-level analysis
Why it is important?
• Converts StringTie output into a usable count format
• Essential step before differential gene expression analysis
• Helps in comparing gene expression between tumor and normal samples
• Organizes data in a clear and structured way
• Makes it easy to import data into R for DESeq2 analysis
• Ensures accurate and reliable downstream results
Differential Gene Expression Analysis (DESeq2)
After creating the gene count matrix, the next step is to identify genes that are differentially Expressed between tumor and normal samples.
In this step, DESeq2 [6] (R tool) is used. It performs statistical analysis to find significant Genes.
Input Files
• gene_count_matrix.csv
• Sample information (Tumor / Normal)
What DESeq2 does?
• Normalizes read counts
• Compares tumor vs normal samples
• Calculates fold change
• Calculates p-value and adjusted p-value (padj)
• Identifies significant genes
Full Typical DESeq2 Workflow in R — Explained
1.Load Libraries
Library(DESeq2)
Library(ggplot2)
Library(pheatmap)
Explanation:
I loaded required libraries where DESeq2 is used for analysis, ggplot2 for plots, and Pheatmap for heatmap visualization.
2.Load Count Matrix
Counts <- read.csv(“gene_count_matrix.csv”, row.names =1)
Explanation:
I imported the gene count matrix.
Rows represent genes and columns represent samples
3.Sample Information
Sample_info <- data.frame(
 row.names = c(“SRR37164810”, “SRR37164811”, “SRR37164812”, “SRR37164813”, “SRR37164814”, “SRR37164815”,“SRR37164816”“SRR37164817”,“SRR37164818”, “SRR37164819”, “SRR37164820”, “SRR37164822”),
 Condition = c(“diseased”, “diseased”, “normal”, “normal”, “normal”, “ normal”, “ normal”, “ normal”, “ disease”, “ disease”, “disease”, “ disease”,)
)
Explanation:
I assigned samples into two groups:
•Diseased (tumor samples) 
• Control (normal samples) 
4.Create DESeq2 Dataset
dds <- DESeqDataSetFromMatrix(
   countData = counts,
    colData = sample_info,
    design = ~ condition
)
Explanation:
I created a DESeq2 dataset by combining count data and sample information.
The design ~ condition is used to compare diseased vs control.

5.Differential Expression Analysis
dds <- estimateSizeFactors(dds)
dds <- estimateDispersionsGeneEst(dds)
Dispersions(dds) <- mcols(dds)$dispGeneEst
dds <- nbinomWaldTest(dds)
Explanation:
I performed the main analysis steps:

•	Normalization of counts 
•	Estimation of gene-wise variation
•	Statistical testing using Wald test 
This step identifies differentially expressed genes.
6.Extract Results
res <- results(dds)
res_df <- as.data.frame(res)
res_df$gene <- rownames(res_df)
writes.csv(res_df, “DESeq2_results.csv”)
print(head(res, 20))
Explanation:
I extracted results for each gene and saved them as a CSV file.
The output contains:
• log2FoldChange 
• p-value 
•adjusted p-value (padj)
7.rlog Transformation
rid <- rlog(dds, blind = TRUE)
Explanation:
I applied rlog transformation to stabilize variance and reduce noise.
This is used for visualization.
8.PCA Plot
pcaData <- plotPCA(rld, intgroup = “condition”, returnData = TRUE)
percentVar <- round(100 * attr(pcaData, “percentVar”))
ggplot(pcaData, aes(x = PC1, y = PC2, color = condition)) +
 geom_point(size = 4)
Explanation:
I generated PCA plot to visualize clustering of samples and check separation between tumor And normal groups.
9.Volcano Plot
res_df <- res_df[!is.na(res_df$padj), ]
res_df$significance <- “Not Significant”
res_df$significance[res_df$padj < 0.05 & res_df$log2FoldChange > 1] <- “Upregulated”
res_df$significance[res_df$padj < 0.05 & res_df$log2FoldChange < -1] <- “Downregulated”
ggplot(res_df, aes(x = log2FoldChange, y = -log10(padj), colour = significance)) + geom_point()
Explanation:
I created volcano plot to show significant genes based on fold change and p-value.
It highlights upregulated and downregulated genes.
10.Heatmap (Top 50 Genes)
topGenes <- head(order(res$padj), 50)
mat <- assay(rld)[topGenes, ]
mat <- t(scale(t(mat)))
pheatmap(mat)
Explanation:
I selected top 50 significant genes and generated a heatmap to show expression patterns Across samples.
Why it is important?
• Identifies upregulated genes
• Identifies downregulated genes
• Main result of the analysis
• Helps in biological interpretation
Output
The code generated the following outputs:
• One results file (CSV format) containing gene expression results
• PCA plot showing sample clustering
• Volcano plot showing significant genes
• Heatmap showing expression patterns of top 50 genes
Data Visualization (PCA, Volcano, Heatmap)
After identifying differentially expressed genes, the results are visualized using plots for better understanding. These plots help in clearly interpreting gene expression differences between tumor and normal samples.
Input Files
• DESeq2 results
• Normalized expression data
What is done?
• Generate PCA plot to check how samples are grouped
• Generate Volcano plot to identify significant genes
• Generate Heatmap for top 50 differentially expressed genes
• Visualize gene expression patterns across samples
• Compare tumor and normal samples clearly
Output Files
• PCA Plot (PDF)
• Volcano Plot (PDF)
• Heatmap (Top 50 genes)
Why it is important?
• Helps in easy understanding of complex data
• PCA plot shows clustering and separation of samples
• Volcano plot highlights upregulated and downregulated genes
• Heatmap shows expressi
