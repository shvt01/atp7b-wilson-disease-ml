# Machine Learning Pipeline for Wilson Disease (ATP7B) Protein Classification

A bioinformatics pipeline that classifies proteins as Wilson Disease–associated (ATP7B-like, copper-transporting) candidates from raw protein sequence data, using engineered biochemical features, unsupervised clustering, supervised classification, and explainability methods.

Wilson Disease is caused by mutations in **ATP7B**, a copper-transporting P-type ATPase. Toxic copper accumulation in the liver and brain results when the protein fails to export copper correctly. This project explores whether protein-level sequence and biochemical features alone can distinguish ATP7B-like, copper-transport-associated proteins from other proteins — without relying on prior labels for most of the dataset.

## Pipeline Overview

1. **Sequence loading & cleaning** — parses protein sequences from FASTA, strips invalid characters, retains only valid amino acid codes.
2. **Feature engineering** — computes per-protein biochemical features: amino acid composition, sequence length, molecular weight, aromaticity, instability index, isoelectric point (pI), and GRAVY (hydropathy).
3. **Exploratory data analysis** — distribution plots and a correlation matrix across the engineered features.
4. **Unsupervised clustering** — K-means and hierarchical (Ward linkage) clustering on standardized features, with PCA for dimensionality reduction and visualization.
5. **Pseudo-label construction** — proteins flagged as Wilson-candidates using cluster assignment plus a rule-based filter on length, molecular weight, and cysteine/histidine/methionine content (copper-binding-relevant residues).
6. **Supervised classification** — Random Forest, SVM, k-NN, and Naive Bayes trained to predict the Wilson-candidate label, with class imbalance handled via class weighting and threshold tuning rather than synthetic oversampling. Models are compared on accuracy, precision, recall, F1, and ROC-AUC, with cross-validation for the strongest candidates.
7. **Explainability (SHAP)** — TreeExplainer applied to the Random Forest model to identify which biochemical features drive Wilson-candidate predictions, both globally and for individual proteins.
8. **Motif and structural analysis** — regex-based search for copper-binding motifs (e.g. the CXXC motif) across sequences, with motif density compared between candidate and non-candidate proteins.
9. **Functional validation** — candidate proteins cross-referenced against known Wilson Disease proteins (ATP7B, ATOX1) and validated via UniProt/NCBI Entrez lookups.
10. **GO enrichment analysis** — gene ontology enrichment (via g:Profiler) on the predicted candidate set, testing whether the model's predictions are independently supported by known copper-transport and metal-binding biological functions — a biological sanity check on the ML results, not just a statistical one.

## Key Result

The best-performing models (k-NN and class-weighted Random Forest) achieved near-perfect cross-validated performance in separating Wilson-candidate proteins from the rest of the dataset, and SHAP analysis confirmed that the model's decisions were driven by biologically meaningful features (cysteine content, molecular weight, sequence length) rather than spurious correlations — consistent with ATP7B's known role as a large, cysteine-rich, copper-binding transmembrane protein.

## Repository Structure

```
atp7b-wilson-disease-ml/
├── wilson_disease_protein_classification.ipynb   # full pipeline, step by step
├── requirements.txt
└── README.md
```

## Running It

```bash
pip install -r requirements.txt
jupyter notebook wilson_disease_protein_classification.ipynb
```

The notebook expects a FASTA file of protein sequences as input (see the first code cell) — swap in your own protein set to apply the same pipeline elsewhere.

## Techniques Used

`Python` · `Biopython` · `scikit-learn` · `SHAP` · `UMAP` / `HDBSCAN` · `SciPy` (hierarchical clustering) · `g:Profiler` (GO enrichment) · `pandas` / `NumPy` · `matplotlib` / `seaborn` / `plotly`

## Author

Shaveta Sohpal — [LinkedIn](https://www.linkedin.com/in/shavetasohpal/) · [Portfolio](https://shavetasohpal.netlify.app/) · [Google Scholar](https://scholar.google.com/citations?user=6Gh_k2wAAAAJ)
