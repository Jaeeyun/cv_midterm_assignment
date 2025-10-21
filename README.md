# cv_midterm_assignment

## CIFAR-10 Classification with K-Nearest Neighbors (KNN)


## KNN Classification Pipeline
1. **Flattening**: Images are reshaped into 1D vectors.
2. **Standardization (StandardScaler)**: Each feature is normalized to have zero mean and unit variance, which prevents features with large numeric scales from dominating distance computation.
3. **Dimensionality Reduction (PCA, 350 components with whitening)**: Reduces computational cost and noise, while keeping most variance. Whitening improves stability of cosine distance.
4. **KNN (cosine distance, distance-based weighting)**  
   - `metric='cosine'`: compares angle similarity between samples  
   - `weights='distance'`: closer neighbors have higher influence  
   - Larger **k** yields smoother decision boundaries, smaller **k** yields more variance

> Since CIFAR-10 is a multi-class dataset, using even values or powers of two for k does not increase the risk of ties, so I experimented with k = 1, 2, 4, 8, ..., 2048 without theoretical issues.

---

### 1. Train/Test Split
- Best **k**: k = 516
- **Accuracy**: 0.5682
- **Precision**: 0.5755
- **Recall**: 0.5682
- **F1-score**: 0.5650

<img width="700" height="470" alt="image" src="https://github.com/user-attachments/assets/61bb8261-38af-4100-9721-d310d025148a" />

> Scores for multiple k values on the test set (single split).  
> **Limitation**: The test set is used directly, so this result may be overfitted to the test distribution.

---

### 2. Train/Validation/Test Split (Hyperparameter Tuning)
- Best **k**: k = 256
- **Accuracy**: 0.5651
- **Precision**: 0.5733
- **Recall**: 0.5651
- **F1-score**: 0.5619

<img width="700" height="470" alt="image" src="https://github.com/user-attachments/assets/3db0be6b-8696-4254-8b82-3b7244377e6d" />

> Scores for multiple k values on the validation set.  
> The selected k is less biased than the Train/Test-only approach.

---

### 3. 5-Fold Cross Validation
- Best **k**: k = 256
- **Mean Accuracy**: 0.5550 (± 0.0053)
- **Mean Precision**: 0.5635
- **Mean Recall**: 0.5552
- **Mean F1-score**: 0.5522

<img width="1189" height="823" alt="image" src="https://github.com/user-attachments/assets/48a70fad-da1f-4ad3-8063-79b146e4c110" />

> 5-fold cross-validation results with error bars.  
> Performance is stable across folds, and k = 256 shows the best balance.

**Test Result after CV**  
Since the best k from cross-validation is `k = 256`, the final test result matches the result from section (2).
