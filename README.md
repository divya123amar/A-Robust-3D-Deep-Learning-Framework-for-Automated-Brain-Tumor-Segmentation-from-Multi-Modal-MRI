# Lightweight 3D U-Net for Multi-Modal MRI-Based Brain Tumor Segmentation


## 📌 Overview

This repository contains the implementation associated with the conference paper:

> **A Robust 3D Deep Learning Framework for Automated Brain Tumor Segmentation from Multi-Modal MRI**

The project presents a lightweight **3D U-Net framework** for automated brain tumor segmentation using multi-modal MRI from the **BraTS2020** dataset.

Four complementary MRI modalities—**FLAIR, T1, T1ce, and T2**—are jointly processed as a four-channel volumetric input. The proposed architecture is designed specifically for experimentation under constrained computational resources.

The study uses approximately **1.4 million trainable parameters** and evaluates multi-class brain tumor segmentation using Dice, IoU, precision, recall, F1-score, and voxel-wise accuracy.

---

## 🧠 Research Objective

Automated brain tumor segmentation from MRI is challenging because tumor regions exhibit substantial variation in morphology, intensity, spatial distribution, and boundary characteristics.

This work investigates whether a relatively lightweight 3D U-Net can learn meaningful volumetric tumor representations when training is performed under limited computational resources.

The study specifically focuses on:

* Multi-modal MRI analysis
* 3D volumetric segmentation
* Lightweight deep-learning architecture
* Computationally constrained training
* Multi-class tumor segmentation
* Class-wise performance evaluation

---

## 🔬 Key Contributions

The main contributions of this work are:

1. A reproducible preprocessing pipeline for multi-modal BraTS2020 MRI volumes.
2. A lightweight 3D U-Net architecture containing approximately **1.4 million trainable parameters**.
3. Four-channel volumetric processing using FLAIR, T1, T1ce, and T2 MRI modalities.
4. A combined **Cross-Entropy + Dice loss** for multi-class segmentation.
5. Quantitative evaluation using Dice, IoU, precision, recall, F1-score, and accuracy.
6. Class-wise analysis of background, necrotic/core, edema, and enhancing tumor regions.

---

## 📊 Dataset

The experiments use the **BraTS2020 training dataset**.

Each patient contains four co-registered MRI modalities:

```text
FLAIR
T1
T1ce
T2
```

The original volumetric data are transformed into:

```text
128 × 128 × 128 × 4
```

The segmentation masks contain four classes:

```text
0 → Background
1 → Necrotic / Core
2 → Edema
3 → Enhancing Tumor
```

The reported experiment uses a **50-patient subset**, divided into:

```text
Training   : 40 patients
Validation : 10 patients
```

The dataset is subject to the applicable BraTS access and usage conditions. Users should obtain the dataset through an authorized source.

---

## ⚙️ Preprocessing

The preprocessing pipeline consists of:

```text
Raw NIfTI MRI Volumes
        ↓
FLAIR / T1 / T1ce / T2
        ↓
Intensity Normalization
        ↓
Resampling
128 × 128 × 128
        ↓
Four-Channel Stacking
        ↓
Mask Label Verification
        ↓
Training / Validation
```

MRI volumes are normalized and resampled to a fixed volumetric resolution. Segmentation masks are resampled using nearest-neighbor interpolation to preserve discrete class labels.

---

## 🏗️ Model Architecture

The proposed model follows a lightweight 3D U-Net encoder–decoder structure:

```text
                 Input
                   │
             4 × 128³
                   │
              Encoder 1
                   │
              Encoder 2
                   │
              Encoder 3
                   │
              Bottleneck
                   │
              Decoder 3
                   │
              Decoder 2
                   │
              Decoder 1
                   │
            1 × 1 × 1 Conv
                   │
                   ▼
          4-Class Segmentation
```

The network uses 3D convolutional operations and skip connections to preserve spatial information throughout volumetric segmentation.

**Trainable parameters:** `1,402,612`

---

## 🧪 Training Configuration

| Parameter            | Value                 |
| -------------------- | --------------------- |
| Input volume         | `128 × 128 × 128 × 4` |
| Number of classes    | 4                     |
| Training patients    | 40                    |
| Validation patients  | 10                    |
| Batch size           | 1                     |
| Optimizer            | Adam                  |
| Learning rate        | `1 × 10⁻⁴`            |
| Loss function        | Cross-Entropy + Dice  |
| Epochs               | 3                     |
| Random seed          | 42                    |
| Compute device       | CPU                   |
| Trainable parameters | 1,402,612             |

These settings correspond to the constrained experimental configuration reported in the paper.

---

## 📈 Experimental Results

The model achieved the following validation performance:

