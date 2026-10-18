# 🔬 CLAM-MAE: Comparative Histopathology Image Classification Across Multiple Magnifications

A computational pathology and deep learning project that investigates how image magnification affects lung cancer histopathology classification. The project combines **Masked Autoencoder (MAE) feature extraction, multi-scale patch augmentation, and Multiple Instance Learning (MIL)** to compare classification performance across 5×, 10×, 20×, and 40× magnifications.

Built around the CLAM (Clustering-constrained Attention Multiple Instance Learning) workflow, the notebook implements whole-slide image processing, patch extraction, feature generation, model training, performance evaluation, and comparative visualization.

The primary objective is to investigate the relationship between magnification, computational efficiency, and classification performance when distinguishing two lung cancer subtypes: **Lung Adenocarcinoma (LUAD)** and **Lung Squamous Cell Carcinoma (LUSC)**.

---

## ✨ Features

- 🔬 Whole-slide histopathology image processing
- 🖼️ Support for SVS slide files
- ✂️ Patch extraction at 5×, 10×, 20×, and 40× magnifications
- 🧠 MAE-based feature extraction using a Vision Transformer
- 🔄 Multi-scale patch augmentation
- 📊 768-dimensional image feature representations
- 🧬 LUAD vs. LUSC classification experiments
- 🧠 Multiple Instance Learning (MIL) classifier
- 📈 Accuracy and ROC-AUC evaluation
- 🛑 Early stopping during training
- 💾 Model checkpoint and evaluation-result saving
- 📊 Comparative performance tables and heatmaps
- 🔍 t-SNE visualization of extracted feature embeddings
- ⚡ Computational efficiency analysis across magnifications

---

## 🎯 Project Objective

Histopathology images contain important morphological information that helps distinguish different types of cancer. However, the level of magnification can affect the features captured by a model, the number of image patches generated, computational requirements, and classification performance.

This project investigates the following questions:

- Which magnification provides the best classification performance?
- How does increasing magnification affect the number of image patches?
- Can MAE-based feature extraction improve the representation of histopathology images?
- How effectively can MIL aggregate patch-level features for classification?
- What trade-offs exist between magnification, processing speed, and predictive performance?
- How do extracted feature representations differ across magnifications?

---

## 🏗️ Project Architecture

```text
Whole-Slide Images (SVS)
          │
          ▼
   Slide Loading & Processing
          │
          ▼
  Patch Extraction at Multiple
      Magnification Levels
          │
          ├── 5×
          ├── 10×
          ├── 20×
          └── 40×
          │
          ▼
  Multi-Scale Patch Augmentation
          │
          ▼
  MAE / Vision Transformer
     Feature Extraction
          │
          ▼
  768-Dimensional Features
          │
          ▼
   Multiple Instance Learning
       (MIL Classifier)
          │
          ▼
    LUAD vs. LUSC Prediction
          │
          ▼
 Comparative Evaluation & t-SNE
```

---

## 📊 Dataset

The notebook processes whole-slide histopathology images and organizes samples into two lung cancer categories.

| Class | Description |
|---|---|
| LUAD | Lung Adenocarcinoma |
| LUSC | Lung Squamous Cell Carcinoma |

### Image magnifications

- **5×:** Lower magnification for broader tissue context
- **10×:** Intermediate magnification
- **20×:** Higher-detail tissue representation
- **40×:** Highest magnification evaluated in the notebook

The notebook uses SVS slide files, extracts image patches, and stores patch coordinates and features for subsequent processing.

**Note:** The notebook processes six slides successfully in its patch-extraction runs. This is a limited slide-level dataset, so the results should be treated as experimental rather than evidence of clinical performance.

---

## 🧠 Methodology

### 1. Environment Setup

Prepare the Google Colab environment, install required libraries, configure the CLAM project directory, and download or access the required slide files.

### 2. Whole-Slide Image Processing

Load SVS files and handle image-reading issues. The notebook also includes a custom JPEG extraction and reader component for problematic TIFF data.

### 3. Patch Extraction

Extract tissue patches at four magnification levels:

- 5×
- 10×
- 20×
- 40×

Patch coordinates and extraction summaries are saved for subsequent feature extraction.

### 4. MAE Feature Extraction

Use a Vision Transformer MAE model to transform image patches into numerical feature vectors.

The notebook uses:

- A ViT-Base MAE model
- 768-dimensional feature representations
- Multiple patch sizes: 64, 96, 128, 160, and 200
- An overlap ratio of 0.5

### 5. MIL Classification

Feed the extracted features into an MIL-based classifier to predict whether the tissue belongs to LUAD or LUSC.

The notebook uses slide-based training and validation splits and includes early stopping and checkpoint saving.

### 6. Comparative Analysis

Compare results across all four magnifications using:

- Validation accuracy
- Test accuracy
- ROC-AUC
- Patch counts
- Processing speed
- Training behavior
- t-SNE feature visualizations

---

## 📈 Experimental Results

The notebook reports the following results from its enhanced MAE+MIL experiments.

| Magnification | Validation Accuracy | Test Accuracy | ROC-AUC | Patches Evaluated |
|---|---:|---:|---:|---:|
| 5× | 66.7% | 33.3% | 0.25 | 4,576 |
| 10× | 33.3% | 33.3% | 0.25 | 2,736 |
| 20× | 33.3% | 33.3% | 0.25 | 68,020 |
| 40× | 100.0%* | 0.0% | 0.00 | 170,640 |

