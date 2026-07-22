# 🏋️ Machine Learning Practice Questions & Interactive Solutions

Organized into comprehensive practice sections covering **Theory Questions (150+)**, **Coding Questions (100+)**, **MCQs (100+)**, **Scenario Questions (75+)**, **Production Problems (50+)**, **Debugging Problems (50+)**, **Output Prediction (50+)**, and **Best Practices (50+)**.

> 💡 **Interactive Practice**: Click on any question block to reveal the **Simple Explanation**, **Best Way to Answer in an Interview**, and **Code / Technical Solution**.

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

### Supervised Learning

<details>
<summary><b>Q1: What is linear regression? State its assumptions.</b></summary>

#### 💡 Simple Explanation
Linear regression draws a straight line through data points to predict a continuous target (like house prices based on square footage).

#### 🎯 Best Way to Answer in an Interview
List the 4 core assumptions using the acronym **LINE**:
1. **L**inearity: Relationship between features and target is linear.
2. **I**ndependence: Residual errors are independent of each other (no autocorrelation).
3. **N**ormality: Residual errors follow a normal distribution $\mathcal{N}(0, \sigma^2)$.
4. **E**qual Variance (Homoscedasticity): Variance of residual errors is constant across all feature values.

#### 📝 Detailed Technical Answer
Minimizes Ordinary Least Squares (OLS) loss:
$$\mathcal{L}(w, b) = \frac{1}{2N} \sum_{i=1}^N (y_i - (w^T x_i + b))^2$$
Closed-form normal equation solution: $w = (X^T X)^{-1} X^T y$.
</details>

<details>
<summary><b>Q2: What is logistic regression? How does it estimate probabilities?</b></summary>

#### 💡 Simple Explanation
Logistic regression takes a straight line equation and squashes it through an S-shaped curve (Sigmoid) so the output is always a probability percentage between 0% and 100%.

#### 🎯 Best Way to Answer in an Interview
1. State the hypothesis equation $\hat{y} = \sigma(w^T x + b)$ where $\sigma(z) = \frac{1}{1 + e^{-z}}$.
2. Explain that coefficients $w_i$ represent the log-odds of the positive class.
3. Mention that it minimizes Binary Cross-Entropy (Log Loss) via gradient descent.

#### 📝 Detailed Technical Answer
Probability estimation:
$$P(y=1 | X) = \sigma(z) = \frac{1}{1 + e^{-(w^T X + b)}}$$
Log-odds interpretation:
$$\ln\left(\frac{P}{1-P}\right) = w^T X + b$$
Minimizes Log Loss: $\mathcal{L} = -\sum [y \log \hat{y} + (1-y) \log(1-\hat{y})]$.
</details>

<details>
<summary><b>Q18: Explain Decision Tree splitting criteria: Gini Impurity and Entropy.</b></summary>

#### 💡 Simple Explanation
Impurity measures how mixed up a bowl of candy is:
- If a bowl has 100% Red M&Ms, impurity is 0 (pure).
- If a bowl has 50% Red and 50% Blue M&Ms, impurity is highest (mixed).
Decision trees look for feature splits that make child bowls as pure as possible.

#### 🎯 Best Way to Answer in an Interview
1. Define Gini: $1 - \sum p_i^2$.
2. Define Entropy: $-\sum p_i \log_2(p_i)$.
3. Compare: Gini is computationally faster because it avoids calculating logarithm $\log_2(p_i)$. Both produce nearly identical tree splits in practice.
</details>

---

## 2. Coding Questions (100+)

<details>
<summary><b>PQ-CD1: Implement K-Means Clustering from Scratch (NumPy Only)</b></summary>

#### 💡 Simple Explanation
Pick $K$ random starting points (centroids). Repeat two steps:
1. Assign every data point to its closest centroid.
2. Move each centroid to the average center of all its assigned points.
Stop when centroids stop moving!

#### 🎯 Best Way to Answer in an Interview
1. Describe the 3 steps: Random centroid initialization $\to$ Distance matrix calculation & cluster assignment $\to$ Centroid re-estimation.
2. Write clean vectorized NumPy code without explicit python loops over data samples.

