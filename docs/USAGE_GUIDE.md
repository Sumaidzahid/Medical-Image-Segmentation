# Environment and usage guide

## 1. Important note before running anything

The original implementation was developed in **Google Colab**. The notebooks include Colab-specific imports, shell commands, Drive mounting and paths under `/content/`. The exported Python files retain notebook syntax and should be treated as readable implementation exports, not guaranteed standalone Python programs.

The instructions below describe the original workflow and the dependencies visible in the notebooks. They do not claim that a fresh runtime has been tested or that the original experiment can be reproduced without further environment setup.

## 2. Recommended way to inspect the project

1. Open the repository on GitHub.
2. Navigate to `Models/`.
3. Open the notebook you want to inspect:
   - `MSc_Project_UNET_&_Swin_UNETR.ipynb`
   - `MSc_Project_nnUNet.ipynb`
4. Use GitHub's notebook preview to read cells, or open the notebook in a compatible Jupyter environment for inspection.
5. Browse the corresponding directory in `Models Result/` for the outputs already recorded.

You do not need to run the notebooks to inspect the existing results.

## 3. Original notebook environment

The notebooks use Google Colab and expect access to a suitable Python runtime. GPU availability affects whether 3D training and inference are practical. The original notebooks check CUDA availability and, for the combined notebook, mount Google Drive.

The combined U-Net/Swin UNETR notebook visibly installs or imports packages including:

- PyTorch
- MONAI
- torchmetrics
- nibabel
- NumPy
- Matplotlib
- scikit-learn
- KaggleHub

The nnU-Net notebook visibly installs or imports packages including:

- PyTorch
- nnunetv2
- SimpleITK
- batchgenerators
- nibabel
- NumPy
- Matplotlib
- KaggleHub

These lists are a guide to dependencies referenced by the notebooks, not a complete pinned environment specification. The repository does not provide a version-locked requirements file. Package versions and compatibility may differ from the original runtime.

## 4. Dataset access and preparation

Both notebooks download the dataset through KaggleHub using:

```text
awsaf49/brats20-dataset-training-validation
```

The notebooks expect the BraTS training data to contain subject folders with the four modality volumes and a segmentation volume. The filenames are checked in the notebook code.

Before running the notebooks:

- Ensure the runtime has enough disk space for the dataset and intermediate files.
- Ensure the runtime has sufficient RAM and GPU memory for 3D volumes and model operations.
- Review the notebook's data paths and access requirements for your own runtime.

The study's recorded implementation selected 100 valid subjects. Changing that selection or other experimental settings would change the experiment and is outside the purpose of this repository guide.

## 5. U-Net and Swin UNETR notebook

Open:

`Models/MSc_Project_UNET_&_Swin_UNETR.ipynb`

This notebook contains both MONAI model paths. The `UNET` variable is the model selector in the exported implementation:

- `UNET = True`: U-Net path.
- `UNET = False`: Swin UNETR path.

The notebook includes dataset inspection, subject-list preparation, preprocessing transforms, fold setup, model construction, training, validation and result generation. Read the cells in order because later cells depend on variables and paths created earlier.

The matching exported source is:

`Models/msc_project_unet_swin-unetr.py`

It includes notebook shell commands (for example, lines beginning with `!`) and Colab-specific imports. Do not assume it can be run directly with `python filename.py`.

## 5.1 Class-weight configuration: important distinction

The dissertation specifies different cross-entropy weighting for the two MONAI model paths. This distinction must be preserved when preparing the notebook to run either model.

| Model path | Cross-entropy class weights | Training and validation |
|---|---|---|
| U-Net (`UNET = True`) | **Weighted CE** | The class-weight tensor must be added manually to the U-Net cross-entropy loss configuration, following the dissertation. |
| Swin UNETR (`UNET = False`) | **Unweighted CE** | Do not pass the U-Net class-weight tensor to the Swin UNETR loss in training or validation. |
| nnU-Net v2 | Native nnU-Net loss setup | It does not use the manually calculated U-Net class weights. |

For U-Net, the dissertation describes calculating class weights from class frequency to address imbalance, particularly the smaller Enhancing Tumour (ET) class. The calculated weights are clamped to a maximum of **5** to avoid excessively large values. The weights apply to the **cross-entropy component** of the combined loss; the documented Dice and cross-entropy balance is **0.7 Dice + 0.3 CE**.

For Swin UNETR, use unweighted cross-entropy. The dissertation explains that class weighting was disabled for this model because it hindered rather than improved its capability. This applies to the Swin UNETR training and validation loss path: do not reuse the U-Net weighted-CE configuration.

The dissertation also notes that this difference means the U-Net and Swin UNETR losses are closely matched but not perfectly loss-controlled. It is part of the reported experimental setup, not a setting to standardise or change.

**Important:** The repository notebook may require you to add the U-Net class-weight tensor manually before running the U-Net path. This guide documents that requirement; it does not modify the notebook or supply invented fixed weight values. Use the calculation and clamping procedure described in the dissertation.

## 6. nnU-Net v2 notebook

Open:

`Models/MSc_Project_nnUNet.ipynb`

The notebook configures the nnU-Net environment variables:

- `nnUNet_raw`
- `nnUNet_preprocessed`
- `nnUNet_results`

The original notebook points these variables to directories under `/content/nnUNet/`. It then prepares the BraTS data in nnU-Net format, creates a dataset definition, and follows the nnU-Net planning, preprocessing, training and evaluation workflow.

The exported source is:

`Models/msc_project_nnunet.py`

As with the other export, it contains Colab-specific setup and notebook commands. Use the notebook as the primary representation of the original workflow.

## 7. Checkpoints and existing outputs

The repository contains recorded metrics, logs, plots, summaries and selected qualitative outputs. Trained checkpoint files are not included. Some JSON files contain absolute paths from the original Colab or local Windows environment. Those paths describe where artefacts were stored when the results were generated; they are not valid paths on another machine.

Consequently:

- You can inspect the committed results without running training.
- You cannot load a missing checkpoint from a path recorded in metadata.
- Re-running inference or training may require obtaining the original checkpoint files and reconstructing the relevant environment.
- This repository does not claim that checkpoint reconstruction or full reproduction is currently supported.

## 8. Suggested navigation

| Goal | Location |
|---|---|
| Inspect U-Net implementation | U-Net/Swin UNETR notebook, with `UNET = True` |
| Inspect Swin UNETR implementation | U-Net/Swin UNETR notebook, with `UNET = False` |
| Inspect nnU-Net implementation | nnU-Net notebook |
| Read training history | Relevant model folder's training log |
| Inspect numerical outputs | Relevant model folder's `metrics/` files and fold summary |
| View training/evaluation plots | Relevant model folder's `plots/` directory |
| View selected predictions | Relevant model folder's qualitative images |
| Understand reported values | [Results guide](RESULTS_GUIDE.md) |

## 9. Reproducibility statement

The notebooks document the implementation used for the submitted dissertation. A fresh execution may require changes to package versions, dataset access, paths, hardware allocation or Colab setup. Such changes should be treated as environment adaptation, not as evidence that the original results have been reproduced. The committed result files remain the reference for the results reported in this repository.
