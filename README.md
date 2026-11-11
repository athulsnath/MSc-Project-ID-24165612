---
license: cc-by-4.0
pretty_name: UCSF-PDGM (BraTS-format structural subset)
task_categories:
- image-segmentation
tags:
- medical-imaging
- mri
- brain
- glioma
- brain-tumor
- brats
- tumor-segmentation
- neuro-oncology
dataset_info:
  features:
  - name: patient_id
    dtype: string
  - name: sex
    dtype: string
  - name: age_at_mri
    dtype: string
  - name: who_cns_grade
    dtype: string
  - name: diagnosis_who2021
    dtype: string
  - name: idh
    dtype: string
  - name: mgmt_status
    dtype: string
  - name: brats21_id
    dtype: string
  - name: os_days
    dtype: string
  - name: dead
    dtype: string
  - name: num_slices
    dtype: int32
  - name: tumor_slice_idx
    dtype: int32
  - name: tumor_voxels
    dtype: int64
  - name: T1
    dtype: image
  - name: T1c
    dtype: image
  - name: T2
    dtype: image
  - name: FLAIR
    dtype: image
  - name: tumor_mask
    dtype: image
  - name: overlay_T1c
    dtype: image
  splits:
  - name: test
    num_bytes: 38063859
    num_examples: 495
  download_size: 38045105
  dataset_size: 38063859
configs:
- config_name: default
  data_files:
  - split: test
    path: data/test-*
---

# UCSF-PDGM — BraTS-format structural subset (T1/T1c/T2/FLAIR + tumor segmentation)

> **⚠️ This repository is the *BraTS-format structural subset* of UCSF-PDGM — NOT the full multi-sequence release.**
> Per patient it provides the **4 structural MRI modalities** (T1, T1c, T2, FLAIR) co-registered to the
> **240 × 240 × 155 BraTS grid** (1 mm isotropic, skull-stripped) plus the **expert-corrected tumor segmentation**.
> The full UCSF-PDGM release additionally contains DWI, ADC, ASL, SWI, HARDI/DTI and derived diffusion maps, N4
> bias-corrected (`_bias`) images, and whole-brain / parenchyma masks — **all intentionally excluded here** to keep
> a compact, BraTS-compatible glioma-segmentation dataset.

The University of California San Francisco Preoperative Diffuse Glioma MRI (UCSF-PDGM) dataset is a cohort of
preoperative 3 T brain MRI from patients with histopathologically confirmed diffuse gliomas.

## Contents

- **495 patients** — one folder each, `UCSF-PDGM-XXXX/`, with **5 NIfTI volumes**.
- `UCSF-PDGM-metadata_v5.csv` — official clinical metadata (sex, age, WHO CNS grade, final WHO-2021 diagnosis,
  MGMT status/index, 1p/19q, IDH, overall survival, extent of resection, and the **BraTS21 ID / cohort columns**).
- `UCSF-PDGM-metadata_glossary.csv` — glossary for the metadata columns.

> Note: the metadata table has **501 rows** (imaging studies) while there are **495 image folders** (unique patients);
> 6 follow-up/duplicate studies are not included as images.

## Per-patient series layout (do not infer — this is the authoritative mapping)

Every `UCSF-PDGM-XXXX/` folder contains exactly these five files:

| File                                   | Series | Description                          | Role        |
|----------------------------------------|--------|--------------------------------------|-------------|
| `UCSF-PDGM-XXXX_T1.nii.gz`             | T1     | Pre-contrast T1-weighted             | image       |
| `UCSF-PDGM-XXXX_T1c.nii.gz`            | T1c    | Post-contrast (gadolinium) T1-w      | image       |
| `UCSF-PDGM-XXXX_T2.nii.gz`             | T2     | T2-weighted                          | image       |
| `UCSF-PDGM-XXXX_FLAIR.nii.gz`          | FLAIR  | T2-FLAIR                             | image       |
| `UCSF-PDGM-XXXX_tumor_segmentation.nii.gz` | seg | Tumor segmentation                  | **ground truth mask** |

All five volumes share identical geometry — shape `(240, 240, 155)`, 1 mm isotropic — and are voxel-aligned.

## Tumor segmentation labels (BraTS convention)

| Label | Region                                            |
|-------|---------------------------------------------------|
| 0     | Background                                         |
| 1     | Non-enhancing tumor core / necrosis (NCR)         |
| 2     | Peritumoral edema (ED)                            |
| 4     | Enhancing tumor (ET)                              |

Masks were produced for the **BraTS 2021** challenge: an automated ensemble of prior BraTS-winning algorithms,
then **manually corrected and approved by neuroradiologists**.

## ⚠️ Benchmark-overlap warning (BraTS 2021 / 2023)

Roughly **298 of the 495 cases (~60 %) overlap with BraTS 2021**, and therefore with **BraTS 2023** (which inherits
the BraTS 2021 glioma data). The `BraTS21 ID`, `BraTS21 Segmentation Cohort`, and `BraTS21 MGMT Cohort` columns in
`UCSF-PDGM-metadata_v5.csv` identify exactly which cases. **Do not treat UCSF-PDGM as an independent benchmark
alongside BraTS2023 without first excluding the intersection** — otherwise evaluation is contaminated by
train/test leakage.

## Joining metadata to images (ID padding differs)

Metadata IDs are **3-digit zero-padded** (`UCSF-PDGM-004`); image folders/filenames are **4-digit**
(`UCSF-PDGM-0004`). Normalize by stripping the `UCSF-PDGM-` prefix and parsing the integer to join the two.

## Preprocessing

- Co-registered (nonlinear) to the T2-FLAIR reference space at **1 mm isotropic**.
- **Skull-stripped** with a deep-learning brain-extraction method.
- Intensities are *not* N4 bias-corrected in this subset (the full release also offers `_bias` variants).

## License

**CC BY 4.0** (Creative Commons Attribution 4.0 International). You must give appropriate credit.

## Citation

Calabrese E, Villanueva-Meyer JE, Rudie JD, et al. *The University of California San Francisco Preoperative
Diffuse Glioma MRI Dataset.* Radiology: Artificial Intelligence. 2022;4(6):e220058.
https://doi.org/10.1148/ryai.220058

Data (TCIA): Calabrese, E., et al. (2022). *UCSF-PDGM* (Version 5) [Data set]. The Cancer Imaging Archive.
https://doi.org/10.7937/tcia.bdgf-8v37 — collection page: https://www.cancerimagingarchive.net/collection/ucsf-pdgm/

## Provenance

Structural subset extracted from **UCSF-PDGM Version 5** (TCIA), retaining only the T1/T1c/T2/FLAIR images and the
`tumor_segmentation` mask from each patient's NIfTI package. Re-distributed here under the source CC BY 4.0 license.
