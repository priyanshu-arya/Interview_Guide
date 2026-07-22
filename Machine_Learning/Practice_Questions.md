# 🏋️ Machine Learning Practice Questions & Solutions

Organized into 8 hands-on categories designed for active practice and self-assessment. Every entry includes a **Problem Statement**, **Difficulty**, **Hint**, **Expected Approach**, and **Common Mistakes**.

---

## 📌 Table of Contents
1. [Concept Questions](#1-concept-questions)
2. [Coding Questions](#2-coding-questions)
3. [Debugging Questions](#3-debugging-questions)
4. [Output Prediction Questions](#4-output-prediction-questions)
5. [Scenario-Based Questions](#5-scenario-based-questions)
6. [Tricky Questions](#6-tricky-questions)
7. [Practical Problems](#7-practical-problems)
8. [Interview Challenges](#8-interview-challenges)

---

## 1. Concept Questions

### PQ-C1: Why does Naive Bayes assume conditional independence between features? What happens if this assumption is violated?
- **Difficulty**: Easy
- **Hint**: Look at Bayes' Theorem expansion for high-dimensional feature vectors $P(x_1, x_2, \dots, x_d | y)$.
- **Expected Approach**:
  - *Why*: Without conditional independence $P(x_1, x_2, \dots, x_d | y) = \prod_{i=1}^d P(x_i | y)$, computing joint probabilities requires estimating $O(2^d)$ parameters for $d$ binary features, leading to severe sample complexity explosion. The "naive" independence assumption reduces parameters to $O(d)$.
  - *Violation Effect*: If features are highly correlated (e.g., duplicate words in text), Naive Bayes overcounts evidence, producing extreme probability predictions (probabilities pushed toward 0 or 1). However, classification ranking often remains surprisingly accurate.
- **Common Mistakes**: Claiming Naive Bayes cannot be used if features are correlated. (It often performs very well in practice, e.g., text classification).

---

### PQ-C2: Explain the difference between Generative and Discriminative models in terms of probability distribution.
- **Difficulty**: Medium
- **Hint**: Think about joint distribution $P(X, y)$ vs conditional distribution $P(y | X)$.
- **Expected Approach**:
  - **Discriminative Models**: Model conditional distribution $P(y | X)$ directly. They learn decision boundaries between classes (e.g., Logistic Regression, SVM, Decision Trees, Neural Networks).
  - **Generative Models**: Model joint distribution $P(X, y) = P(X | y)P(y)$ or data distribution $P(X)$. They learn how data is generated and can sample new synthetic instances (e.g., Naive Bayes, GMMs, VAEs, GANs, Autoregressive LLMs).
- **Common Mistakes**: Thinking Logistic Regression is a generative model because it outputs probabilities.

---

## 2. Coding Questions

### PQ-CD1: Implement K-Means Clustering from Scratch (NumPy Only)
- **Difficulty**: Medium
- **Hint**: Iteratively assign points to nearest centroid using Euclidean distance, then recompute centroids as column means.

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
        # Randomly initialize k centroids from dataset points
        random_indices = np.random.choice(len(X), self.k, replace=False)
        self.centroids = X[random_indices]

        for _ in range(self.max_iters):
            # 1. Compute distances from points to centroids: shape (N, k)
            distances = np.linalg.norm(X[:, np.newaxis] - self.centroids, axis=2)
            
            # 2. Assign cluster labels based on minimum distance
            labels = np.argmin(distances, axis=1)

            # 3. Compute new centroids
            new_centroids = np.array([
                X[labels == i].mean(axis=0) if np.sum(labels == i) > 0 else self.centroids[i]
                for i in range(self.k)
            ])

            # Check convergence
            if np.linalg.norm(new_centroids - self.centroids) < self.tol:
                break
            self.centroids = new_centroids

    def predict(self, X):
        distances = np.linalg.norm(X[:, np.newaxis] - self.centroids, axis=2)
        return np.argmin(distances, axis=1)
```

- **Common Mistakes**: Forgetting to handle empty clusters (division by zero when computing cluster mean).

---

### PQ-CD2: Write a PyTorch Custom Loss Function: Focal Loss for Binary Classification
- **Difficulty**: Hard
- **Hint**: $\text{FL}(p_t) = -\alpha_t (1 - p_t)^\gamma \log(p_t)$. Compute numerical stability using `log_sigmoid` or `binary_cross_entropy_with_logits`.

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
        # logits: (N, 1), targets: (N, 1)
        bce_loss = F.binary_cross_entropy_with_logits(logits, targets, reduction='none')
        probs = torch.sigmoid(logits)
        
        # p_t is prob of true class
        p_t = targets * probs + (1 - targets) * (1 - probs)
        alpha_factor = targets * self.alpha + (1 - targets) * (1 - self.alpha)
        focal_weight = alpha_factor * (1 - p_t) ** self.gamma

        focal_loss = focal_weight * bce_loss
        return focal_loss.mean()
```

- **Common Mistakes**: Applying `torch.sigmoid` twice when using `binary_cross_entropy_with_logits`.

---

## 3. Debugging Questions

### PQ-DB1: Debugging PyTorch Neural Network Loss Stuck at Constant Value
- **Difficulty**: Medium
- **Problem Statement**: An engineer trains a binary classifier, but the training loss stays completely flat at `0.6931` (`-log(0.5)`) for 50 epochs.
- **Hint**: What does `-log(0.5)` signify in Binary Cross-Entropy?
- **Expected Approach**:
  - `0.6931` is $-\log(0.5)$, meaning the network is outputting constant uniform predictions $p=0.5$ for every input.
  - **Root Causes**:
    1. Zeroed gradients: Forgetting `optimizer.zero_grad()`.
    2. Zero weight initialization: Initializing all weights to zero (symmetry broken failure).
    3. Learning rate set to 0.0 or learning rate extremely high causing immediate weight explosion into flat Sigmoid saturation regions.
    4. Missing non-linear activation or broken backprop connection (`detach()` called accidentally).

---

### PQ-DB2: Silent Data Leakage in Feature Standardization
- **Difficulty**: Easy
- **Problem Statement**: Identify the bug in the following scikit-learn preprocessing code snippet:

```python
# BUGGY CODE
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X) # <--- BUG HERE!
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_test=0.2)
```

- **Hint**: When should `StandardScaler.fit()` be called?
- **Expected Approach**:
  - `scaler.fit_transform(X)` computes global mean $\mu$ and standard deviation $\sigma$ across the **entire dataset** (including test samples) prior to splitting. Test set statistics leak into the training features.
  - **Fix**:

```python
# CORRECT CODE
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train) # Fit ONLY on Train
X_test_scaled = scaler.transform(X_test)       # Transform Test using Train stats
```

---

## 4. Output Prediction Questions

### PQ-OP1: Compute Output Shape of 2D Convolution Layer
- **Difficulty**: Easy
- **Problem Statement**: What is the output shape of an input tensor of size `(Batch=32, Channels=3, Height=64, Width=64)` passed through a 2D convolution with `out_channels=16, kernel_size=5, stride=2, padding=1`?
- **Formula**:
  $$H_{\text{out}} = \left\lfloor \frac{H_{\text{in}} + 2 \times P - K}{S} \right\rfloor + 1$$
- **Calculation**:
  $$H_{\text{out}} = \left\lfloor \frac{64 + 2(1) - 5}{2} \right\rfloor + 1 = \left\lfloor \frac{61}{2} \right\rfloor + 1 = 30 + 1 = 31$$
- **Output Shape**: `(32, 16, 31, 31)`

---

### PQ-OP2: Calculate Number of Trainable Parameters in Multi-Head Attention
- **Difficulty**: Hard
- **Problem Statement**: Given $d_{\text{model}} = 512$ and number of heads $h = 8$, calculate total trainable weight parameters in a Multi-Head Attention layer (including $W_Q, W_K, W_V, W_O$ projections, ignoring biases).
- **Calculation**:
  - $W_Q \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}} \rightarrow 512 \times 512 = 262,144$
  - $W_K \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}} \rightarrow 512 \times 512 = 262,144$
  - $W_V \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}} \rightarrow 512 \times 512 = 262,144$
  - $W_O \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}} \rightarrow 512 \times 512 = 262,144$
  - **Total Parameters**: $4 \times 512^2 = 1,048,576$ parameters ($\approx 1.05\text{M}$).

