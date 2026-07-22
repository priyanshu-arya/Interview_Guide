# ⚡ Machine Learning (ML) Quick Revision Cheat Sheet

A concise revision document designed to refresh core concepts, formulas, code snippets, and comparison tables within **10–15 minutes** before an interview.

---

## 📌 Table of Contents

1. [Core Formulas & Mathematical Identities](#1-core-formulas--mathematical-identities)
2. [High-Value Comparison Tables](#2-high-value-comparison-tables)
3. [PyTorch & Scikit-Learn Boilerplate Syntax](#3-pytorch--scikit-learn-boilerplate-syntax)
4. [Top 10 ML Interview Pitfalls & Red Flags](#4-top-10-ml-interview-pitfalls--red-flags)
5. [Memory Tricks & Mnemonics](#5-memory-tricks--mnemonics)

---

## 1. Core Formulas & Mathematical Identities

### Loss Functions & Metrics
- **Mean Squared Error (MSE)**:
  $$\text{MSE} = \frac{1}{N} \sum_{i=1}^N (y_i - \hat{y}_i)^2$$
- **Binary Cross-Entropy (BCE) Loss**:
  $$\mathcal{L}_{\text{BCE}} = -\frac{1}{N} \sum_{i=1}^N \left[ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) \right]$$
- **Categorical Cross-Entropy Loss**:
  $$\mathcal{L}_{\text{CE}} = -\sum_{i=1}^N \sum_{c=1}^C y_{i,c} \log(\hat{y}_{i,c})$$
- **Precision, Recall, F1**:
  $$\text{Precision} = \frac{TP}{TP + FP}, \quad \text{Recall} = \frac{TP}{TP + FN}, \quad F_1 = 2 \cdot \frac{P \cdot R}{P + R}$$

### Optimization & Regularization
- **L1 Regularization (Lasso)**: $\mathcal{L}_{\text{total}} = \mathcal{L}_0 + \lambda \sum_j |w_j|$ (Drives weights to zero $\to$ Feature selection)
- **L2 Regularization (Ridge)**: $\mathcal{L}_{\text{total}} = \mathcal{L}_0 + \lambda \sum_j w_j^2$ (Shrinks weight magnitudes)
- **Self-Attention Mechanism**:
  $$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$
- **LoRA Decomposition**:
  $$W = W_0 + \Delta W = W_0 + \frac{\alpha}{r} (B \cdot A), \quad B \in \mathbb{R}^{d \times r}, A \in \mathbb{R}^{r \times k}, r \ll \min(d,k)$$

---

## 2. High-Value Comparison Tables

### Table 1: Classical Machine Learning Algorithms

| Algorithm | Model Type | Parametric? | Sensitivity to Feature Scale | Primary Hyperparameters | Best For |
|-----------|------------|-------------|------------------------------|-------------------------|----------|
| **Logistic Regression** | Linear Classifier | Yes | High | $\lambda, C$ (Penalty, solver) | Fast baseline, probabilistic outputs |
| **Decision Trees** | Non-Linear / Rule-based | No | None | `max_depth`, `min_samples_leaf` | Interpretable tabular data |
| **Random Forest** | Ensemble (Bagging) | No | None | `n_estimators`, `max_features` | High variance reduction, tabular data |
| **XGBoost / LightGBM** | Ensemble (Boosting) | No | None | `learning_rate`, `max_depth`, `subsample` | Structured Kaggle-style tabular problems |
| **Support Vector Machines** | Maximum Margin | No | High | $C, \gamma$, Kernel function | Medium tabular, text, high-dimensional |
| **K-Means** | Unsupervised Cluster | No | High | $K$, Initialization method | Customer segmentation, exploratory analysis |

---

### Table 2: Bagging vs. Boosting

| Feature | Bagging (e.g. Random Forest) | Boosting (e.g. XGBoost, AdaBoost) |
|---------|------------------------------|-----------------------------------|
| **Build Process** | Parallel (Independent trees) | Sequential (Each tree learns from previous errors) |
| **Primary Goal** | Reduces **Variance** (Overfitting) | Reduces **Bias** (Underfitting) |
| **Tree Depth** | Deep fully-grown trees | Shallow trees / Stumps |
| **Noise Sensitivity**| Robust to noisy data | Sensitive to outliers and noise |

---

### Table 3: Modern Architectures (CNN vs. RNN vs. Transformer)

| Architecture | Inductive Bias | Sequential Operations | Long-Distance Context | Primary Domain |
|--------------|----------------|-----------------------|-----------------------|----------------|
| **CNN** | Translation Invariance & Locality | $O(1)$ | $O(\log_k N)$ via pooling | Computer Vision, Images |
| **RNN / LSTM** | Sequential Ordering | $O(N)$ | $O(N)$ (Suffers from vanishing gradient) | Sequential/Speech (Historical) |
| **Transformer** | Minimal (Permutation Invariant) | $O(1)$ | $O(1)$ via Self-Attention | NLP, GenAI, Vision Transformers |

---

### Table 4: LLM Customization (Prompting vs RAG vs Fine-Tuning)

| Method | Knowledge Source | Training Required? | Hardware Cost | Hallucination Prevention |
|--------|------------------|--------------------|---------------|--------------------------|
| **Prompt Engineering** | In-context examples | No | $O(1)$ API calls | Low to Moderate |
| **RAG (Retrieval)** | External Vector DB | No (Retriever indexing) | Low (Vector Search) | High (Grounded in context) |
| **PEFT / LoRA** | Updated Weights | Yes (Low-rank adapters) | Moderate (Single GPU) | Moderate |
| **Full Fine-Tuning** | Updated Weights | Yes (Full model) | High (Multi-GPU Cluster)| Moderate to High |

---

## 3. PyTorch & Scikit-Learn Boilerplate Syntax

### PyTorch Training Loop Boilerplate

```python
import torch
import torch.nn as nn
import torch.optim as optim

# 1. Define Model
class SimpleMLP(nn.Module):
    def __init__(self, in_dim, hidden_dim, out_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(in_dim, hidden_dim),
            nn.BatchNorm1d(hidden_dim),
            nn.ReLU(),
            nn.Dropout(p=0.2),
            nn.Linear(hidden_dim, out_dim)
        )
    def forward(self, x):
        return self.net(x)

# 2. Setup Device, Model, Criterion, Optimizer
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = SimpleMLP(784, 128, 10).to(device)
criterion = nn.CrossEntropyLoss()
optimizer = optim.AdamW(model.parameters(), lr=1e-3, weight_decay=1e-2)

# 3. Training Epoch Loop
model.train()
for epoch in range(num_epochs):
    for X_batch, y_batch in train_loader:
        X_batch, y_batch = X_batch.to(device), y_batch.to(device)
        
        optimizer.zero_grad()           # Reset gradients
        outputs = model(X_batch)         # Forward pass
        loss = criterion(outputs, y_batch)# Compute loss
        loss.backward()                  # Backward pass
        torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0) # Clip gradients
        optimizer.step()                 # Update parameters
```

---

## 4. Top 10 ML Interview Pitfalls & Red Flags

1. ❌ **Data Leakage**: Fitting transformers/scalers before train-test split or time-series split.
2. ❌ **Using Accuracy on Imbalanced Data**: Evaluating a 99:1 target distribution with standard accuracy.
3. ❌ **Ignoring Missing Value Indicators**: Blindly mean-imputing without capturing missingness patterns.
4. ❌ **Forgetting `model.eval()` and `torch.no_grad()`**: Running PyTorch validation with Dropout active and gradient computation enabled.
5. ❌ **Scaling Tree Models unnecessarily**: Suggesting Z-score normalization for Random Forest or XGBoost.
6. ❌ **Confusing Batch Normalization in Training vs. Inference**: Not realizing BatchNorm uses running mean/var during inference.
7. ❌ **Over-relying on Default Learning Rates**: Not mentioning learning rate warm-up or decay schedules when training deep networks.
8. ❌ **Mistaking RAG for Parameter Fine-Tuning**: Claiming RAG alters pre-trained LLM weights.
9. ❌ **Lacking Baseline Comparison**: Recommending a complex Transformer/LLM before evaluating simple Logistic Regression / XGBoost baselines.
10. ❌ **Failing to check for Multicollinearity**: Using correlated features directly in Linear/Logistic regression without checking VIF or L2 regularization.

---

## 5. Memory Tricks & Mnemonics

- 💡 **FAME-DL** (Five Pillars of ML Interview):
  - **F**undamentals (Bias/Variance, Metrics)
  - **A**rchitectures (CNN, Transformer, LSTM)
  - **M**LOps (Serving, Pipelines, Drift)
  - **E**valuation (Precision, Recall, ROC-AUC)
  - **D**eep **L**earning (Backprop, Loss, Optimization)

- 💡 **PRECISION vs RECALL**:
  - **P**recision = **P**urity of predictions (How many positive predictions are actually correct?)
  - **R**ecall = **R**ecovery of actual positives (How many true positive instances did we capture?)

- 💡 **L1 vs L2 Shape**:
  - **L1** (Lasso) = Diamond corners on axis $\to$ Hits zero weights $\to$ **Sparse**.
  - **L2** (Ridge) = Smooth Circle $\to$ Shrinks weights uniformly $\to$ **Non-Sparse**.

---

## 6. Confusion Matrix Template

> [!IMPORTANT]
> The confusion matrix is the **#1 most-drawn diagram** in ML/DS interviews. Know it cold.

```
                   Predicted POSITIVE    Predicted NEGATIVE
Actual POSITIVE  |   True Positive (TP)  | False Negative (FN)  |  ← Type II Error
Actual NEGATIVE  |   False Positive (FP) | True Negative (TN)   |  ← Type I Error
```

| Metric | Formula | Intuition |
|--------|---------|-----------|
| **Accuracy** | $(TP + TN) / (TP + TN + FP + FN)$ | Overall correctness (unreliable on imbalanced data) |
| **Precision** | $TP / (TP + FP)$ | Of all positive predictions, how many were right? |
| **Recall (Sensitivity)** | $TP / (TP + FN)$ | Of all actual positives, how many did we catch? |
| **Specificity** | $TN / (TN + FP)$ | Of all actual negatives, how many did we correctly identify? |
| **F1 Score** | $2 \cdot \frac{Precision \cdot Recall}{Precision + Recall}$ | Harmonic mean — balances Precision and Recall |
| **F-Beta Score** | $(1 + \beta^2) \cdot \frac{P \cdot R}{\beta^2 P + R}$ | $\beta > 1$: Recall-weighted; $\beta < 1$: Precision-weighted |

**Key Tradeoffs**:
- **High Precision, Low Recall**: Model is conservative — misses many positives but rarely false alarms. (Good for: spam filtering, medical device alerts)
- **High Recall, Low Precision**: Model is aggressive — catches everything but with many false alarms. (Good for: cancer screening, fraud detection)

---

## 7. ROC-AUC vs PR-AUC

> [!TIP]
> When class distribution is **imbalanced**, always use **PR-AUC**. ROC-AUC can be misleadingly optimistic.

```
ROC Curve: True Positive Rate (Recall) vs False Positive Rate
PR Curve:  Precision vs Recall

A random classifier has:
  - ROC-AUC = 0.5 (diagonal line)
  - PR-AUC  = class_positive_rate (baseline = prevalence)
```

| Dimension | ROC-AUC | PR-AUC |
|-----------|---------|--------|
| **X-Axis** | False Positive Rate = FP/(FP+TN) | Recall = TP/(TP+FN) |
| **Y-Axis** | True Positive Rate (Recall) | Precision = TP/(TP+FP) |
| **Random Baseline** | AUC = 0.5 | AUC = class prevalence |
| **Best When** | Balanced class distribution | Imbalanced classes (fraud, rare disease) |
| **Insensitive To** | Class imbalance (uses TN) | Large TN counts (doesn't include TN) |
| **Interpretation** | P(model ranks random positive above random negative) | Overall quality at detecting the minority class |

**Decision Rule**: Use **PR-AUC** when the positive class is rare (<10% of data). Use **ROC-AUC** for balanced datasets.

---

## 8. Cross-Validation Strategies

| Strategy | How It Works | When To Use | Pitfall |
|----------|-------------|-------------|---------|
| **K-Fold** | Splits data into K folds; each fold used once as validation | Standard tabular ML | Data leakage if preprocessing done before split |
| **Stratified K-Fold** | K-Fold with class distribution preserved in each fold | Classification with class imbalance | Still leaks if preprocessing not inside pipeline |
| **Repeated K-Fold** | K-Fold repeated N times with different shuffles | Small datasets to reduce variance | Computationally expensive (K×N model fits) |
| **Time Series Split** | Train on past, validate on future (no shuffling!) | Any time-ordered data | Cannot shuffle — temporal order must be preserved |
| **Group K-Fold** | Ensures all rows of a group stay in same fold | Medical data (patient groups), NLP (per-document) | Group leakage if ignored |
| **Leave-One-Out (LOO)** | K = N (each sample is a validation set once) | Very small datasets (<100 samples) | Very slow; high variance estimates |

```python
from sklearn.model_selection import StratifiedKFold, TimeSeriesSplit
from sklearn.pipeline import Pipeline

# Stratified K-Fold for imbalanced classification
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
for fold, (train_idx, val_idx) in enumerate(skf.split(X, y)):
    X_tr, X_val = X[train_idx], X[val_idx]
    y_tr, y_val = y[train_idx], y[val_idx]
    model.fit(X_tr, y_tr)
    print(f"Fold {fold} AUC: {roc_auc_score(y_val, model.predict_proba(X_val)[:, 1]):.4f}")

# Time Series Cross-Validation (no shuffle, forward-only)
tscv = TimeSeriesSplit(n_splits=5, gap=7)  # 7-day gap prevents leakage
for train_idx, val_idx in tscv.split(X):
    # Train always before val in time
    X_tr, X_val = X.iloc[train_idx], X.iloc[val_idx]
```

> [!WARNING]
> **Always fit preprocessing (scaling, imputation, encoding) INSIDE the CV loop** using `sklearn.Pipeline`. Fitting a scaler on all data before CV causes data leakage from validation set statistics into the training process.