| Metric    |      Score |
| --------- | ---------: |
| Accuracy  | **0.9933** |
| Precision | **0.4707** |
| Recall    | **0.6360** |
| F1-score  | **0.5063** |
| Dice      | **0.5063** |
| IoU       | **0.4174** |

The reported results are based on three training epochs using the 50-patient experimental subset.

### Class-wise Dice

| Class           |  Dice (Mean ± Std.) |
| --------------- | ------------------: |
| Background      | **0.9945 ± 0.0042** |
| Necrotic / Core | **0.2068 ± 0.1085** |
| Edema           | **0.5321 ± 0.1935** |
| Enhancing Tumor | **0.2918 ± 0.1029** |

The substantial difference between background accuracy and tumor-region Dice reflects the strong class imbalance inherent in volumetric brain tumor segmentation.

---

## 📉 Validation Dice Progression

The validation Dice increased during the three training epochs:

```text
Epoch 1 → 0.328
Epoch 2 → 0.384
Epoch 3 → 0.506
```

The continuing improvement suggests that the model had not reached convergence when training was stopped because of the available computational budget.

---

## 📁 Repository Structure

```text
3D-UNet-BraTS2020-Brain-Tumor-Segmentation/
│
├── README.md
├── 3D_UNet_BraTS2020.ipynb
├── requirements.txt
├── LICENSE
│
├── results/
│   ├── figures/
│   ├── metrics/
│   └── predictions/
│
├── paper/
│   └── paper.pdf
│
└── src/
    ├── preprocessing.py
    ├── model.py
    ├── train.py
    └── evaluate.py
```

For the initial GitHub release, the notebook can remain as the primary executable implementation. The code can subsequently be modularized into the `src/` directory.

---

## 💻 Installation

Install the required Python packages:

```bash
pip install torch torchvision torchaudio
pip install numpy scipy nibabel matplotlib tqdm scikit-learn
pip install monai SimpleITK opencv-python torchmetrics scikit-image
```

A `requirements.txt` file is included in the repository.

---

## ▶️ Running the Project

Open the notebook:

```text
3D_UNet_BraTS2020.ipynb
```

The complete workflow follows:

```text
BraTS2020 Dataset
       ↓
Data Loading
       ↓
MRI Preprocessing
       ↓
3D Volume Construction
       ↓
3D U-Net Training
       ↓
Validation
       ↓
Segmentation Prediction
       ↓
Quantitative Evaluation
       ↓
Visualization
```

The notebook is suitable for execution in **Google Colab or a local Jupyter environment**, subject to available computational resources.

---

## 🔐 Security Notice

**Never upload API keys, passwords, access tokens, or other credentials to GitHub.**

The original working notebook contained a hard-coded Kaggle API credential. The GitHub version should use environment variables or another secure authentication mechanism instead.

If a credential has previously been exposed, it should be **revoked and regenerated** before publishing the repository.

---

## ⚠️ Limitations

The reported experiment should be interpreted in light of its computational constraints.

The main limitations are:

* Only 50 patients were used for the reported experiment.
* Training was performed on 40 patients.
* Validation was performed on 10 patients.
* Training was limited to three epochs.
* Experiments were conducted using CPU hardware.
* Minority tumor classes achieved substantially lower Dice scores than the background class.
* Full Hausdorff-distance evaluation was not completed.
* The study does not claim state-of-the-art performance.

The paper explicitly frames the reported results as a resource-constrained proof-of-concept rather than an unconstrained benchmark of 3D U-Net performance.

---

## 🚀 Future Work

Future development will focus on:

* Training using the complete BraTS2020 cohort
* GPU-accelerated training
* Extended training schedules
* Attention-gated 3D U-Net
* Multi-scale feature aggregation
* Data augmentation
* Class-imbalance-aware loss functions
* Hausdorff-distance evaluation
* More extensive statistical validation
* Comparison with established segmentation architectures

These directions follow the limitations identified in the submitted paper.

---

## 📄 Associated Paper



**Title:**

> **A Robust 3D Deep Learning Framework for Automated Brain Tumor Segmentation from Multi-Modal MRI**

**Conference:**



**Authors:**

* Divyashree A


The submitted manuscript identifies the work as a 3D deep-learning framework for automated segmentation using FLAIR, T1, T1ce, and T2 modalities on BraTS2020.

---

## 📚 Citation

If you use this implementation or build upon this work, please cite:

```bibtex
@inproceedings{divyashree2026brain,
  title     = {A Robust 3D Deep Learning Framework for Automated Brain Tumor Segmentation from Multi-Modal MRI},

}


## ⚕️ Disclaimer

This repository is intended exclusively for **research and academic purposes**.

The presented model is **not a clinically validated diagnostic system** and should not be used for clinical diagnosis, treatment decisions, or other medical decision-making.