*The 40× validation result is misleading: the model achieves 100% training accuracy in the reported logs but 0% test accuracy, indicating severe overfitting or a split/label-handling problem.*

### Interpretation

- **5×:** Best overall option according to the notebook's comparative summary, with 33.3% test accuracy.
- **10×:** Similar test accuracy, but lower validation accuracy.
- **20×:** Processes substantially more patches without improving reported test performance.
- **40×:** Exhibits a major validation-to-test discrepancy and does not generalize in the reported evaluation.

Although the notebook identifies 5× as the preferred magnification, **the reported test performance is poor across all four settings**. These experiments do not establish that the model can reliably classify lung cancer subtypes.

The notebook also reports ROC-AUC values of 0.25 for 5×, 10×, and 20× and 0.00 for 40×. These values warrant further investigation of label encoding, prediction probabilities, and evaluation logic.

---

## 📊 Visualization & Feature Analysis

The notebook includes several comparative visualizations.

### Performance Comparison

Compares validation accuracy, test accuracy, and ROC-AUC across the four magnification settings.

### Performance Heatmap

Provides a visual comparison of experimental metrics across magnifications.

### t-SNE Feature Visualization

Uses t-SNE to project high-dimensional MAE feature vectors into a lower-dimensional space for visual inspection of feature distributions.

The notebook loads 768-dimensional feature vectors and subsamples them for visualization.

### Training Curves and Confusion Matrices

Saves training curves and confusion matrices for the individual magnification experiments.

---

## 🛠️ Tech Stack

### Deep Learning
- Python
- PyTorch
- Torchvision
- Vision Transformer (ViT)
- Masked Autoencoder (MAE)
- Multiple Instance Learning (MIL)
- CLAM workflow

### Image Processing
- OpenCV
- Pillow
- NumPy
- h5py

### Data Analysis
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn

### Environment
- Google Colab
- CUDA-enabled GPU
- Jupyter Notebook

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/humairyousaf6/CLAM-MAE-Magnification-Analysis.git
cd CLAM-MAE-Magnification-Analysis
```

### 2. Open the Notebook

Open:

`CLAM_Project(MAE)_Magnification_Comparative_Analysis.ipynb`

You can run it in Google Colab or a compatible Jupyter environment with GPU support.

### 3. Prepare the Data

Provide access to the required SVS slide files and update the notebook's dataset paths.

The notebook expects a CLAM project directory containing slide data and generated results.

### 4. Install Dependencies

The notebook contains environment setup and installation cells. Review and execute these cells in the target environment before running the processing pipeline.

Typical dependencies include:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn h5py opencv-python pillow tqdm
```

PyTorch, Torchvision, and the required CLAM/MAE dependencies must also be installed in compatible versions.

### 5. Run the Pipeline

Execute the notebook sequentially:

1. Environment setup
2. Slide loading and image processing
3. Patch extraction
4. MAE feature extraction
5. MIL training
6. Model evaluation
7. Magnification comparison
8. t-SNE feature visualization

---

## 📁 Project Structure

```text
CLAM-MAE-Magnification-Analysis/
│
├── CLAM_Project(MAE)_Magnification_Comparative_Analysis.ipynb
│
├── results/
│   ├── patches/
│   ├── coords/
│   ├── features/
│   ├── enhanced_clam_results/
│   ├── patch_extraction_summary_*.json
│   └── clam_magnification_results.csv
│
└── README.md
```

The `results/` directory represents the generated outputs and expected organization of the pipeline; these files may not all be included in the repository.

---

## 🔮 Future Improvements

- Increase the number of independent whole-slide samples.
- Correct and validate slide-level labels and train/validation/test splits.
- Use a held-out test set with no overlap at the patient level.
- Investigate the low test accuracy and ROC-AUC results.
- Evaluate balanced accuracy, precision, recall, F1-score, and per-class ROC-AUC.
- Compare MAE features against pretrained histopathology encoders.
- Implement and verify a standard attention-based CLAM model if CLAM is the intended method.
- Tune MIL pooling, class balancing, and regularization.
- Evaluate stain normalization and tissue-quality filtering.
- Compare computational cost and predictive performance using consistent evaluation procedures.
- Add reproducible experiment configurations and seed control.

---

## ⚠️ Limitations

This is an experimental research notebook, not a clinically validated diagnostic system.

The dataset contains a small number of whole-slide samples, and the reported test metrics indicate that the current classifier is not reliable. Higher validation accuracy, especially the 40× result, should not be interpreted as successful generalization.

The notebook's reported performance and data-splitting logic should be audited before drawing scientific conclusions or making clinical claims.

---

## 👨‍💻 Author

**Muhammad Humair**

AI/ML & Automation Engineer

- Python
- Deep Learning
- Computer Vision
- Computational Pathology
- PyTorch
- Feature Extraction
- Multiple Instance Learning
- Data Analysis

---

## ⭐ Project Highlights

- 🔬 Histopathology image analysis
- 🧠 MAE-based feature extraction
- 🧩 Multiple Instance Learning
- 🔍 Four magnification levels
- 🧬 LUAD vs. LUSC classification
- 📊 Comparative model evaluation
- 📈 t-SNE feature visualization
- ⚡ GPU-enabled processing
- 🧪 Experimental computational pathology research
