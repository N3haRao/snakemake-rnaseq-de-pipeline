# 🧬 RNA-seq Differential Expression Analysis

**WT vs KO (Human Samples)**

*Final Project – Comprehensive RNA-seq Workflow*

---

## Project Overview

This project performs a complete RNA-seq analysis workflow comparing:

* **3 Wild-Type (WT)** samples
* **3 Knock-Out (KO)** samples
* Human RNA-seq data

The primary goal is to identify **differentially expressed genes (DEGs)** between WT and KO conditions and interpret their biological significance through functional enrichment analysis.

This repository contains the full analysis workflow including:

* Read-level quality control
* Alignment & count generation
* Differential expression analysis (edgeR)
* Exploratory data analysis (PCA, MDS)
* Visualization (Volcano, Heatmap, log2FC distribution)
* Functional enrichment analysis

---

## Experimental Design

| Condition      | Samples |
| -------------- | ------- |
| Wild-Type (WT) | 3       |
| Knock-Out (KO) | 3       |
| Organism       | Human   |

Comparison performed: **KO vs WT**

---

## Workflow Overview

```
Raw FASTQ
   ↓
FastQC + MultiQC
   ↓
Trimmomatic
   ↓
STAR Alignment
   ↓
SAMtools QC
   ↓
VERSE Count Matrix
   ↓
edgeR Differential Expression
   ↓
PCA / MDS / Visualization
   ↓
Enrichr Functional Enrichment
```

---

## Tools & Software

| Step                    | Tool        | Version |
| ----------------------- | ----------- | ------- |
| Read QC                 | FastQC      | 0.11.9  |
| QC Summary              | MultiQC     | 1.20    |
| Trimming                | Trimmomatic | 0.39    |
| Alignment               | STAR        | 2.7.9a  |
| Alignment QC            | SAMtools    | 1.9     |
| Read Counting           | VERSE       | 2.0.5   |
| Differential Expression | edgeR       | 3.26.0  |
| PCA Visualization       | DESeq2      | 1.30.1  |
| Functional Enrichment   | Enrichr     | 3.0     |

---

## Repository Structure

```

snakemake-rnaseq-de-pipeline/
├── README.md
├── workflow.snake
├── edger-input.snake                

├── envs/
│   ├── base_env.yml           
│   ├── fastqc.yml
│   ├── multiqc.yml
│   ├── trimmomatic.yml
│   ├── star.yml
│   ├── samtools.yml
│   ├── featurecounts.yml
│   ├── deseq2.yml
│   ├── edger.yml
│   ├── enrichr.yml
│   └── gsea.yml

├── scripts/
│   ├── concat_df.py
│   ├── filter_cts_mat.py

├── results/
│   ├── de/
│   │   └── de_results.csv

├── figures/
│   ├── PCA-plot.png
│   ├── mds-plot.png
│   ├── Heatmap.png
│   ├── EdgeR-sample-heatmap.png
│   ├── volcano-plot.png
│   └── log2fc.png

├── reports/
│   ├── final-project-knit.html
│   └── Rao_528_Report.pdf
```

---

## Quality Control

### Read-Level QC

* FastQC used for initial quality assessment
* MultiQC aggregated reports
* Trimmomatic used to remove:

  * Adapter contamination
  * Low-quality bases

### Alignment QC

* STAR alignment to human reference genome
* Evaluated:

  * Mapping rates
  * Duplication rates
  * Coverage statistics

No samples were excluded based on QC metrics.

---

## Exploratory Data Analysis

### PCA Plot

* Performed using DESeq2
* Visualizes global expression structure
* WT and KO samples cluster distinctly

Output: `PCA-plot.png`

---

### MDS Plot

* EdgeR-based sample distance visualization
* Confirms separation between conditions

Output: `mds-plot.png`

---

## Differential Expression Analysis

Differential expression performed using **edgeR**:

* TMM normalization
* Negative binomial model
* Dispersion estimation
* Exact test for KO vs WT

### Thresholds Used:

* FDR < 0.05
* |log2FoldChange| ≥ 1

Results file: `de_results.csv`

---

### Visualizations

| Plot             | Description                   | File                       |
| ---------------- | ----------------------------- | -------------------------- |
| Volcano Plot     | Significance vs fold change   | `volcano-plot.png`         |
| log2FC Histogram | Fold-change distribution      | `log2fc.png`               |
| Heatmap          | Sample clustering by DE genes | `Heatmap.png`              |
| EdgeR Heatmap    | Sample-level clustering       | `EdgeR-sample-heatmap.png` |

---

## Functional Enrichment Analysis

Significant DE genes were analyzed using **Enrichr**.

Databases queried include:

* GO Biological Process
* KEGG Pathways
* Reactome

Results were summarized in the final report (`Rao_528_Report.pdf`).

Enrichment analysis provides insight into biological processes altered in the KO condition.

---

## Final Deliverables

* ✔ QC assessment
* ✔ PCA and MDS plots
* ✔ Differential expression results (CSV)
* ✔ Volcano & heatmap visualizations
* ✔ Functional enrichment analysis
* ✔ Full HTML and PDF report

---

## References

[1] Andrews, S. (2010). FastQC:  A Quality Control Tool for High Throughput Sequence Data [Online]. Available online at: http://www.bioinformatics.babraham.ac.uk/projects/fastqc/

[2] Philip Ewels, Måns Magnusson, Sverker Lundin, Max Käller, MultiQC: summarize analysis results for multiple tools and samples in a single report, Bioinformatics, Volume 32, Issue 19, October 2016, Pages 3047–3048, https://doi.org/10.1093/bioinformatics/btw354

[3] Bolger AM, Lohse M, Usadel B. Trimmomatic: a flexible trimmer for Illumina sequence data. Bioinformatics. 2014 Aug 1;30(15):2114-20. doi: 10.1093/bioinformatics/btu170. Epub 2014 Apr 1. PMID: 24695404; PMCID: PMC4103590.

[4] Dobin A, Davis CA, Schlesinger F, Drenkow J, Zaleski C, Jha S, Batut P, Chaisson M, Gingeras TR. STAR: ultrafast universal RNA-seq aligner. Bioinformatics. 2013 Jan 1;29(1):15-21. doi: 10.1093/bioinformatics/bts635. Epub 2012 Oct 25. PMID: 23104886; PMCID: PMC3530905.

[5] Petr Danecek, James K Bonfield, Jennifer Liddle, John Marshall, Valeriu Ohan, Martin O Pollard, Andrew Whitwham, Thomas Keane, Shane A McCarthy, Robert M Davies, Heng Li, Twelve years of SAMtools and BCFtools, GigaScience, Volume 10, Issue 2, February 2021, giab008, https://doi.org/10.1093/gigascience/giab008

[6] Zhu Q, Fisher SA, Shallcross J, Kim J. VERSE: a versatile and efficient RNA-Seq read counting tool. bioRxiv; 2016. DOI: 10.1101/053306.

[7] Robinson MD, McCarthy DJ, Smyth GK. edgeR: a Bioconductor package for differential expression analysis of digital gene expression data. Bioinformatics. 2010 Jan 1;26(1):139-40. doi: 10.1093/bioinformatics/btp616. Epub 2009 Nov 11. PMID: 19910308; PMCID: PMC2796818.

[8] Chen, E.Y., Tan, C.M., Kou, Y. et al. Enrichr: interactive and collaborative HTML5 gene list enrichment analysis tool. BMC Bioinformatics 14, 128 (2013). https://doi.org/10.1186/1471-2105-14-128
