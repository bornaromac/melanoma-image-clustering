# Melanoma Image Clustering and Feature Analysis

## Overview

This project explores **unsupervised learning techniques for analyzing dermoscopic images of benign and malignant skin lesions**. It investigates image preprocessing, pixel-level segmentation, deep feature extraction, dimensionality reduction, and clustering evaluation.

The main objective was to examine whether clustering algorithms can identify meaningful patterns in melanoma images and how preprocessing and feature representation affect the resulting clusters.

This project is intended for educational and experimental analysis. It is **not a clinical diagnostic system**.

## Dataset

The dataset was obtained from Kaggle and contains approximately **13,900 images of benign and malignant skin lesions**.

Due to computational resource limitations, a balanced subset of 400 images was selected for the experiments:

- 200 benign images
- 200 malignant images

A balanced subset was used to explore clustering behavior across the two diagnostic categories.

**Dataset note:** The original project description also reports a training set of 160 images and a test set of 40 images, with 80/80 and 20/20 benign/malignant distributions, respectively. These figures account for 200 images rather than the full 400-image subset. The exact split should be verified before reproducing the experiments.

The dataset's original source and licensing terms should be consulted before redistributing any images.

## Methodology

### 1. Image preprocessing

Preprocessing was performed to prepare the images for segmentation and feature extraction. The main operations included:

- Conversion to grayscale
- K-Means-based pixel segmentation
- Preparation of original and segmented images for comparison

An important challenge was preserving diagnostically relevant visual characteristics. Excessive segmentation can remove potentially useful patterns, so both original and segmented representations were evaluated.

### 2. K-Means segmentation

K-Means was applied to group image pixels according to their similarity. The segmentation experiment used **five pixel clusters** to produce different image regions.

The elbow method and Silhouette Score were considered when assessing the number of clusters. A separate clustering analysis indicated that two clusters provided a more suitable separation under the selected evaluation criterion.

These values refer to different experimental decisions: pixel-level segmentation and subsequent clustering should not be interpreted as the same task.

### 3. DBSCAN clustering

DBSCAN (Density-Based Spatial Clustering of Applications with Noise) was explored as an alternative to K-Means.

Unlike K-Means, DBSCAN does not require the number of clusters to be specified in advance. Its behavior depends primarily on two parameters:

- `eps`: the maximum neighborhood distance between samples
- `min_samples`: the minimum number of neighboring samples required to form a core point

A grid-search approach was used to explore parameter combinations. DBSCAN was investigated for its ability to identify dense regions and potentially irregular patterns, although its results are sensitive to parameter selection and the representation of the input data.

### 4. Deep feature extraction with MobileNetV2

To obtain higher-level visual representations, the project used **MobileNetV2 pretrained on ImageNet** through transfer learning.

Instead of relying exclusively on raw pixel values or manually defined image descriptors, this approach extracts feature vectors from a pretrained convolutional neural network. These vectors can capture visual patterns learned from a large image dataset and serve as input for subsequent clustering.

MobileNetV2 was selected as a relatively computationally efficient feature extractor.

### 5. Dimensionality reduction with PCA

Principal Component Analysis (PCA) was used to reduce the dimensionality of the extracted feature vectors and visualize their structure in two dimensions.

The resulting representation supports qualitative inspection of cluster overlap and separation. However, a two-dimensional PCA projection may not preserve all relationships present in the original feature space.

### 6. Distribution analysis

The distribution of the analyzed data was investigated using:

- Histograms
- Quantile–Quantile (Q–Q) plots
- The Shapiro–Wilk test

The visual analysis suggested that the examined data did not follow a normal distribution. This assessment was considered alongside the statistical test rather than inferred from the histogram alone.

### 7. Clustering evaluation

The experiments used two clustering evaluation metrics:

- **Silhouette Score:** Measures how well samples fit within their assigned clusters compared with neighboring clusters.
- **Purity Score:** Measures the extent to which clusters correspond to the reference benign/malignant labels.

Purity requires reference labels for evaluation, even when the clustering algorithm itself is unsupervised. It is also sensitive to the number of clusters and does not, by itself, establish clinical usefulness.

## Results

### K-Means

| Image representation | Purity Score | Silhouette Score |
|---|---:|---:|
| Segmented images | 0.55 | 0.113 |
| Original images | 0.61 | 0.060 |

**Interpretation:** Segmented images achieved a higher Silhouette Score, suggesting better-defined clusters according to this metric. Original images achieved higher purity, indicating greater agreement between cluster assignments and the reference labels.

The results illustrate that cluster compactness and correspondence with diagnostic labels are different properties. Improving one does not necessarily improve the other.

### DBSCAN

| Metric | Score |
|---|---:|
| Silhouette Score | 0.317 |
| Purity Score | 0.613 |

**Interpretation:** DBSCAN achieved a higher Silhouette Score than the reported K-Means experiments and a purity score of 0.613. However, the clusters did not cleanly separate benign and malignant samples, and samples from different categories remained mixed.

These results suggest that clustering performance depends on the selected representation, algorithm, and evaluation metric. They do not establish that DBSCAN is a reliable method for distinguishing malignant lesions from benign ones.

## Disclaimer

This project is intended for educational and research purposes only. It is not designed or validated for medical diagnosis, treatment decisions, or clinical use.
