# Red Wine Quality Classification

![Language](https://img.shields.io/badge/Language-R-blue)
![License](https://img.shields.io/badge/License-MIT-yellow)

An R script that classifies red wine quality into distinct grades using Support Vector Machine (SVM) models — both linear and radial kernels — trained on physicochemical features from the UCI Wine Quality dataset.

## Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [R Packages](#r-packages)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Limitations](#limitations)
- [License](#license)

## Overview

The script treats wine quality prediction as a multi-class classification problem. Quality scores range from 3 to 8 in the dataset; extreme classes (3 and 8) are collapsed to their nearest neighbours (4 and 7) due to very few samples. Class imbalance across the remaining grades is addressed with upsampling before model training.

## How It Works

1. **Load & preprocess** — reads `dataset.csv`, collapses sparse quality classes (3→4, 8→7), converts quality to a factor, and upsamples minority classes with `caret::upSample`.
2. **Split** — 80/20 train/test split using `createDataPartition` with `set.seed(123)` for reproducibility.
3. **Train SVM Linear** — 10-fold repeated cross-validation (3 repeats) with centre/scale preprocessing.
4. **Train SVM Radial** — same CV setup using the `e1071` radial basis kernel.
5. **Evaluate** — prints confusion matrix and per-class precision, recall, and F1 for each model on the held-out test set.

## R Packages

Install the required packages once in R:

```r
install.packages("caret")
install.packages("e1071")
```

## Prerequisites

- R 4.0 or newer
- RStudio (recommended) or any R environment

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/archiskhuspe/red-wine-quality-classification.git
   cd red-wine-quality-classification
   ```
2. Open `red_wine_quality_classification.R` in RStudio (or run from the terminal).
3. Install packages if not already installed (see [R Packages](#r-packages)).
4. Run the script — results print to the console.

## Dataset

`dataset.csv` is the **UCI Wine Quality dataset (Red Wine)** — a publicly available dataset originally published by:

> P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. *Modeling wine preferences by data mining from physicochemical properties.* Decision Support Systems, 47(4):547–553, 2009.

The dataset contains 1,599 samples with 11 physicochemical input features (fixed acidity, volatile acidity, citric acid, residual sugar, chlorides, free/total sulfur dioxide, density, pH, sulphates, alcohol) and a quality score (3–8) as the target.

## Project Structure

```
red-wine-quality-classification/
├── red_wine_quality_classification.R   # SVM classification script
├── dataset.csv                         # UCI Wine Quality dataset (red wine)
├── LICENSE
└── README.md
```

## Limitations

- **Public dataset** — `dataset.csv` is the UCI Wine Quality dataset; the data was not collected by the author.
- **Class collapsing** — quality scores 3 and 8 are merged into adjacent classes due to insufficient samples, reducing the classification range to grades 4–7.
- **Upsampling introduces duplicates** — `upSample` duplicates existing rows; the model may not generalise as well as if more diverse data were available.
- **Single-digit quantity constraint** — SVM training with repeated CV is computationally expensive; runtime on a standard laptop may be several minutes.
- **No hyperparameter tuning reported** — `tuneLength = 10` explores a grid but the best parameters vary per run; results are not deterministic across R versions or OS environments (beyond `set.seed`).

## License

Released under the [MIT License](LICENSE).
