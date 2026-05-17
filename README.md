# SHAP-Guided Explainable Brain Tumor MRI Classification

An Explainable AI (XAI)-driven deep learning framework for brain tumor MRI classification using transfer learning, SHAP-based interpretability analysis, and dynamic weight adjustment for enhanced model accuracy and transparency.

---

## Overview

Brain tumor diagnosis from MRI scans is a critical task in medical imaging that requires both high predictive accuracy and interpretability. While deep learning models achieve strong classification performance, they often operate as black-box systems, limiting transparency in clinical decision-making.

This project proposes a SHAP-guided transfer learning framework that improves both classification performance and interpretability by dynamically adjusting sample weights based on pixel importance identified through SHAP attribution analysis.

The proposed approach combines transfer learning using ResNet50 with Explainable AI techniques to iteratively refine model behavior and reduce misclassification.

---

## Dataset

The dataset used in this project is a combination of multiple publicly available MRI brain tumor datasets commonly used in medical imaging research, including:

- Figshare Dataset
- Sartaj Dataset
- BR35H Dataset

The combined dataset contains:

- 7,023 MRI brain images
- 4 classification categories:
  - Glioma
  - Meningioma
  - Pituitary Tumor
  - No Tumor

Dataset split:
- Training images: 5,618
- Testing images: 1,405

---

## Objectives

- Develop an accurate brain tumor MRI classification framework
- Improve model interpretability using SHAP-based explanations
- Analyze model misclassification behavior
- Dynamically adjust sample weights using SHAP values
- Enhance diagnostic transparency in AI-assisted healthcare systems

---

## Methodology

### 1. Data Preprocessing

MRI images from multiple datasets were:
- resized to a standardized resolution of 224×224
- normalized
- converted into structured arrays for training

Label encoding was applied to transform categorical tumor labels into numerical format.

---

### 2. Transfer Learning Model

The proposed framework uses ResNet50 as the base transfer learning architecture.

Additional layers added:
- Global Average Pooling Layer
- Dense Layer with ReLU activation
- Dropout Layer for regularization
- Final Softmax Classification Layer

Training configuration:
- Optimizer: Adam
- Loss Function: Sparse Categorical Crossentropy
- Epochs: 20
- GPU acceleration using Google Colab

The baseline transfer learning model achieved:

- Initial Test Accuracy: 95.44%

---

### 3. SHAP-Based Explainability Analysis

To improve interpretability, SHAP (SHapley Additive exPlanations) was used to analyze pixel-level contributions toward model predictions.

A SHAP masker was employed to identify the regions of MRI images contributing most significantly to classification outcomes.

The generated SHAP attribution maps helped:
- identify incorrectly emphasized image regions
- analyze model attention behavior
- detect patterns leading to misclassification

Positive SHAP values indicated pixels contributing toward a predicted class, while negative SHAP values represented inverse contributions.

---

### 4. SHAP-Guided Weight Adjustment (Core Innovation)

The primary contribution of this work is a SHAP-guided retraining strategy.

For misclassified images:
- SHAP values were extracted
- positively contributing pixels leading to incorrect predictions were identified
- weight adjustment values were computed using the difference between SHAP values and base SHAP values

A custom weighted loss function was then introduced:

- Modified Sample Weights × Sparse Categorical Crossentropy Loss

Misclassified images were reintroduced into training with adjusted weights, allowing the model to iteratively refine its attention toward diagnostically relevant MRI regions.

---

## Results

### Baseline Model
- Accuracy: 95.44%

### SHAP-Guided Refined Model
- Accuracy: 99.22%

Key improvements:
- Significant reduction in misclassified samples
- Improved model focus on clinically relevant regions
- Enhanced transparency and interpretability
- Improved prediction reliability

---

## Verification and Interpretability Analysis

Verification experiments were conducted to analyze how SHAP-guided retraining altered model behavior.

### Before Refinement
A misclassified MRI image was incorrectly predicted as a pituitary tumor.

SHAP image plots revealed that the model relied heavily on irrelevant pixel regions, indicated by strong positive attribution values.

### After Refinement
Following SHAP-guided retraining:
- the same MRI image was correctly classified as "No Tumor"
- attribution maps demonstrated a shift in model attention
- previously emphasized regions reduced in importance
- diagnostically relevant regions gained stronger influence

The reversal in SHAP attribution patterns demonstrated that the model successfully learned to focus on more meaningful image features.

---

## Technologies Used

- Python
- TensorFlow / Keras
- ResNet50
- SHAP
- NumPy
- OpenCV
- Matplotlib
- Flask
- Google Colab

---

## Repository Structure

```text
xai-brain-tumor-classification/
│
├── notebooks/
│   ├── brain_tumor_xai.ipynb
│   ├── secondary_use_case.ipynb
│   └── xai_verification_analysis.ipynb
│
├── images/
├── results/
├── docs/
├── demo/
└── README.md
```

---

## Flask-Based Prediction Interface

A lightweight Flask web application was developed to:
- upload MRI images
- generate tumor predictions
- display confidence scores for each tumor category

This interface demonstrates the practical deployment capability of the proposed framework.

---

## Research Contribution

This work demonstrates a novel integration of:
- transfer learning
- Explainable AI
- SHAP-based attribution analysis
- dynamic sample weight adjustment

for improving both performance and interpretability in medical image classification systems.

The proposed SHAP-guided retraining framework contributes toward more transparent and trustworthy AI-assisted diagnostic systems.

---

## Future Scope

- Multimodal medical imaging integration
- Attention-based architectures
- Clinical validation on larger datasets
- Real-time diagnostic deployment
- Integration with physician-assisted workflows

---

## Copyright

This project received a registered copyright for the proposed methodology and implementation.

---

## Author

Bhavna Mulchandani

Research Interests:
- Explainable AI (XAI)
- Medical Image Analysis
- Computer Vision
- Human-Centered AI
- Multimodal AI
