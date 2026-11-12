# 3D Brain Tumour Segmentation: MSc Dissertation Project

This repository accompanies an MSc dissertation comparing three deep-learning approaches to **3D brain tumour segmentation** using the **BraTS 2020** dataset:

- **U-Net** — convolutional neural network baseline.
- **Swin UNETR** — transformer-based encoder with a U-Net-style segmentation decoder.
- **nnU-Net v2** — self-configuring segmentation framework.

The repository is an archive of the submitted research implementation and its reported outputs. It is intended to make the methodology, implementation notebooks, and existing results accessible.

## Start here

| If you want to… | Go to… |
|---|---|
| Understand the project and its files | [Project guide](docs/PROJECT_GUIDE.md) |
| See the reported metrics, plots and qualitative outputs | [Results guide](docs/RESULTS_GUIDE.md) |
| Understand the original Colab environment and notebook workflow | [Environment and usage guide](docs/USAGE_GUIDE.md) |
| Inspect the original implementation | [Models](Models/) |
| Browse the submitted outputs | [Models Result](Models%20Result/) |

## Research at a glance

The experiments use four MRI modalities: **FLAIR, T1, T1ce and T2**. The segmentation task evaluates three tumour regions:

- **Whole Tumour (WT)**
- **Tumour Core (TC)**
- **Enhancing Tumour (ET)**

The implementation uses a 100-subject subset of the BraTS 2020 training cohort. The recorded experiments cover folds 0 and 1 of a planned five-fold cross-validation setup. The dissertation discusses the dataset, preprocessing, model configurations, evaluation and study limitations in full.

## Models and implementation

| Model | Implementation |
|---|---|
| U-Net | MONAI implementation in the combined U-Net/Swin UNETR notebook. |
| Swin UNETR | MONAI implementation in the same notebook. |
| nnU-Net v2 | Native nnU-Net v2 workflow in the separate nnU-Net notebook. |

For the combined notebook, the `UNET` selector determines which of the U-Net or Swin UNETR model paths is used. The nnU-Net notebook follows its own dataset conversion, planning, preprocessing, training and result workflow. See the [project guide](docs/PROJECT_GUIDE.md) and [usage guide](docs/USAGE_GUIDE.md) before attempting to run the notebooks.

## Results

The repository includes results for the two recorded U-Net and Swin UNETR folds and the recorded nnU-Net cross-validation output. The nnU-Net summary reports mean Dice values of **0.8989 WT**, **0.8372 TC**, **0.6970 ET**, and **0.8110 foreground**. U-Net and Swin UNETR fold-level best validation Dice values are listed in the [results guide](docs/RESULTS_GUIDE.md), alongside links to the source result files.

## Important scope and limitations

- This repository documents the submitted dissertation implementation and its existing outputs.
- The study used 100 subjects, and the recorded cross-validation work covers folds 0 and 1; it is not a completed five-fold evaluation.
- nnU-Net uses its own automated planning and preprocessing workflow. It is therefore a framework-level comparison, not an architecture-only controlled comparison against the manually configured MONAI models.
- The notebooks were developed for Google Colab and contain Colab-specific setup, paths and notebook commands. The exported `.py` files are implementation references and are not guaranteed to run as standalone scripts without adaptation.
- Trained model checkpoint files are not included in the repository. Some result metadata records paths from the original Colab or local environment; those paths are historical references, not portable download locations.


For the full research rationale, methodology, discussion and limitations, consult the dissertation associated with this project (available demand).

## Citation and dataset

The experiments use the BraTS 2020 brain tumour segmentation dataset. The original notebooks refer to the KaggleHub dataset identifier `awsaf49/brats20-dataset-training-validation`.

## Licence

See [LICENSE](LICENSE) for the repository licence.