#### 💻 NumPy Implementation
```python
import numpy as np

class KMeansScratch:
    def __init__(self, k=3, max_iters=100, tol=1e-4):
        self.k = k
        self.max_iters = max_iters
        self.tol = tol
        self.centroids = None

    def fit(self, X):
        np.random.seed(42)
        random_indices = np.random.choice(len(X), self.k, replace=False)
        self.centroids = X[random_indices]

        for _ in range(self.max_iters):
            # Distance computation using broadcasted Euclidean norm
            distances = np.linalg.norm(X[:, np.newaxis] - self.centroids, axis=2)
            labels = np.argmin(distances, axis=1)

            new_centroids = np.array([
                X[labels == i].mean(axis=0) if np.sum(labels == i) > 0 else self.centroids[i]
                for i in range(self.k)
            ])

            if np.linalg.norm(new_centroids - self.centroids) < self.tol:
                break
            self.centroids = new_centroids

    def predict(self, X):
        distances = np.linalg.norm(X[:, np.newaxis] - self.centroids, axis=2)
        return np.argmin(distances, axis=1)
```
</details>

<details>
<summary><b>PQ-CD2: Write a PyTorch Custom Loss Function: Binary Focal Loss</b></summary>

#### 💡 Simple Explanation
Standard cross-entropy loss treats easy examples and hard examples with similar weight. Focal Loss dynamically scales down the loss for easy-to-classify examples, forcing the model to focus training efforts on hard, tricky negative examples.

#### 🎯 Best Way to Answer in an Interview
1. State formula: $\text{FL}(p_t) = -\alpha_t (1 - p_t)^\gamma \log(p_t)$.
2. Explain $\gamma$ parameter: When $\gamma=2$ and model is confident ($p_t=0.99$), weight $(1-0.99)^2 = 0.0001$ shrinks loss contribution by $10,000\times$.
3. Write clean PyTorch `nn.Module` code using `binary_cross_entropy_with_logits` for numerical stability.

#### 💻 PyTorch Implementation
```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class BinaryFocalLoss(nn.Module):
    def __init__(self, alpha=0.25, gamma=2.0):
        super().__init__()
        self.alpha = alpha
        self.gamma = gamma

    def forward(self, logits, targets):
        bce_loss = F.binary_cross_entropy_with_logits(logits, targets, reduction='none')
        probs = torch.sigmoid(logits)
        p_t = targets * probs + (1 - targets) * (1 - probs)
        alpha_factor = targets * self.alpha + (1 - targets) * (1 - self.alpha)
        focal_weight = alpha_factor * (1 - p_t) ** self.gamma

        return (focal_weight * bce_loss).mean()
```
</details>

---

## 3. MCQs (100+)

<details>
<summary><b>MCQ 1: Which of the following is NOT an activation function?</b></summary>

- **Options**: a) ReLU | b) Leaky ReLU | c) MaxPool | d) tanh
- **Answer**: **c) MaxPool**
- **Explanation**: MaxPool is a spatial downsampling pooling layer, not a non-linear activation function.
</details>

<details>
<summary><b>MCQ 2: L1 regularization tends to produce:</b></summary>

- **Options**: a) Dense weights | b) Sparse weights | c) No change | d) Negative weights
- **Answer**: **b) Sparse weights**
- **Explanation**: L1 (Lasso) penalty $|w|$ has diamond-shaped constraint corners at 0, forcing less important feature weights to become exactly zero.
</details>

<details>
<summary><b>MCQ 3: Which metric is most appropriate for a severely imbalanced dataset?</b></summary>

- **Options**: a) Accuracy | b) Precision | c) F1-score / PR-AUC | d) ROC-AUC
- **Answer**: **c) F1-score / PR-AUC**
- **Explanation**: Accuracy is inflated by true negatives. PR-AUC focuses directly on minority class precision and recall.
</details>

---

## 4. Debugging Problems (50+)

<details>
<summary><b>PQ-DB1: Neural Network Loss Stuck at Flat `0.6931` (-log(0.5))</b></summary>

#### 💡 Simple Explanation
`-log(0.5) = 0.6931`. This means your neural network is guessing 50/50 coin flips for every single input and isn't learning at all!

#### 🎯 Best Way to Answer in an Interview
Go through a systematic debugging checklist:
1. **Zero Gradients**: Forgetting `optimizer.zero_grad()` or `loss.backward()`.
2. **Zero Weight Initialization**: Initializing all weights to 0 (all neurons compute identical updates).
3. **Learning Rate**: Learning rate set to 0.0 or set too high causing immediate activation saturation.
4. **Broken Graph**: Accidentally calling `.detach()` on a tensor inside the forward pass.
</details>
