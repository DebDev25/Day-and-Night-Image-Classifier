# Day-and-Night-Image-Classifier
## 1. Problem Statement
Given a cityscape image, the objective is to classify whether the image was taken during the day or at night.

The goal of this project is not only to build a classifier, but to demonstrate feature engineering, validation strategy, model comparison, and error analysis.

---

## 2. Dataset
Dataset source:
https://www.kaggle.com/datasets/heonh0/daynight-cityview

Dataset details:
  - 522 daytime images
  - 227 nighttime images
  - High resolution images (1024×1024 or larger)

Class distribution:
Day: ~70%
Night: ~30%

Stratified splitting was used to preserve this distribution during training and testing.

---

## 3. Feature Engineering
Instead of training directly on raw images, manually global features were extracted:
- avg_hue – Average hue value
- avg_sat – Average saturation
- avg_val – Average brightness (HSV value channel)
- avg_r, avg_g, avg_b – Mean RGB channel values

Images were resized to a common resolution before feature extraction to ensure consistency.

Feature Insights
Boxplot analysis shows:
1. `avg_val` (brightness) provides the strongest separation between classes.
2. RGB channel averages also clearly separate day and night images.
3. Hue and saturation offer weaker but complementary signals.

This strong brightness-based separation explains why linear models perform extremely well on this dataset.

---

## 4. Modeling Approach
Preprocessing
- Numerical features scaled using StandardScaler
- Implemented via ColumnTransformer
- Entire workflow wrapped in Pipeline to prevent data leakage

Models Evaluated (5-fold Stratified Cross-Validation)
1. Logistic Regression
2. Linear SVC
3. K-Nearest Neighbors
4. Random Forest Classifier

Evaluation metrics:
- Accuracy
- Precision
- Recall

---

## 5. Hyperparameter Tuning
The two strongest baseline models were selected:
1. Linear SVC
2. Logistic Regression

GridSearchCV was applied using stratified cross-validation.

Observation:
- The baseline models were already near optimal; hyperparameter tuning provided only marginal improvements.

---

## 6. Final Model
Logistic Regression was selected as the final model.

Test Set Performance:
- Accuracy: 0.9933
- Precision: 1.0000
- Recall: 0.9905

This is a relatively easy classification task due to the strong brightness-based signal present in the data.

---

## 7. Error Analysis
Only 1 image in the test set was misclassified.
The image appears to depict a twilight scene with relatively low brightness and saturation, and because the model relies mainly on brightness to make decisions, it tends to misclassify images with moderate lighting, like those taken at dusk/dawn.

---

## 8. Key Takeaways
1. Proper data ordering (sorted file loading) ensures reproducibility across environments.
2. Linear models can perform exceptionally well on structured, low-dimensional feature spaces.
3. Near-perfect accuracy does not imply robustness to edge cases.
4. Error analysis provides insight beyond metric reporting.

---

## 9. Tech Stack
- Python
- OpenCV
- NumPy
- Pandas
- Scikit-learn




Matplotlib

