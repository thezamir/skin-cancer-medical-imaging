# Skin Cancer Medical Imaging

Medical imaging case study based on a Kaggle competition for **multi-class skin cancer classification** under **severe class imbalance**.

> Focus: robust, metric-driven modeling under label scarcity and imbalance — not leaderboard hacking.

---

## 1) Context & Goal

This project tackles a medical imaging classification task with extreme label scarcity:

- 45,000 **unlabeled** images  
- **293 positive** (rare class of interest)  
- 5,000 **negative** samples  

The goal is to build a **reliable classifier** under real-world constraints:
- positives are rare and costly to miss
- limited labeled data
- high risk of overfitting
- evaluation must reflect imbalance (accuracy is not meaningful here)

---

## 2) Data Source (Kaggle)

This work is based on a Kaggle competition dataset.

- Competition link: **https://www.kaggle.com/competitions/dsm-2025/overview**

I include the link for transparency and context. The dataset itself is not stored in this repository.

---

## 3) Key Ideas (What Was Tried and Why)

### 3.1 Initial hypothesis: use unlabeled data to learn better features
Because labeled positives are extremely limited, the first approach was:

1. Use the **unlabeled pool** to learn a stronger feature extractor (backbone)
2. Train the downstream classifier using the labeled set

This aims to reduce overfitting and improve representation quality under scarcity.

**Outcome:** the transfer did not produce the expected gains consistently enough. Improvements were limited and unstable under the imbalance constraints.

### 3.2 Why ViT was not used
Vision Transformers were not a good fit because:
- effective labeled set is small (especially for positives)
- ViTs generally require more labeled data and are less stable in this regime

### 3.3 Shift to classical CNN + heavy augmentation
Given the constraints, the solution moved to a classical CNN approach with:
- **aggressive augmentation** to expand the positive manifold
- imbalance-aware training decisions (sampling / loss / thresholds depending on your implementation)
- evaluation centered on imbalance-relevant metrics

### 3.4 Best result: overlap/ensemble of two CNN models
The most stable performance came from combining predictions of **two independently trained CNN models**, reducing variance and improving robustness on rare classes.

---

## 4) ## Project Notebooks

This repository contains two notebooks, each focusing on a different architectural approach under severe data imbalance:

- **U-CNN / representation learning approach**  
  Explores leveraging 45,000 unlabeled images to train a backbone, followed by supervised fine-tuning on a highly imbalanced labeled set (293 positives vs 5,000 negatives). This notebook documents the motivation, experiments, and limitations of this approach under extreme label scarcity.

- **Classical CNN with augmentation and ensembling**  
  A shift to a classical CNN architectures, 4 models were tested combined with aggressive data augmentation to compensate for class imbalance. The strongest and most stable results were achieved by overlapping (ensembling) the predictions of two independently trained CNN models.



