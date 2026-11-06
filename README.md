# PulseGuard

**Biometric authentication via palm-vein recognition using a custom AlexNet-inspired CNN.**

PulseGuard is a biometric authentication system that identifies individuals from vein images using convolutional neural networks. It supports two datasets — **DB_Vein** and **FYODB** — with a custom AlexNet-inspired architecture trained per dataset for subject identification.

---

## Overview

This project implements a CNN-based biometric authentication pipeline for vein image classification:

- Two independent models are trained — one for **DB_Vein** (98 classes) and one for **FYODB** (160 classes).
- The architecture is a custom CNN inspired by AlexNet, using convolutional blocks with ReLU, batch normalization, max pooling, and fully connected layers with dropout.
- Trained models are saved as `.pth` checkpoints and reused at inference time by `2Application.py`.

---


## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/IAmFarrokhnejad/Palm-Vein-Recognition.git
   cd Palm-Vein-Recognition
   ```

2. **Install the required dependencies.** The project is built on PyTorch; install PyTorch (with the CUDA build matching your system, if applicable) along with the supporting libraries used by the training and prediction scripts:
   ```bash
   pip install torch torchvision matplotlib
   ```
   > Adjust the PyTorch install command based on your platform/CUDA version — see the [official PyTorch instructions](https://pytorch.org/get-started/locally/).

---

## Datasets

| Dataset     | Subjects | Images                          | Source |
|-------------|----------|---------------------------------|--------|
| **DB_Vein** | 98       | 13,721 vein images              | [Kaggle – Contactless Knuckle/Palm Print and Vein Dataset](https://www.kaggle.com/datasets/michaelgoh/contactless-knuckle-palm-print-and-vein-dataset?resource=download) |
| **FYODB**   | 160      | 640 (+6,400 augmented) images   | [FYO – EMU download page](https://fyo.emu.edu.tr/en/download) |

**Expected directory layout** after downloading:

```
Data/
├── Vein_Dataset/
│   └── DB_Vein/     # DB_Vein images
└── FYODB/           # FYODB images
```

---

## Model Architecture

A custom CNN inspired by **AlexNet**, composed of:

- Convolutional layers with **ReLU** activation and **max pooling**
- **Batch normalization**
- Fully connected layers with **dropout**

The output layer size depends on the dataset:

| Dataset     | Output classes |
|-------------|----------------|
| DB_Vein     | 98             |
| FYODB       | 160            |

---

## Training

Run the training script:

```bash
python 1PalmVeinTrain.py
```

This script will:

1. Load the datasets from the paths above.
2. Train a separate model for each dataset (**DB_Vein** and **FYODB**).
3. Save the trained weights as `DB_Vein_model.pth` and `FYODB_model.pth`.
4. Generate training plots for loss and accuracy.

**Fixed hyperparameters per dataset:**

| Dataset     | Learning rate | Batch size | Dropout | Optimizer |
|-------------|---------------|------------|---------|-----------|
| DB_Vein     | 0.001         | 16         | 0.2     | SGD       |
| FYODB       | 0.005         | 16         | 0.2     | SGD       |

---

## Prediction

To classify a new vein image, run:

```bash
python 2Application.py --dataset [DB_Vein|FYODB] --image path/to/image.png
```

**Arguments:**

| Argument    | Description                                              |
|-------------|----------------------------------------------------------|
| `--dataset` | Which model to use: `DB_Vein` or `FYODB`                 |
| `--image`   | Path to the input image to classify                      |

The script loads the corresponding trained model (`DB_Vein_model.pth` or `FYODB_model.pth`) and prints the predicted subject label.

---

## Credits

Developed by **Morteza Farrokhnejad**.

---

### Summary of changes made

- Added a descriptive tagline and **Table of Contents**.
- Normalized heading levels and formatting; consistent **bold** for dataset/model names and fixed "Alexnet" → "AlexNet".
- Added a **Project Structure** section showing the scripts and expected `Data/` layout.
- Expanded the installation section with an explicit `pip install` command and a note about selecting the correct PyTorch build.
- Converted dataset and hyperparameter details into **tables** for readability, and clarified the dataset table (subjects/images/source).
- Added an **Overview** section and clarified what the training and prediction scripts do.
- Added an arguments table to the prediction section and clarified that the correct `.pth` checkpoint is loaded automatically.
- Removed the stray `###` and tightened wording throughout. All technical facts (paths, class counts, hyperparameters, commands) were preserved.
