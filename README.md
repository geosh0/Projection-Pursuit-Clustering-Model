# RLAC: Random Line Approximation Clustering

## Overview
This repository contains the Python implementation of **Random Line Approximation Clustering (RLAC)**, a hierarchical divisive clustering algorithm designed for high-dimensional data. 

This project was developed as part of a **Bachelor's Thesis**. It implements the methodology proposed in the scientific paper *"RLAC: Random Line Approximation Clustering"* (Barbas, Vrahatis, Tasoulis, 2021) and expands upon it by introducing a suite of Projection Pursuit Indices (PPIs) to improve robustness against non-standard cluster shapes.

## The Problem: High-Dimensional Clustering
Traditional clustering algorithms (like K-Means) struggle in high-dimensional space due to the "Curse of Dimensionality"—distance metrics become meaningless as dimensions increase. Dimensionality reduction techniques like PCA often fail to preserve cluster separability because they maximize *variance*, not *multimodality*.

## The Solution: RLAC
RLAC combines **Random Projections (RP)** with **Projection Pursuit (PP)**. 

Instead of searching for clusters in the original $D$-dimensional space, RLAC:
1.  Projects the data onto a lower $r$-dimensional subspace using a sparse random matrix (Achlioptas matrix).
2.  Treats these random vectors as "lines" of sight through the data.
3.  Analyzes the density of data along these lines to find natural "valleys" (separators) between clusters.

### Key Theoretical Concepts

#### 1. Johnson-Lindenstrauss Lemma
The algorithm relies on the JL Lemma, which states that points in a high-dimensional space can be projected into a lower-dimensional space while approximately preserving Euclidean distances. RLAC calculates the optimal number of random projections ($r$) automatically to ensure statistical validity.

#### 2. The "Depth Ratio" (Original Contribution)
The core innovation of the original paper is the **Depth Ratio** metric. For any projected dimension (line), we calculate the Kernel Density Estimation (KDE). The quality of a split is determined by:

$$ \text{Depth Ratio} = \frac{\text{Density}(Md_1) - \text{Density}(x^*)}{\text{Density}(Md_2)} $$

Where:
*   $x^*$ is the **Global Minimum** (the deepest valley).
*   $Md_1$ and $Md_2$ are the local maxima (peaks) surrounding that valley.
*   This ratio ensures we only split if there is a deep, significant separation between dense regions.

## Thesis Expansion: New Projection Pursuit Indices
While the original paper focused on the Depth Ratio, this thesis expands the model to handle diverse data distributions by implementing additional **Projection Pursuit Indices (PPIs)**.

The user can now select the splitting criterion (`method=...`):

*   **`depth_ratio`**: The original paper's metric. Best for clear, bimodal separations.
*   **`dip`**: **Hartigan's Dip Test**. statistically tests for unimodality. (Robust against noise).
*   **`min_kurt` / `max_kurt`**: Uses **Kurtosis** to find bimodal distributions (min) or outlier-heavy projections (max).
*   **`negentropy`**: Measures deviation from Gaussianity (based on Information Theory).
*   **`skewness`**: Looks for asymmetric distributions.
*   **`fisher`**: Maximizes the Fisher Discriminant Ratio (variance between classes vs. variance within classes).
*   **`friedman_tukey`**: Optimizes for "spread" and "clumpiness" simultaneously.

---
## 📚 Reference
This work is based on the following paper. If you use this code, please cite the original paper: 
Barbas, P., Vrahatis, A. G., & Tasoulis, S. K. (2021). [RLAC: Random Line Approximation Clustering. In 2021 IEEE International Conference on Big Data (Big Data)](https://ieeexplore.ieee.org/document/9671596)
