# Malaria Detection from Cell Images Using Traditional Machine Learning

## 📌 Project Overview

This project explores the automatic classification of microscopic cell images into **Parasitized** and **Uninfected** classes using traditional machine learning algorithms and handcrafted texture descriptors.

Instead of relying on heavy deep learning architectures, this study evaluates how effectively high-dimensional visual features—specifically **Histogram of Oriented Gradients (HOG)** and **Local Binary Patterns (LBP)**—can capture morphological structural changes and micro-textures associated with malaria infection.

---

## ⚙️ Methodology & Pipeline

The project follows a 6-stage computer vision and machine learning workflow:

1. **Dataset Acquisition:** Utilizes the Kaggle *Cell Images for Detecting Malaria* dataset comprising **27,558 single-cell images** (13,779 Parasitized and 13,779 Uninfected).

2. **Preprocessing:**
   - **Resizing:** Images resized to $64 \times 64$ pixels.
   - **Grayscale Conversion:** Strips color noise to focus on texture and boundaries.
   - **CLAHE:** Applied Contrast Limited Adaptive Histogram Equalization for local contrast enhancement.
   - **Gaussian Blur:** Noise smoothing using a 2D Gaussian kernel:
     $$G(x,y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}}$$
   - **Otsu Thresholding:** Isolates the foreground cell from the background to dark-mask non-cellular pixels.

3. **Feature Extraction:**
   - **HOG:** Captures edge orientation and shape contours ($324$ descriptors).
   - **LBP:** Encodes fine local intensity variation and texture patterns ($1,450$ descriptors).
   - **Combined Vector:** $1,774$ descriptors per image.

4. **Feature Selection:**
   - Applied **ANOVA F-test (`f_classif`)** via `SelectKBest` on the training set.
   - Reduced feature dimensionality by **83.1%**, keeping the top **300 features** to prevent overfitting and boost computation speed.

5. **Model Training:** Evaluated four distinct algorithms:
   - **Logistic Regression** (Supervised Linear Classifier)
   - **Random Forest** (Supervised Ensemble)
   - **K-Nearest Neighbors (KNN)** (Supervised Distance-based)
   - **K-Means Clustering** (Unsupervised + Hungarian matching for evaluation)

6. **Evaluation:** Evaluated on a held-out stratified test set across Accuracy, Precision, Recall/Sensitivity, Specificity, F1-Score, and Inference Time per image.

---

## 📊 Experimental Results

| Rank | Model | Accuracy | Precision | Recall (Sensitivity) | Specificity | F1 Score | Inference Time / Image |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| 🥇 | **Logistic Regression** | **82.71%** | **83.96%** | **80.88%** | **84.54%** | **82.39%** | **~0.010 ms** |
| 🥈 | **Random Forest** | 81.28% | 80.55% | 82.48% | 80.08% | 81.50% | ~0.035 ms |
| 🥉 | **KNN** | 66.17% | 83.67% | 40.17% | 92.16% | 54.28% | ~1.303 ms |
| 4 | **K-Means** | 62.19% | 68.79% | 44.63% | 79.75% | 54.14% | ~0.002 ms |

### Key Takeaways:
- **Logistic Regression** delivered the best overall balance between classification accuracy ($82.71\%$) and rapid execution speed ($0.010\text{ ms/image}$).
- **Random Forest** achieved the highest recall ($82.48\%$), making it effective at minimizing dangerous false-negative predictions (missed infections).
- **KNN & K-Means** failed to perform adequately, suffering from high false-negative rates on distance-based metrics in this feature space.
- **Robustness Check:** Reducing training set size from 80% to 50% only led to a slight performance drop ($82.71\% \rightarrow 81.23\%$), confirming model stability.

---

## ⚠️ Limitations & Future Work

- **Handcrafted Descriptors:** Traditional features require manual hyperparameter selection and lack the adaptive feature-learning capabilities of deep neural networks.
- **Resolution Constraints:** Resizing to $64 \times 64$ may obscure very small intracellular parasites.
- **Next Steps:**
  1. Perform $k$-fold cross-validation and validate on independent external laboratory datasets.
  2. Benchmark against standard deep learning architectures (e.g., ResNet, MobileNet).
  3. Explore advanced feature fusion and selection techniques.

> **Disclaimer:** *This project is designed as a lightweight benchmark for educational and research purposes. It is not intended for clinical or diagnostic use without extensive medical validation.*

---

## 👥 Authors

- **Dhvan Shah**
- **Khush Chauhan**

*B.Tech (ENTC) | Machine Learning Project*