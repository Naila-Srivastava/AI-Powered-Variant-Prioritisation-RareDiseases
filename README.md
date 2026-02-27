# AI-Powered Variant Prioritisation for Rare Diseases

> *Extending a WES variant calling pipeline with machine learning to intelligently prioritise Variants of Uncertain Significance (VUS) for rare disease diagnostics*.

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.14-blue?style=flat-square&logo=python" />
  <img src="https://img.shields.io/badge/Scikit--learn-ML-orange?style=flat-square" />
  <img src="https://img.shields.io/badge/Data-ClinVar%20GRCh38-green?style=flat-square" />
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square" />
</p>

---

## Project Overview

This project extends the [Variant Calling WES Rare Disease pipeline](https://github.com/Naila-Srivastava/Variant_Calling-WES-RareDisease) by integrating a **Random Forest classifier** to score and rank Variants of Uncertain Significance (VUS) by their likelihood of pathogenicity.
Clinical genomicists face a critical challenge: millions of variants are classified as VUS, yet only a fraction are clinically actionable. 
This AI layer bridges that gap by learning from **4.1 million labelled ClinVar variants** and applying that knowledge to prioritise previously unclassified variants, reducing the diagnostic haystack to a shortlist of high-risk candidates.

---

## Biological Context

Whole Exome Sequencing (WES) generates thousands of variants per patient. Standard annotation tools (e.g., GATK, VEP) can classify variants as Pathogenic, Benign, or VUS, but the VUS category remains clinically unactionable. 
This project uses machine learning to assign a **pathogenicity probability score** to every VUS, enabling clinicians to prioritise which variants to investigate further.

---

## Tools & Technologies

| Category | Tools |
|----------|-------|
| Language | Python 3.14 |
| ML Framework | `Scikit-learn` (Random Forest) |
| Data Processing | `Pandas`, `NumPy` |
| Visualisation | `Matplotlib` |
| Data Source | ClinVar VCF GRCh38 (NCBI) |
| Model Persistence | `Joblib` |

---

## Data Source

* **Database:** [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/)
* **Genome Build:** GRCh38
* **File:** clinvar.vcf.gz
* **Accessed:** February 2026

ClinVar is a freely accessible, public archive of reports of the relationships among human variations and phenotypes, with supporting evidence.

---

## How to Run

1. Clone the repository

  ```bash
  git clone https://github.com/Naila-Srivastava/AI-Powered-Variant-Prioritisation-RareDiseases.git
  cd AI-Powered-Variant-Prioritisation-RareDiseases
  ```
2. Install dependencies

  ```bash
  pip install pandas numpy scikit-learn matplotlib joblib
  ```
3. Download ClinVar data
  
  ```bash
  # Download GRCh38 VCF from NCBI ClinVar
  wget https://ftp.ncbi.nlm.nih.gov/pub/clinvar/vcf_GRCh38/clinvar.vcf.gz -P data/
  ```
4. Run the notebook in `VS Code Studio` or `JupyterLab` and run all cells.

---

## Methodology

```
ClinVar VCF (4.3M variants)
        │
        ▼
Data Parsing & INFO field extraction
        │
        ▼
Label Simplification (Pathogenic / Benign / VUS)
        │
        ▼
Feature Engineering
(REF/ALT length, variant type, chromosome, position)
        │
        ▼
Class Balancing (50K Pathogenic + 50K Benign)
        │
        ▼
Random Forest Classifier (100 estimators)
        │
        ▼
VUS Scoring & Ranking (2.45M variants scored)
        │
        ▼
High-Risk VUS List (439,139 variants, score ≥ 0.7)
```
---

## Model Performance
The model was trained on a balanced dataset of 100,000 variants (50K Pathogenic, 50K Benign) and evaluated on a held-out 20% test set.

**Balanced Classification Report:**

| Class | Precision | Recall | F1-Score |
|-------|-----------|--------|----------|
| Benign | 0.72 | 0.69 | 0.70 |
| Pathogenic | 0.70 | 0.73 | 0.71 |

**ROC-AUC: 0.7921**

**Top Predictive Feature:** Genomic position (POS) reflecting that pathogenic variants cluster in biologically constrained regions, consistent with known hotspot mutation patterns.

---

## Results

| Metric | Value |
|--------|-------|
| Training variants | 1,678,471 |
| VUS scored | 2,455,002 |
| High-risk VUS identified (score ≥ 0.7) | 439,139 |
| ROC-AUC Score | 0.7921 |
| Pathogenic F1-Score | 0.71 |
| Overall Accuracy | 71% |

<img width="1685" height="466" alt="image" src="https://github.com/user-attachments/assets/12abb094-b466-46d8-a663-ee236d1b2756" />


### Top High-Risk Disease Categories Identified
- Inborn Genetic Diseases
- Hereditary Cancer-Predisposing Syndrome
- Cardiovascular Phenotype
- Hereditary Breast/Ovarian Cancer Syndrome
- PTEN Hamartoma Tumor Syndrome
- Neurofibromatosis Type 1
- Ataxia-Telangiectasia Syndrome

<img width="1022" height="542" alt="image" src="https://github.com/user-attachments/assets/af10ee0d-dab6-461a-a63c-b97126d2cb31" />

---

## Key Takeaways

* **Class imbalance is critical—** The initial model had poor Pathogenic recall (F1: 0.50). Balancing the training set to equal Pathogenic/Benign samples improved it dramatically to 0.71, highlighting how real-world genomic datasets require careful handling before ML is applied.
* **Genomic position is the strongest predictor—** The feature importance analysis revealed that chromosomal position (POS) dominates all other features, which is biologically meaningful: pathogenic variants tend to cluster in functionally constrained, evolutionarily conserved regions of the genome.
* **VUS prioritisation has real clinical value—** Out of 2.45 million VUS, the model flagged 439,139 as high-risk (score ≥ 0.7), with the top disease categories including Inborn Genetic Diseases and Hereditary Cancer-Predisposing Syndromes. This kind of shortlisting is exactly what rare disease diagnostic labs need to reduce the burden of manual review.
* **Simple features can go a long way—** This model used only 6 features (variant length, type, chromosome, position) yet achieved a ROC-AUC of 0.79. Adding functional annotation features (e.g., CADD scores, conservation scores, splicing impact) would significantly improve performance in future iterations.
* **Reproducibility matters—** The entire pipeline from raw VCF parsing to model training and VUS scoring is contained in a single notebook, making it fully reproducible and easy to extend.

---

## References

- **Python 3.14:** Python Software Foundation. (2025). Python 3.14.3. [https://www.python.org](https://www.python.org)
- **Visual Studio Code:** Microsoft. (2024). Visual Studio Code (VS Code). [https://code.visualstudio.com](https://code.visualstudio.com)
- **ClinVar:** Landrum MJ, et al. (2018). ClinVar: improving access to variant interpretations and supporting evidence. Nucleic Acids Research, 46(D1), D1062–D1067. [https://www.ncbi.nlm.nih.gov/clinvar/](https://www.ncbi.nlm.nih.gov/clinvar/)
- **Scikit-learn:** Pedregosa F, et al. (2011). Scikit-learn: Machine Learning in Python. Journal of Machine Learning Research, 12, 2825–2830. [https://scikit-learn.org](https://scikit-learn.org)
- **Pandas:** McKinney W. (2010). Data Structures for Statistical Computing in Python. Proceedings of the 9th Python in Science Conference. [https://pandas.pydata.org](https://pandas.pydata.org)
- **Matplotlib:** Hunter JD. (2007). Matplotlib: A 2D Graphics Environment. Computing in Science & Engineering, 9(3), 90–95. [https://matplotlib.org](https://matplotlib.org)

---

## Project Structure

```
variant_project/
├── data                           # ClinVar GRCh38 variant database
│            
├── notebooks/
│   └── 01_data_exploration.ipynb  # Full pipeline notebook
├── models/
│   └── rf_variant_classifier.pkl  # Trained Random Forest model
└── results/
    ├── model_performance.png      # ROC curve, confusion matrix, feature importance
    ├── vus_distribution.png       # VUS score distribution & top diseases
    └── vus_prioritised.csv        # Full ranked VUS output
```

---

## License
This project is open source and available under the Apache License.

---
