# Project guide

## 1. Purpose

This repository contains the implementation artefacts and recorded outputs for an MSc dissertation on deep-learning-based 3D brain tumour segmentation. It compares U-Net, Swin UNETR and nnU-Net v2 on the BraTS 2020 dataset.

## 2. Dataset and task

The notebooks obtain the BraTS 2020 training/validation dataset through KaggleHub using the identifier:

```text
awsaf49/brats20-dataset-training-validation
```

Each subject is represented using four MRI modalities:

- FLAIR
- T1
- T1ce
- T2

The segmentation labels in the source dataset are 0 (background), 1, 2 and 4. The implementation converts labels for model training and, in the nnU-Net workflow, maps them to contiguous labels. The evaluated tumour regions are:

- **WT (Whole Tumour):** labels 1, 2 and 4 in the original BraTS label convention.
- **TC (Tumour Core):** labels 1 and 4.
- **ET (Enhancing Tumour):** label 4.

The study implementation selects 100 valid subjects. The recorded experiments cover folds 0 and 1 of a planned five-fold cross-validation setup. Refer to the dissertation for the complete rationale, preprocessing description, split details and study limitations.

## 3. Model implementations

### U-Net

The U-Net implementation uses MONAI's 3D U-Net network. It is contained in the combined U-Net/Swin UNETR notebook and exported Python file.

### Swin UNETR

The Swin UNETR implementation uses MONAI's SwinUNETR network. It shares the combined notebook and data workflow with U-Net. The notebook's `UNET` selector chooses the model path.

### nnU-Net v2

The nnU-Net implementation uses the nnU-Net v2 framework. Its notebook prepares a nnU-Net dataset, converts BraTS labels, configures the nnU-Net environment variables, and uses the framework's planning, preprocessing, training and evaluation workflow.

nnU-Net retains its own automated dataset-adaptive workflow. Its results should therefore be understood as a framework-level comparison with the manually configured MONAI models, not as an architecture-only comparison under identical preprocessing and planning.

## 4. Repository map

| Path | Contents |
|---|---|
| `Models/MSc_Project_UNET_&_Swin_UNETR.ipynb` | Original notebook for U-Net and Swin UNETR. |
| `Models/MSc_Project_nnUNet.ipynb` | Original nnU-Net v2 notebook. |
| `Models/msc_project_unet_swin-unetr.py` | Exported Python representation of the combined notebook. |
| `Models/msc_project_nnunet.py` | Exported Python representation of the nnU-Net notebook. |
| `Models Result/UNet/` | U-Net logs, metrics, fold summary, plots and qualitative outputs. |
| `Models Result/Swin UNETR/` | Swin UNETR logs, metrics, fold summary, plots and qualitative outputs. |
| `Models Result/nnU-Net results/` | nnU-Net metrics, metadata, logs, plots and qualitative outputs. |
| `Models Result/Result explanation.md` | Existing written summary of selected results and source artefacts. |

## 5. How to read the implementation

A practical reading order is:

1. Read the dissertation for the research question, methodology and experimental context (available on demand).
2. Open the relevant notebook in `Models/` to follow the implementation in its original notebook format.
3. Use the exported Python file when you want to search or inspect the code as text.
4. Open the matching model directory under `Models Result/` to inspect its logs, metrics, plots and qualitative examples.
5. Read [RESULTS_GUIDE.md](RESULTS_GUIDE.md) for a navigation guide to the existing result artefacts.

The exported Python files preserve notebook-oriented code. They include Google Colab imports, shell/magic commands and environment-specific paths. They are provided as implementation references, not as a promise of direct execution as ordinary Python scripts.

## 6. Study scope and limitations

The repository reflects the submitted study, including its practical constraints:

- A 100-subject subset was used rather than the full BraTS 2020 training cohort.
- The recorded cross-validation work covers folds 0 and 1, rather than all five planned folds.
- nnU-Net's automated preprocessing and planning differ from the manually configured MONAI pipelines.
- The work focuses on one dataset and one application domain.
- Training and evaluation were shaped by the available compute environment.

These limitations are part of the research record. This guide does not change the experiments or extend their claims.
