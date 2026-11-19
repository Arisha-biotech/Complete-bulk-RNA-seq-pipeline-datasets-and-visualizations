#### Bulk RNA-Seq Normalization, Differential Expression & GSEA Pathway Analysis in IPF (GSE150910)

#### Overview

In this project, I analyzed bulk RNA sequencing (RNA-Seq) data from human lung tissue to find out which genes and biological pathways are turned up in **Idiopathic Pulmonary Fibrosis (IPF)** compared to healthy control lungs. Using Python, Pandas, SciPy, and gseapy, I processed a compressed gene-level count matrix from NCBI GEO (**GSE150910**), normalized expression values across patient samples, isolated **65 strongly upregulated genes**, and explored their biological roles through clustering and pathway enrichment. 

#### Dataset & Tools Used

* **Source:** NCBI Gene Expression Omnibus (GEO Accession: **GSE150910**)
* **Samples:** Human lung tissue samples comparing **IPF** patients against **Healthy Controls**
* **Tools & Libraries:** Python, Pandas, NumPy, SciPy (ks_2samp), Matplotlib, Seaborn, and gseapy (Gene Set Enrichment Analysis)

#### Step-by-Step Workflow

#### Part 1: Normalization & Differential Expression (IPF_Bulk_RNA_Seq_Analysis.ipynb)

1. **Data Loading & Sample Filtering:** Loaded the compressed gene count matrix (GSE150910_gene_level_count_file.csv), extracted sample condition labels (control vs. ipf), and removed unrelated samples.
2. **Library Size Normalization (CPM):** Converted raw read counts into **Counts Per Million (CPM)** so samples with different sequencing depths can be compared fairly.
3. **Low-Expression Filtering:** Kept only genes with an average expression of at least 5 CPM in either the control or ipf group to remove background noise.
4. **Statistical Testing & Fold-Change:** Ran the non-parametric **Kolmogorov-Smirnov test (ks_2samp)** for every gene, applied **Bonferroni correction** (p_value_bonf) to control false positives, and calculated log2 Fold Change (log2_FC).
5. **Isolating Top Upregulated Genes:** Filtered for genes with p_value_bonf <= 0.05 and log2_FC >= 2 (at least a 4-fold increase in IPF), saving **65 significant genes** into upregulated_DEGs.csv (including key fibrosis and airway markers like MUC5B, CXCL13, COL1A1, COL10A1, MMP/proteolysis genes, and SPP1).

#### Part 2: Visualization & GSEA Pathway Enrichment (Visualization & GSEA Notebook)

1. **Biomarker Volcano Plot:** Plotted statistical significance (-log10 adjusted p-value) against biological magnitude (log2_FC) to highlight the 65 upregulated IPF biomarkers.
2. **Gene Correlation Clustermap:** Computed pairwise gene correlations across significance and fold-change metrics and grouped co-regulated genes using hierarchical clustering.
3. **Gene Set Enrichment Analysis (GSEA):** Ranked the upregulated genes by log2_FC and queried GO_Biological_Process_2021 using gseapy.prerank to identify enriched biological processes and plot enrichment mountain curves.

#### Key Results & Visuals

#### 1. Identified Biomarker Signatures (Volcano Plot)

Each red dot represents one of the 65 statistically significant genes upregulated by at least 4-fold (log2_FC >= 2, Bonferroni adjusted p <= 0.05) in IPF lungs compared to healthy controls. 

 

#### 2. Gene Correlation Clustermap

Hierarchical clustering groups the 65 upregulated IPF genes by similarity in their statistical and expression profiles. 

 

#### 3. Top Enriched Biological Pathway (GSEA Mountain Plot)

Pre-ranked Gene Set Enrichment Analysis (GO_Biological_Process_2021) identified **proteolysis (GO:0006508, NES = 1.53)** and **antimicrobial humoral immune response** among the top enriched processes driving tissue remodeling in IPF. 

 

#### Repository Files

* IPF_Bulk_RNA_Seq_Analysis.ipynb — Part 1 notebook: data loading, CPM normalization, KS testing, Bonferroni correction, and DEG export.
* 02_IPF_Visualization_and_GSEA_Pathways.ipynb — Part 2 notebook: Volcano plot, correlation clustermap, and GSEA pathway enrichment.
* upregulated_DEGs.csv — Table of the 65 significantly upregulated IPF genes with raw p-values, Bonferroni adjusted p-values, and log2 Fold Changes.
* ipf_discovered_pathways.csv — Output table from gseapy pathway enrichment analysis.
* ipf_volcano_plot.png — Scatter plot of significant upregulated IPF biomarkers.
* ipf_gene_clustermap.png — Hierarchical correlation heatmap of the 65 upregulated genes.
* ipf_top_enrichment_mountain.png / ipf_top_pathway_mountain.png — GSEA enrichment score curves for the top hit (proteolysis).
* README.md — Project summary, workflow, and visual results.

#### How to Run

1. Open IPF_Bulk_RNA_Seq_Analysis.ipynb in **Google Colab** with the GSE150910 count matrix uploaded to /content/ to generate upregulated_DEGs.csv.
2. Run the second notebook (02_IPF_Visualization_and_GSEA_Pathways.ipynb) in the same folder to reproduce all plots and GSEA pathway tables.
