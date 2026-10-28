# Vectorized Bulk RNA-Seq Workflow & Functional GSEA for Interstitial Pulmonary Fibrosis (IPF)

## Project Overview
This repository contains a standalone, python-driven dry-lab analysis pipeline built to digest high-throughput transcriptome profiles from NCBI GEO accession GSE150910. The pipeline handles data cleaning, vectorized normalization, non-parametric distribution significance testing, and biological pathway annotation. 

The underlying backend scripts were explicitly engineered to optimize memory utilization within highly constrained mobile computing environments (Infinix hardware runtime utilizing Termux and local Google Colab cloud engines), avoiding desktop-bound GUI dependencies (like Galaxy) via custom Pandas-vectorized matrices.

## Pipeline Architecture & Analytical Benchmarks
1. **Data Ingestion & Cohort Subsetting:** Parsed the raw molecular expression matrix of GSE150910, filtering sample conditions to segment Interstitial Pulmonary Fibrosis (IPF) cases against healthy control groups.
2. **Mathematical Normalization:** Executed Counts Per Million (CPM) adjustments across sample dimensions to account for variance in baseline library sequencing depth.
3. **Statistical Signature Filtering:** Deployed a non-parametric two-sample Kolmogorov-Smirnov test (`scipy.stats.ks_2samp`) on log2-transformed expression vectors with a pseudo-count of 1 to identify gene distribution shifts.
4. **Multiple-Testing Correction:** Applied a stringent Bonferroni correction threshold to control the False Discovery Rate (FDR), filtering out background transcriptional noise and exporting 65 highly validated upregulated Differentially Expressed Genes (DEGs).
5. **Functional Pathway Enrichment:** Ran a pre-ranked Gene Set Enrichment Analysis (GSEA) via `gseapy` against the `GO_Biological_Process_2021` library to map downstream molecular targets.

## Repository Data Matrix Directory
* `upregulated_DEGs.csv`: Master array containing raw p-values, Log2 fold changes, and Bonferroni-adjusted significance fields for identified biomarker signatures.
* `ipf_discovered_pathways.csv`: Output table from the pre-ranked GSEA runtime tracking absolute Normalized Enrichment Scores (NES) across indexed Gene Ontology terms.
* `ipf_volcano_plot.png`: High-resolution visual signature chart displaying biological magnitude shift (Log2FC) against the statistical significance scale (-Log10 Adjusted P-Value).
* `ipf_gene_clustermap.png`: Pearson Correlation Matrix Clustermap exposing hierarchical co-expression clusters across candidate markers.
* `ipf_top_pathway_mountain.png` / `ipf_top_enrichment_mountain.png`: Three-panel GSEA enrichment curve highlighting the leading-edge subset for the top identified biological process: Proteolysis (GO:0006508).

## Environment Setup
To replicate this workflow locally or on a mobile dry-lab system, install the core Python data science dependencies:
```bash
pip install numpy pandas scipy gseapy matplotlib seaborn
```
