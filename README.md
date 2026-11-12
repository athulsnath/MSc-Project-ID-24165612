# MSc data Science and Analytics Final Project

**Student Name:** Athul Sheeja Nath
**Student ID:** 24165612
**Module:** MSc Final Project (7COM1075-0901-2026 / Data Science and Analytics Masters Project)
**Project Title:** Multimodal Deep Learning for Glioma IDH Mutation Classification Using MRI and Clinical Data

---

### Overview
This repository contains the code and experimental pipelines for predicting  IDH gene mutation status (Wildtype  vs Mutant) in glioma using the UCSF-PDGM dataset.

## Research Questions
* **RQ1 (Multimodal Fusion):** To what extent does fusing clinical demographic metadata (specifically age and sex) with spatial features extracted from preoperative MRI sequences improve the diagnostic performance (ROC-AUC, F1-score) of deep learning models in predicting glioma IDH mutation status compared to an MRI-only baseline model?
* **RQ2 (Feature Contribution Analysis):** What is the relative contribution of tabular clinical variables versus image-derived feature representations in predicting IDH mutation status across different glioma WHO grades (Grades 2-4)?

---

### Current Progress
- [x] Data extraction pipeline for 2D axial T1c slices from 3D NIfTI volumes.
- [x] Baseline ResNet18 architecture with dynamic positive class weighting ($pos weight = 3.8611$).
- [x] Validation-based model checkpointing (`best_baseline_resnet18.pt`).
- [x] Test set evaluation establishing baseline performance (Test ROC-AUC: ~0.80-0.84)

### Next Stage (RQ1)
- [ ] Multimodal fusion architecture integrating 2D MRI visual features with tabular clinical metadata (Age, Sex).
- [ ] Dual-branch PyTorch architecture combining ResNet18 spatial embedding with tabular MLP features.
- [ ] Statistical performance evaluation (ROC-AUC, F1-score, Sensitivity, Specificity) vs MRI-only baseline.
