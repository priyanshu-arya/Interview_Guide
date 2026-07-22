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
