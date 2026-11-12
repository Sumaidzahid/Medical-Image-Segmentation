# Results guide

## 1. What is included

The `Models Result/` directory is the repository's record of the model outputs. It contains the existing training logs, numerical metrics, fold summaries, plots and selected qualitative segmentation images for:

- U-Net
- Swin UNETR
- nnU-Net v2

This guide points to those artefacts and transcribes selected values from the committed result files.

## 2. U-Net

**Fold-level best validation Dice** from `Models Result/UNet/kfold_summary_unet.json`:

| Fold | Best epoch | Best validation Dice |
|---:|---:|---:|
| 0 | 44 | 0.7528 |
| 1 | 45 | 0.7634 |

The same summary records regional validation Dice values in WT, TC and ET order:

| Fold | WT | TC | ET |
|---:|---:|---:|---:|
| 0 | 0.8519 | 0.7851 | 0.6213 |
| 1 | 0.8508 | 0.7831 | 0.6564 |

Browse:
- `Models Result/UNet/UNET training log.txt`
- `Models Result/UNet/metrics/`
- `Models Result/UNet/plots/`
- `Models Result/UNet/` qualitative result images

## 3. Swin UNETR

**Fold-level best validation Dice** from `Models Result/Swin UNETR/kfold_summary.json`:

| Fold | Best epoch | Best validation Dice |
|---:|---:|---:|
| 0 | 44 | 0.7858 |
| 1 | 17 | 0.7991 |

The same summary records regional validation Dice values in WT, TC and ET order:

| Fold | WT | TC | ET |
|---:|---:|---:|---:|
| 0 | 0.8052 | 0.8006 | 0.7517 |
| 1 | 0.8683 | 0.7259 | 0.8030 |

Browse:
- `Models Result/Swin UNETR/Swin UNETR training log.txt`
- `Models Result/Swin UNETR/metrics/`
- `Models Result/Swin UNETR/plots/`
- `Models Result/Swin UNETR/` qualitative result images

## 4. nnU-Net v2

The committed nnU-Net summary is:

`Models Result/nnU-Net results/my_all_processed_results/summary.json`

The mean Dice values in that file are:

| Region | Label set in summary | Mean Dice |
|---|---|---:|
| WT | (1, 2, 3) | 0.8989 |
| TC | (2, 3) | 0.8372 |
| ET | (3,) | 0.6970 |
| Foreground | — | 0.8110 |

The nnU-Net inference metadata records folds 0 and 1, configuration `3d_fullres`, plans identifier `nnUNetResEncL_Colab` and trainer `nnUNetTrainer_50epochs`.

Browse:
- `Models Result/nnU-Net results/my_all_processed_results/nnunet_metrics.json`
- `Models Result/nnU-Net results/my_all_processed_results/summary.json`
- `Models Result/nnU-Net results/my_all_processed_results/inference_information.json`
- `Models Result/nnU-Net results/my_all_processed_results/` training logs, plots and qualitative outputs

## 5. Interpreting the results responsibly

The values above come from different result summaries and evaluation workflows:

- U-Net and Swin UNETR values shown above are fold-level validation summaries from their respective JSON files.
- nnU-Net values are the regional means recorded in its processed cross-validation summary.
- nnU-Net uses its own automated planning and preprocessing. It is not an architecture-only controlled comparison with the MONAI implementations.
- The recorded work covers two folds, not the complete five-fold plan.
- Qualitative images are selected examples and should not be interpreted as a quantitative evaluation of every subject.

## 6. Existing result explanation

The repository also includes:

`Models Result/Result explanation.md`

That file gives an existing narrative description of selected training curves, metric plots and prediction examples, and identifies the source artefacts for those observations. It should be read alongside the original images and metric files.
