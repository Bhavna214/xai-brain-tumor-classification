# Methodology

## Overview

This project proposes a SHAP-guided Explainable AI (XAI) framework for brain tumor MRI classification using transfer learning and dynamic weight adjustment. The methodology combines deep learning-based image classification with SHAP-based interpretability analysis to improve both predictive performance and model transparency.

The overall workflow consists of:
1. Dataset preparation and preprocessing
2. Transfer learning-based model training
3. Misclassification analysis
4. SHAP-based attribution analysis
5. SHAP-guided sample weight adjustment
6. Iterative model refinement and evaluation

---

# 1. Dataset Preparation

The dataset used in this project is a combination of multiple publicly available MRI brain tumor datasets, including:

- Figshare Brain Tumor Dataset
- Sartaj Brain MRI Dataset
- BR35H Dataset

The combined dataset contains 7,023 MRI images categorized into four classes:

- Glioma
- Meningioma
- Pituitary Tumor
- No Tumor

The dataset was divided into:
- Training Set: 5,618 images
- Testing Set: 1,405 images

---

# 2. Data Preprocessing

MRI images collected from multiple datasets varied in dimensions and formatting. Therefore, preprocessing was performed to standardize the data before training.

The preprocessing pipeline included:

- Resizing all images to 224 × 224 resolution
- Converting images into array format
- Label extraction from image paths
- Label encoding of tumor categories into numerical values
- Dataset normalization for improved model training

The preprocessing step ensured consistency across all MRI images and improved compatibility with the transfer learning architecture.

---

# 3. Transfer Learning Model

The classification framework uses ResNet50 as the base transfer learning architecture.

ResNet50 is a pre-trained Convolutional Neural Network (CNN) model widely used in image classification tasks due to its strong feature extraction capabilities and deep residual learning structure.

## Model Architecture

The following layers were added on top of the ResNet50 base model:

- Global Average Pooling Layer
- Dense Layer with ReLU activation
- Dropout Layer for regularization
- Final Dense Layer with Softmax activation for multi-class classification

The model was compiled using:

- Optimizer: Adam
- Loss Function: Sparse Categorical Crossentropy
- Evaluation Metric: Accuracy

Training was performed for 20 epochs using GPU acceleration in the Google Colab environment.

The baseline transfer learning model achieved an accuracy of approximately 95.44% on the testing dataset.

---

# 4. Misclassification Analysis

After training the baseline model, predictions were generated on the testing dataset.

Misclassified images were identified by comparing:
- Actual labels
- Predicted labels

The indices of incorrectly classified images were stored for further analysis.

The following metrics were monitored:
- Number of misclassified images
- Accuracy improvement across iterations

These values were later visualized using performance graphs and confusion matrices.

---

# 5. SHAP-Based Explainability Analysis

To improve interpretability and transparency, SHAP (SHapley Additive exPlanations) was integrated into the framework.

SHAP is an Explainable AI technique that assigns importance values to input features based on their contribution toward model predictions.

For image classification tasks, SHAP helps identify:
- which image regions influence predictions
- which pixels contribute positively or negatively toward a predicted class

## SHAP Masker

A SHAP masker was defined to analyze the influence of different image regions.

The masker selectively hides portions of MRI images and evaluates how prediction confidence changes, thereby identifying important regions influencing the model's decision-making process.

---

# 6. SHAP-Guided Weight Adjustment

The primary innovation of this work lies in the integration of SHAP values with transfer learning to dynamically adjust sample weights during retraining.

SHAP values were computed for misclassified images using the trained transfer learning model.

The SHAP library returned attribution maps representing:
- pixel contribution values
- base SHAP values
- class-specific feature importance

Positive SHAP values represented pixels contributing strongly toward the predicted class, while negative SHAP values represented inverse contributions.

## Weight Adjustment Strategy

For incorrectly classified images:

1. Pixels contributing toward incorrect predictions were identified
2. The difference between SHAP values and base SHAP values was calculated
3. The mean difference was computed as a weight adjustment factor
4. Misclassified samples were assigned modified sample weights

This approach encouraged the model to focus more effectively on diagnostically relevant MRI regions.

---

# 7. Custom Weighted Loss Function

A custom weighted loss function was introduced during retraining.

The weighted loss was computed as:

Modified Sample Weights × Sparse Categorical Crossentropy Loss

This strategy increased the influence of difficult or incorrectly classified samples during optimization.

The modified model was retrained iteratively using:
- updated sample weights
- misclassified image subsets
- SHAP-guided attribution feedback

---

# 8. Iterative Model Refinement

The retraining process was repeated iteratively until improved performance was achieved.

During each iteration:
- SHAP values were recalculated
- sample weights were updated
- the model was retrained
- performance metrics were evaluated

The SHAP-guided retraining strategy significantly reduced the number of misclassified samples and improved classification performance.

The refined model achieved approximately 99.22% accuracy after the first iteration.

---

# 9. Evaluation Metrics

The framework was evaluated using:
- Accuracy
- Confusion Matrix
- Misclassification Trends
- SHAP Attribution Analysis

Visualization plots were generated to analyze:
- accuracy improvement
- reduction in misclassified samples
- model behavior changes across iterations

---

# 10. Verification Analysis

Verification experiments were conducted to analyze how SHAP-guided retraining altered model behavior.

Initially, certain MRI images were incorrectly classified due to the model focusing on irrelevant image regions.

SHAP image plots revealed the regions contributing toward incorrect predictions.

After retraining using SHAP-guided weight adjustment:
- the same MRI images were correctly classified
- attribution maps demonstrated a shift in model attention
- previously dominant regions reduced in importance
- clinically relevant regions gained stronger influence

The reversal in SHAP attribution patterns demonstrated improved interpretability and better feature learning behavior.

---

# Conclusion

The proposed framework demonstrates an effective integration of:
- Transfer Learning
- Explainable AI
- SHAP-based attribution analysis
- Dynamic weight adjustment

for improving both classification accuracy and interpretability in brain tumor MRI classification systems.

The methodology contributes toward the development of more transparent, trustworthy, and clinically interpretable AI-assisted diagnostic frameworks.