---

## 5. Scenario-Based Questions

### PQ-SC1: Designing a Real-Time E-Commerce Recommendation System
- **Difficulty**: Hard
- **Scenario**: You are tasked with building a product recommendation module for an e-commerce platform processing 50,000 requests/sec with a strict 30ms latency SLA.
- **Expected Approach**:
  1. **Offline Phase**: Generate item and user embeddings using Two-Tower Deep Learning / Graph Neural Networks. Index product embeddings in HNSW Vector DB (Milvus / Redis).
  2. **Online Phase**:
     - *Retrieval (sub-5ms)*: Fetch user profile embedding from Redis feature store. Query Vector DB for top-100 candidate items using ANN dot-product search.
     - *Ranking (sub-15ms)*: Score candidates using lightweight LightGBM or distilled Transformer model predicting purchase probability.
     - *Business Rules (sub-5ms)*: Filter out of stock items, apply brand diversity filters.

---

## 6. Tricky Questions

### PQ-TR1: If you add 1,000 duplicate identical rows to a dataset, how does it affect Decision Trees vs Logistic Regression?
- **Difficulty**: Medium
- **Expected Approach**:
  - **Decision Trees**: **Unaffected**. Tree splits are determined by feature order and relative class impurity ratios. Duplicate identical rows do not change node Gini/Entropy threshold split values.
  - **Logistic Regression**: **Affected**. The loss function sums over all sample log-losses. Duplicating rows acts as implicit sample weighting, altering gradient updates and moving optimal decision boundary weights $w$.

