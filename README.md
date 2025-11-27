# SG-Net: Hard Spatial Gating for Precision-Driven Brain Metastasis Segmentation

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Framework](https://img.shields.io/badge/PyTorch-1.12-EE4C2C.svg)](https://pytorch.org/)
[![MONAI](https://img.shields.io/badge/MONAI-1.0-blueviolet.svg)](https://monai.io/)
[![Paper Status](https://img.shields.io/badge/Status-Research_Paper-green)](Titlelabel1.pdf)

> **Addressing the "Over-Segmentation Paradox" in Deep Attention Networks for Medical Imaging.**

---

## 📖 Overview

Brain metastasis segmentation is a critical task in neuroradiology, characterized by tiny lesion sizes (median diameter 5-15mm) and extreme class imbalance (tumor voxels < 2% of brain volume).

Existing state-of-the-art models, such as **Attention U-Net**, often suffer from a critical failure mode we term the **"Over-Segmentation Paradox"**: they achieve high sensitivity (Recall > 0.88) but catastrophic precision (Precision < 0.23), leading to massive false positives.

**SG-Net (Spatial Gating Network)** is a precision-first architecture introduced in this repository. Unlike soft-attention mechanisms that assign continuous weights, SG-Net implements **Hard Spatial Gating** to enforce strict binary-like feature selection. This allows for aggressive suppression of background noise while preserving genuine tumor features.

---

## Features & Contributions

* **Lightweight Architecture:** Requires only **0.67M parameters** (8.8x fewer than Attention U-Net), making it suitable for resource-constrained clinical environments.
* **Hard Spatial Gating Module (SGM):** A novel mechanism that partitions input features and applies threshold-based gating to filter out background artifacts effectively.
* **Superior Boundary Precision:** Achieves a **3x improvement** in boundary accuracy (95% Hausdorff Distance: 56.13mm vs. 157.52mm for Attention U-Net).
* **Balanced Performance:** Solves the precision-recall trade-off, achieving a Dice Similarity Coefficient of **0.5578** (State-of-the-Art for this task).


---

## Performance Benchmark

Quantitative evaluation on the independent test set (n=19):

| Model | Dice Score (Mean ± SD) | Precision | Recall (Sensitivity) | HD95 (mm) | Params (M) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Standard U-Net | 0.49 ± 0.16 | 0.42 | 0.73 | 79.57 | 1.98 |
| Attention U-Net | 0.30 ± 0.16 | 0.20 | **0.88** | 157.52 | 5.91 |
| ResU-Net | 0.34 ± 0.17 | 0.23 | 0.87 | 121.97 | 4.81 |
| **SG-Net (Ours)** | **0.56 ± 0.24** | **0.52** | 0.79 | **56.13** | **0.67** |

> *Note: While Attention U-Net has high Recall, its low Precision and high HD95 indicate severe over-segmentation. SG-Net provides the most clinically viable segmentation.*

---

## Dataset

This project utilizes the **Brain-Mets-Lung-MRI-Path-Segs** dataset.

* **Cohort:** 92 patients with brain metastases from lung cancer.
* **Modalities Used:**
    1.  **T1ce:** T1-weighted contrast-enhanced MRI (Active tumor).
    2.  **FLAIR:** Fluid-Attenuated Inversion Recovery (Peritumoral edema).
* **Preprocessing:**
    * Reorientation to RAS coordinates.
    * Resampling to isotropic 1.0mm resolution.
    * Intensity normalization [0, 1] and clipping [0.5, 99.5 percentiles].
    * Patch extraction: `96 × 96 × 32` with 25% overlap.

---

## Tech Stack

* **Language:** Python 3.8+
* **Deep Learning Framework:** PyTorch 1.12
* **Medical Imaging Library:** MONAI 1.0 (Medical Open Network for AI)
* **Hardware:** Optimized for Single NVIDIA T4 GPU training.
* **Loss Function:** Hybrid Loss (Dice Loss + Binary Cross Entropy).

---

## Installation & Setup

Follow these steps to set up the environment and run the model.

### 1. Clone the Repository
```bash
git clone [https://github.com/your-username/SG-Net-Segmentation.git](https://github.com/your-username/SG-Net-Segmentation.git)
cd SG-Net-Segmentation
