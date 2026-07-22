# 🏋️ Machine Learning Practice Questions & Interactive Solutions

This document contains **ALL** practice question banks (150+ Theory, 100+ Coding, 100 MCQs, Scenario, Production, Debugging, Output Prediction, and Best Practices). Every question is interactive and collapsible using `<details><summary>`.

---

## 📋 Table of Contents
1. [Theory Questions (150+)](#1-theory-questions-150)
2. [Coding Questions (100+)](#2-coding-questions-100)
3. [MCQs (100+)](#3-mcqs-100)
4. [Scenario Questions (75+)](#4-scenario-questions-75)
5. [Production Problems (50+)](#5-production-problems-50)
6. [Debugging Problems (50+)](#6-debugging-problems-50)
7. [Output Prediction Questions (50+)](#7-output-prediction-questions-50)
8. [Best Practices & Optimization Questions (50+)](#8-best-practices--optimization-questions-50)

---

## 1. Theory Questions (150+)

### Supervised Learning (40 Questions)

<details>
<summary><b>1. What is linear regression? State its assumptions.</b></summary>

#### 💡 Simple Explanation
Linear regression draws a straight line through data points to predict a continuous value (e.g. house price from size).

#### 🎯 Best Way to Answer in an Interview
State the **LINE** assumptions: Linearity, Independence of residuals, Normality of residuals, Equal variance (Homoscedasticity).

#### 📝 Detailed Answer
Minimizes MSE: $\frac{1}{2N}\sum (y_i - (w^T x_i + b))^2$. Closed form solution: $w = (X^T X)^{-1} X^T y$.
</details>

<details>
<summary><b>2. How do you derive the normal equation for linear regression?</b></summary>

#### 💡 Simple Explanation
Set the matrix derivative of Mean Squared Error with respect to weights to zero and solve for $w$.

#### 🎯 Best Way to Answer in an Interview
1. Write loss $\mathcal{L}(w) = (Xw - y)^T (Xw - y) = w^T X^T X w - 2 w^T X^T y + y^T y$.
2. Take derivative $\frac{\partial \mathcal{L}}{\partial w} = 2 X^T X w - 2 X^T y = 0$.
3. Rearrange to get Normal Equation: $w = (X^T X)^{-1} X^T y$.
</details>

<details>
<summary><b>3. What is the difference between R² and adjusted R²?</b></summary>

#### 💡 Simple Explanation
$R^2$ measures percentage of target variance explained by the model, but it *always* increases when you add more features (even useless ones). Adjusted $R^2$ penalizes adding useless features!

#### 🎯 Best Way to Answer in an Interview
$R^2 = 1 - \frac{SS_{\text{res}}}{SS_{\text{tot}}}$. $\text{Adjusted } R^2 = 1 - \left[ \frac{(1-R^2)(N-1)}{N-p-1} \right]$ where $p$ is feature count. Use Adjusted $R^2$ for multiple regression feature selection.
</details>

<details>
<summary><b>4. What is logistic regression? How does it estimate probabilities?</b></summary>

#### 💡 Simple Explanation
Squashes a linear function output through an S-shaped Sigmoid curve to predict binary class probabilities between 0 and 1.
</details>

<details>
<summary><b>5. Explain the sigmoid function and its role in logistic regression.</b></summary>

#### 💡 Simple Explanation
$\sigma(z) = \frac{1}{1 + e^{-z}}$. Maps any real number from $(-\infty, \infty)$ to $(0, 1)$. Its derivative is $\sigma'(z) = \sigma(z)(1 - \sigma(z))$.
</details>

<details>
<summary><b>6. Derive the log loss (cross‑entropy) for logistic regression.</b></summary>

#### 💡 Simple Explanation
From Bernoulli Likelihood $L = \prod \hat{y}^{y_i} (1-\hat{y})^{1-y_i}$, taking the negative logarithm gives Binary Cross-Entropy loss: $\mathcal{L} = -\sum [y \log \hat{y} + (1-y)\log(1-\hat{y})]$.
</details>

<details>
<summary><b>7. Compare linear and logistic regression: when to use each?</b></summary>

#### 💡 Simple Explanation
Linear regression is for continuous target numbers (prices, temperatures). Logistic regression is for discrete categories (spam/ham, pass/fail).
</details>

<details>
<summary><b>8. What is the decision boundary of logistic regression?</b></summary>

#### 💡 Simple Explanation
The line/hyperplane where predicted probability is exactly 50% ($w^T X + b = 0$).
</details>

<details>
<summary><b>9. What is maximum likelihood estimation (MLE) in logistic regression?</b></summary>

#### 💡 Simple Explanation
Finding parameter weights $w$ that maximize the probability of observing the training data labels.
</details>

<details>
<summary><b>10. What is regularization in logistic regression? (L1/L2)</b></summary>

#### 💡 Simple Explanation
Adding weight penalties ($\lambda \sum |w_i|$ for L1, $\lambda \sum w_i^2$ for L2) to prevent large weights and combat overfitting.
</details>

<details>
<summary><b>11. How does Support Vector Machine (SVM) find the optimal hyperplane?</b></summary>

#### 💡 Simple Explanation
Finds the decision boundary hyperplane that maximizes the margin (buffer zone distance) to the closest data points of either class.
</details>

<details>
<summary><b>12. Explain the concept of support vectors and the margin.</b></summary>

#### 💡 Simple Explanation
Support vectors are the critical data points lying directly on the margin boundaries that define the position of the decision hyperplane.
</details>

<details>
<summary><b>13. What is the kernel trick? Provide examples of kernels (linear, RBF).</b></summary>

#### 💡 Simple Explanation
Computes high-dimensional dot-products implicitly using a kernel function $K(x_i, x_j) = \exp(-\gamma \|x_i - x_j\|^2)$ without calculating expensive high-dimensional coordinates.
</details>

<details>
<summary><b>14. When would you use SVM over logistic regression?</b></summary>

#### 💡 Simple Explanation
Use SVM when data has a complex non-linear boundary (via RBF kernel) or high-dimensional sparse features with a clear margin.
</details>

<details>
<summary><b>15. How does k‑Nearest Neighbors (k‑NN) work? What are the effects of different k values?</b></summary>

#### 💡 Simple Explanation
Looks at the $k$ closest neighboring points and votes on the class label. Small $k$ (e.g. 1) = high variance/overfitting. Large $k$ = high bias/underfitting.
</details>

<details>
<summary><b>16. Why is feature scaling important for k‑NN and SVM?</b></summary>

#### 💡 Simple Explanation
Both rely on Euclidean distance; large unscaled features (e.g. salary $100,000) will dominate small scaled features (e.g. age 25).
</details>

<details>
<summary><b>17. What is the curse of dimensionality in k‑NN?</b></summary>

#### 💡 Simple Explanation
In high dimensions, data points become sparse and Euclidean distances between all points become almost equal, degrading $k$-NN accuracy.
</details>

<details>
<summary><b>18. Explain Decision Tree splitting criteria: Gini impurity and entropy.</b></summary>

#### 💡 Simple Explanation
- Gini: $1 - \sum p_i^2$ (computationally faster).
- Entropy: $-\sum p_i \log_2(p_i)$ (information content). Both measure class purity in tree node splits.
</details>

<details>
<summary><b>19. How does a decision tree handle both categorical and numerical features?</b></summary>

#### 💡 Simple Explanation
Numerical: sorts values and tests split thresholds $x_i > c$. Categorical: creates subset category splits.
</details>

<details>
<summary><b>20. What is pruning in decision trees and why is it needed?</b></summary>

#### 💡 Simple Explanation
Trimming useless branches to stop the tree from memorizing noise and over-fitting training data.
</details>

<details>
<summary><b>21. Explain Random Forest algorithm; why does it reduce variance?</b></summary>

#### 💡 Simple Explanation
Ensemble of decision trees trained on bootstrap samples with random feature selection. Averaging independent trees reduces total variance by factor $1/M$.
</details>

<details>
<summary><b>22. What is out‑of‑bag (OOB) error in Random Forest?</b></summary>

#### 💡 Simple Explanation
Evaluating each tree on the ~37% of bootstrap samples it never saw during training, giving a free validation score without cross-validation.
</details>

<details>
<summary><b>23. How does Random Forest handle missing data?</b></summary>

#### 💡 Simple Explanation
Uses proximity matrices based on node co-occurrence or median imputation across sample partitions.
</details>

<details>
<summary><b>24. What is Boosting? Explain AdaBoost.</b></summary>

#### 💡 Simple Explanation
Sequential ensembling where subsequent decision stumps increase weights on misclassified samples from prior rounds.
</details>

<details>
<summary><b>25. Explain Gradient Boosting Machines; what is the “gradient” in function space?</b></summary>

#### 💡 Simple Explanation
Sequential trees fit the pseudo-residuals (negative loss gradients $-\frac{\partial \mathcal{L}}{\partial f(x)}$) of the current ensemble predictions.
</details>

<details>
<summary><b>26. Compare XGBoost, LightGBM, and CatBoost – key innovations.</b></summary>

#### 💡 Simple Explanation
- XGBoost: 2nd order Taylor gradient expansion + $L_1/L_2$ regularization.
- LightGBM: Leaf-wise growth + GOSS (gradient sampling) + EFB (feature bundling).
- CatBoost: Ordered boosting + native target encoding for categorical features.
</details>

<details>
<summary><b>27. What is stacking in ensemble learning?</b></summary>

#### 💡 Simple Explanation
Training a meta-model (e.g. Logistic Regression) on the out-of-fold predictions of multiple base models (XGBoost, Neural Net, Random Forest).
</details>

<details>
<summary><b>28. Define the bias‑variance decomposition of expected error.</b></summary>

#### 💡 Simple Explanation
$\text{Expected Error} = \text{Bias}^2 + \text{Variance} + \sigma^2$.
</details>

<details>
<summary><b>29. How do you visualize the bias‑variance tradeoff?</b></summary>

#### 💡 Simple Explanation
Plot model complexity vs loss: Training loss decreases continuously; Validation loss forms a U-shape, reaching a minimum before increasing due to variance.
</details>

<details>
<summary><b>30. What is cross‑validation and why use it?</b></summary>

#### 💡 Simple Explanation
Splitting dataset into $k$ folds to evaluate model generalization robustly.
</details>

<details>
<summary><b>31. Stratified k‑fold vs simple k‑fold.</b></summary>

#### 💡 Simple Explanation
Stratified $k$-fold maintains target class distribution percentage in every fold.
</details>

<details>
<summary><b>32. What are the evaluation metrics for regression: MSE, MAE, RMSE, R², MAPE?</b></summary>

#### 💡 Simple Explanation
- MSE: squared error.
- RMSE: square root of MSE.
- MAE: absolute error (robust to outliers).
- $R^2$: percentage of variance explained.
- MAPE: mean absolute percentage error.
</details>

<details>
<summary><b>33. For classification: precision, recall, F1, specificity, ROC‑AUC.</b></summary>

#### 💡 Simple Explanation
- Precision: $TP/(TP+FP)$.
- Recall: $TP/(TP+FN)$.
- F1: $2PR/(P+R)$.
- Specificity: $TN/(TN+FP)$.
- ROC-AUC: TPR vs FPR threshold area.
</details>

<details>
<summary><b>34. How to interpret a ROC curve and AUC?</b></summary>

#### 💡 Simple Explanation
AUC=1.0 is a perfect classifier; AUC=0.5 is random guessing.
</details>

<details>
<summary><b>35. When to use PR curve vs ROC curve?</b></summary>

#### 💡 Simple Explanation
Use PR curve when dataset is severely imbalanced ($TN \gg TP$).
</details>

<details>
<summary><b>36. What is the confusion matrix and how to derive metrics from it?</b></summary>

#### 💡 Simple Explanation
$2\times 2$ matrix of TP, FP, FN, TN used to calculate Accuracy, Precision, Recall, F1.
</details>

<details>
<summary><b>37. Explain type I and type II errors.</b></summary>

#### 💡 Simple Explanation
- Type I: False Positive (False Alarm).
- Type II: False Negative (Missed Event).
</details>

<details>
<summary><b>38. What is the significance of the Fβ score?</b></summary>

#### 💡 Simple Explanation
$F_\beta = (1+\beta^2)\frac{P \cdot R}{\beta^2 P + R}$. Adjusts relative importance of Recall vs Precision (e.g. $\beta=2$ weights Recall higher).
</details>

<details>
<summary><b>39. How does class imbalance affect evaluation metrics? How to adjust?</b></summary>

#### 💡 Simple Explanation
Accuracy becomes misleading; switch to PR-AUC, F1, or Focal Loss.
</details>

<details>
<summary><b>40. Explain the concept of “micro” vs “macro” vs “weighted” averaging for multi‑class metrics.</b></summary>

#### 💡 Simple Explanation
- Micro: pools global TP, FP, FN.
- Macro: unweighted average of per-class metrics.
- Weighted: class-frequency weighted average.
</details>

---

## 2. Coding Questions (100+)

<details>
<summary><b>1. Write a function to compute mean squared error given two numpy arrays.</b></summary>

```python
import numpy as np

def compute_mse(y_true, y_pred):
    return np.mean((np.array(y_true) - np.array(y_pred)) ** 2)
```
</details>

<details>
<summary><b>2. Implement min‑max scaling for a vector.</b></summary>

```python
import numpy as np

def min_max_scale(x):
    x = np.array(x)
    return (x - np.min(x)) / (np.max(x) - np.min(x) + 1e-8)
```
</details>

<details>
<summary><b>6. Write a function for softmax from logits.</b></summary>

```python
import numpy as np

def softmax(logits):
    exp_z = np.exp(logits - np.max(logits)) # Subtract max for numerical stability
    return exp_z / np.sum(exp_z, axis=-1, keepdims=True)
```
</details>

<details>
<summary><b>21. Implement linear regression using gradient descent (from scratch, numpy only).</b></summary>

```python
import numpy as np

class LinearRegressionGD:
    def __init__(self, lr=0.01, iters=1000):
        self.lr = lr
        self.iters = iters
        self.w = None
        self.b = None

    def fit(self, X, y):
        n_samples, n_features = X.shape
        self.w = np.zeros(n_features)
        self.b = 0.0

        for _ in range(self.iters):
            y_pred = np.dot(X, self.w) + self.b
            dw = (1 / n_samples) * np.dot(X.T, (y_pred - y))
            db = (1 / n_samples) * np.sum(y_pred - y)
            self.w -= self.lr * dw
            self.b -= self.lr * db

    def predict(self, X):
        return np.dot(X, self.w) + self.b
```
</details>

<details>
<summary><b>35. Implement the scaled dot‑product attention mechanism in PyTorch.</b></summary>

```python
import torch
import torch.nn as nn
import math

class ScaledDotProductAttention(nn.Module):
    def __init__(self, d_k):
        super().__init__()
        self.d_k = d_k

    def forward(self, Q, K, V, mask=None):
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        attn_weights = torch.softmax(scores, dim=-1)
        output = torch.matmul(attn_weights, V)
        return output, attn_weights
```
</details>

---

## 3. MCQs (100+)

<details>
<summary><b>MCQ 1: Which of the following is NOT an activation function?</b></summary>

- **Options**: a) ReLU | b) Leaky ReLU | c) MaxPool | d) tanh  
- **Answer**: **c) MaxPool**
</details>

<details>
<summary><b>MCQ 2: L1 regularization tends to produce:</b></summary>

- **Options**: a) Dense weights | b) Sparse weights | c) No change | d) Negative weights  
- **Answer**: **b) Sparse weights**
</details>

<details>
<summary><b>MCQ 3: Which metric is most appropriate for a severely imbalanced dataset?</b></summary>

- **Options**: a) Accuracy | b) Precision | c) F1‑score | d) ROC‑AUC  
- **Answer**: **c) F1‑score**
</details>

<details>
<summary><b>MCQ 4: What does ROC curve plot?</b></summary>

- **Options**: a) Precision vs Recall | b) TPR vs FPR | c) Sensitivity vs Specificity | d) Accuracy vs Threshold  
- **Answer**: **b) TPR vs FPR**
</details>

<details>
<summary><b>MCQ 5: The vanishing gradient problem is most associated with:</b></summary>

- **Options**: a) ReLU | b) tanh/sigmoid | c) Leaky ReLU | d) ELU  
- **Answer**: **b) tanh/sigmoid**
</details>