---

### PQ-TR2: Is it possible for a model to have 99% accuracy but a zero F1-Score?
- **Difficulty**: Easy
- **Expected Approach**:
  - **Yes!** In an imbalanced dataset where 99% of samples are class 0 and 1% are class 1, a naive model that predicts class 0 for all instances achieves:
    - $\text{Accuracy} = 99\%$
    - $\text{True Positives (TP)} = 0 \implies \text{Precision} = 0, \text{Recall} = 0 \implies \text{F1-Score} = 0$.

---

## 7. Practical Problems

### PQ-PR1: Mitigating "Dying ReLU" Neurons in Deep Networks
- **Difficulty**: Medium
- **Expected Approach**:
  - Replace standard ReLU $\max(0, x)$ with **Leaky ReLU** $\max(0.01x, x)$ or **ELU** $\alpha(e^x - 1)$.
  - Use **He Initialization** ($w \sim \mathcal{N}(0, \sqrt{2/n_{\text{in}}})$, specifically designed for non-linear ReLU activations).
  - Apply Batch Normalization to keep layer activations centered around zero mean.

---

## 8. Interview Challenges

### PQ-IC1: Derive Backpropagation through Time (BPTT) for a Vanilla RNN Cell
- **Difficulty**: Hard
- **Challenge Statement**: Given hidden state update $h_t = \tanh(W_{hh} h_{t-1} + W_{xh} x_t + b_h)$ and total loss $\mathcal{L} = \sum_{t=1}^T \mathcal{L}_t$, derive $\frac{\partial \mathcal{L}}{\partial W_{hh}}$.
- **Solution Derivation**:
  $$\frac{\partial \mathcal{L}}{\partial W_{hh}} = \sum_{t=1}^T \sum_{k=1}^t \frac{\partial \mathcal{L}_t}{\partial h_t} \left( \prod_{j=k+1}^t \frac{\partial h_j}{\partial h_{j-1}} \right) \frac{\partial h_k}{\partial W_{hh}}$$
  where Jacobian $\frac{\partial h_j}{\partial h_{j-1}} = \text{diag}(1 - \tanh^2(\cdot)) W_{hh}^T$.
  This product of matrices $\prod_{j=k+1}^t W_{hh}^T$ clearly demonstrates why vanilla RNNs suffer from vanishing gradients when eigenvalues of $W_{hh} < 1$.
