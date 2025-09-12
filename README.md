# FFPE-Brain-Tumor-Classification

A machine learning pipeline for classifying **FFPE-derived brain tumor RNA-seq samples** (GSE272042) into molecular subtypes, and evaluating generalization against TCGA glioma cohorts.
Includes **data preprocessing, feature selection, classification, differential gene expression (DGE) analysis, and visualization**.

---

## 📂 Repository Structure

```
FFPE-Brain-Tumor-Classification/
├── notebooks/
│   ├── 01_ffpe_train.ipynb          # Training pipeline on FFPE samples
│   └── 02_tcga_generalization.ipynb # Cross-cohort validation on TCGA
├── scripts/
│   └── download_gse272042.sh        # Script to fetch GSE272042 dataset
├── data/                            # Raw datasets (ignored in GitHub)
├── results/                         # Generated plots and metrics
├── README.md                        # Project documentation
└── .gitignore
```

---

## Data

* **Dataset:** [GSE272042 (NCBI GEO)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE272042)
* **Samples:** 153 FFPE brain tumors
* **Platform:** Illumina NextSeq 550
* **Supplementary File:** `GSE272042_tpmALL.csv.gz` (gene expression TPM matrix)

The dataset is downloaded automatically via:

```bash
bash scripts/download_gse272042.sh
```

---

## Pipeline Overview

1. **Preprocessing**

   * Parse TPM expression matrix
   * Parse sample metadata (Series Matrix file)
   * Filter lowly expressed genes
   * Select top variable genes

2. **Classification**

   * Train **Logistic Regression** with class weights
   * Evaluate on train/val/test split
   * Metrics: F1, balanced accuracy, AUROC

3. **Visualization**

   * PCA and UMAP projections of samples
   * Confusion matrix + reliability diagrams

4. **Differential Gene Expression (DGE)**

   * Contrast analyses:

     * GBM\_IDHwt vs REST
     * ASTRO\_IDHmut vs GBM\_IDHwt
     * ODG\_IDHmut\_1p19q vs GBM\_IDHwt
   * Volcano plots + ranked gene lists (for GSEA)

---

## Key Results

* **Classification metrics (test set):**

  * Macro F1 ≈ 0.51
  * Balanced Accuracy ≈ 0.46
  * AUROC ≈ 0.77

* **Outputs saved in `/results/`:**

  * `confusion_ffpe.png` — Confusion matrix
  * `reliability_ffpe.png` — Reliability calibration plot
  * `pca_train.png` / `umap_train.png` — Dimensionality reduction plots
  * `dge_*.csv` — Differential expression tables
  * `volcano_*.png` — Volcano plots for DGE contrasts
  * `*.rnk` — Ranked gene lists for GSEA

---

## Next Steps

* Extend models to **Random Forests** and **Gradient Boosted Trees**.
* Incorporate **neural networks** for more complex nonlinear patterns.
* Explore **cross-cohort validation** with TCGA glioma expression data.
* Perform **pathway enrichment analysis** with GSEA using `.rnk` files.



