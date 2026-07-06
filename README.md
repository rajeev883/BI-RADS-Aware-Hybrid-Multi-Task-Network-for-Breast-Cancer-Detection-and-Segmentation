# BI-RADS-Aware Hybrid Multi-Task Network for Breast Cancer Detection and Segmentation

**MTBIRADSNet** — a CNN + Vision Transformer hybrid with a BI-RADS density-aware FiLM prior, trained for joint breast-cancer classification and lesion segmentation on full-field digital mammography (FFDM).

> CNN + Transformer Backbone with Multi-Task Heads and BI-RADS Density-Based Prior Regularization
> Rajeev Ranjan Yadav (22030148) — under the guidance of Dr. Anil Kumar Soni, Guru Ghasidas Vishwavidyalaya (GGU), Bilaspur

---

## 1. Overview

Breast tissue density frequently masks malignant lesions on mammograms. MTBIRADSNet addresses this by combining:
- an **EfficientNet-B4** CNN encoder for local texture,
- a **Vision Transformer** branch for long-range context,
- a **BI-RADS density prior module** (Feature-wise Linear Modulation) that conditions the shared features on the ACR breast-density category,
- a **UNet-style decoder** for pixel-wise lesion segmentation, trained jointly with a classification head.

## 2. Key Results

| Setting | AUC-ROC | Sensitivity | Specificity | Dice | IoU |
|---|---|---|---|---|---|
| Single-split (A5, INbreast) | 0.9040 | 0.9500 | 0.7619 | 0.3628 | 0.3234 |
| 5-fold CV (INbreast, mean ± SD) | 0.8234 ± 0.0338 | 0.7614 ± 0.1258 | 0.6429 ± 0.1570 | 0.2007 ± 0.0431 | 0.1720 ± 0.0320 |
| External — CBIS-DDSM (zero-shot) | 0.5760 | 0.5109 | 0.6449 | 0.0507 | 0.0363 |
| External — CBIS-DDSM (fine-tuned) | 0.7449 | 0.7029 | 0.6542 | 0.3674 | 0.2891 |

Full ablation study, statistical tests, and discussion are in the paper (`docs/`).

## 3. Repository Structure

```
.
├── notebooks/
│   └── Q1PAPER__LATEST_CV_External.ipynb   # end-to-end: data prep, training, ablation, CV, external validation
├── docs/
│   └── Q1_Paper.docx                        # full write-up
├── results/
│   ├── figures/                             # exported PNGs (Fig 1-11 from Appendix B)
│   └── tables/                              # metrics as CSV (per-fold CV, ablation, external val)
├── requirements.txt
├── .gitignore
└── README.md
```

> **Note:** raw datasets, full checkpoints, and bulk experiment outputs are **not** stored in this repository (see Section 5). Only the notebook, curated result tables/figures, and the paper are version-controlled.

## 4. Setup

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
```

The notebook was developed and run on Google Colab (GPU runtime). To reproduce:
1. Open `notebooks/Q1PAPER__LATEST_CV_External.ipynb` in Colab.
2. Mount your Google Drive and update `PROJECT_PATH` in Cell 1 to your own Drive folder.
3. Follow Section 5 below to obtain the datasets.

## 5. Data Access

This project uses two mammography datasets, neither of which is redistributed in this repo due to size and/or licensing:

- **INbreast** (primary training/validation set): request access via the [official INbreast dataset page](http://medicalresearch.inescporto.pt/breastresearch/index.php/Get_INbreast_Database). Place the extracted archive so that `AllDICOMs/`, `AllXML/`, and `INbreast.xls` are discoverable, as expected by Cell 7–9 of the notebook.
- **CBIS-DDSM** (external validation): publicly available from [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/collection/cbis-ddsm/). Used with the official mass/calcification case-description CSVs to build a strict, non-overlapping 2,864/704 train/test split (see paper, Section 10.2).

## 6. Model Checkpoints

Trained weights (`.pth`) are **not** committed to git (GitHub's 100 MB per-file limit and general git-hygiene). Choose one of:
- Upload checkpoints to a [GitHub Release](https://docs.github.com/en/repositories/releasing-projects-on-github) for this repo and link them here, or
- Track large files with [Git LFS](https://git-lfs.com/), or
- Keep them on Google Drive and add a shareable link below.

| Checkpoint | Description | Link |
|---|---|---|
| `best_inbreast.pth` | Best single-split (A5) model | *add your Drive/Release link* |
| `cbis_ddsm_finetuned.pth` | CBIS-DDSM fine-tuned model | *add your Drive/Release link* |

## 7. Citation

If you use this code or build on this work, please cite:

```
@unpublished{yadav_mtbiradsnet,
  author = {Rajeev Ranjan Yadav},
  title  = {BI-RADS-Aware Hybrid Multi-Task Network for Breast Cancer Detection and Segmentation},
  note   = {Under the guidance of Dr. Anil Kumar Soni, Guru Ghasidas Vishwavidyalaya (GGU), Bilaspur},
  year   = {2026}
}
```

## 8. License

Add a `LICENSE` file (e.g., MIT for code) before making the repo public. Note: dataset licenses (INbreast, CBIS-DDSM) are separate and must be respected independently of your code license.
