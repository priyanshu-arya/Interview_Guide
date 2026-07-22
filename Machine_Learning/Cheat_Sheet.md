# ⚡ Machine Learning Cheat Sheet (Quick Revision)

Designed for rapid review in 5–10 minutes before technical interviews. Covers core mathematical formulas, algorithm comparisons, PyTorch code snippets, and MLOps rules of thumb.

---

## 📐 Core Formulas & Mathematical Equations

| Concept | Equation / Formula | Key Takeaway |
|:---|:---|:---|
| **Mean Squared Error (MSE)** | $\text{MSE} = \frac{1}{n}\sum_{i=1}^n (y_i - \hat{y}_i)^2$ | Penalizes large errors quadratically. Sensitive to outliers. |
| **Binary Cross-Entropy** | $\mathcal{L} = -\frac{1}{n}\sum [y \log \hat{y} + (1-y)\log(1-\hat{y})]$ | Loss function for binary classification probabilities. |
| **Sigmoid Function** | $\sigma(z) = \frac{1}{1 + e^{-z}}$ | Maps $(-\infty, \infty) \to (0, 1)$. Output derivative: $\sigma'(z) = \sigma(z)(1 - \sigma(z))$. |
| **Softmax Function** | $\text{Softmax}(z_i) = \frac{e^{z_i}}{\sum_{j} e^{z_j}}$ | Converts raw logits into probability distribution summing to 1. |
| **L1 Regularization (Lasso)** | $\text{Loss} + \lambda \sum \|w_i\|$ | Enforces **sparsity** (drives inactive feature weights to exact zero). |
| **L2 Regularization (Ridge)** | $\text{Loss} + \lambda \sum w_i^2$ | Enforces **weight decay** (shrinks weights smoothly toward zero). |
| **Self-Attention** | $\text{Attention}(Q,K,V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$ | $\sqrt{d_k}$ prevents vanishing softmax gradients in high dimensions. |
| **LoRA Weight Update** | $W_{\text{new}} = W_0 + \frac{\alpha}{r}(B \cdot A)$ | Trainable $A \in \mathbb{R}^{r \times k}, B \in \mathbb{R}^{d \times r}$ with rank $r \ll \min(d,k)$. |

---

## 📊 High-Value Comparison Matrices

### 1. Classification & Distance Algorithms

| Algorithm | Model Type | Parametric? | Scalability to large $N$ | Outlier Sensitivity | Feature Scaling Required? |
|:---|:---|:---:|:---:|:---:|:---:|
| **Logistic Regression** | Linear Classifier | Yes | Excellent | High | Yes |
| **Support Vector Machine (SVM)** | Margin Classifier | Yes (Kernelized No) | Poor ($O(N^2) - O(N^3)$) | Moderate | Yes |
| **Decision Trees** | Non-Linear Split | No | Good | Low | No |
| **Random Forest** | Parallel Ensemble | No | Excellent | Low | No |
| **XGBoost / LightGBM** | Sequential Ensemble | No | Industry Standard | Low | No |
| **$k$-Nearest Neighbors ($k$-NN)** | Instance-Based | No | Poor at inference | High | Yes |

### 2. Normalization Layers in Deep Learning

| Layer Type | Normalization Dimension | Batch Size Dependent? | Primary Use Case |
|:---|:---|:---:|:---|
| **Batch Normalization** | Across Batch dimension $(N)$ per channel. | **Yes** (Fails if batch size $< 4$) | CNNs, Vision Architectures |
| **Layer Normalization** | Across Feature channels per single sample $(C, H, W)$. | **No** | Transformers, LLMs, NLP |
| **Group Normalization** | Across groups of channels per single sample. | **No** | Small Batch Size Vision Tasks |
| **Instance Normalization**| Across spatial dimensions $(H, W)$ per channel per sample. | **No** | Style Transfer, GANs |

---

## 💻 Essential PyTorch & NumPy Idioms

### Custom PyTorch Dataset & Training Loop
```python
import torch
import torch.nn as nn
from torch.utils.data import Dataset, DataLoader

# 1. Custom Dataset Definition
class CustomDataset(Dataset):
    def __init__(self, X, y):
        self.X = torch.tensor(X, dtype=torch.float32)
        self.y = torch.tensor(y, dtype=torch.float32).unsqueeze(1)

    def __len__(self):
        return len(self.X)

    def __getitem__(self, idx):
        return self.X[idx], self.y[idx]

# 2. PyTorch Training Loop Pattern
def train_one_epoch(model, dataloader, criterion, optimizer, device):
    model.train()
    total_loss = 0.0
    for X_batch, y_batch in dataloader:
        X_batch, y_batch = X_batch.to(device), y_batch.to(device)

        optimizer.zero_grad()            # Clear previous gradients
        outputs = model(X_batch)         # Forward pass
        loss = criterion(outputs, y_batch)# Compute loss
        loss.backward()                  # Backpropagation (autograd)
        torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0) # Gradient clipping
        optimizer.step()                 # Update weights

        total_loss += loss.item() * X_batch.size(0)
    return total_loss / len(dataloader.dataset)
```

### Self-Attention Implementation in PyTorch
```python
import torch
import torch.nn as nn
import math

class ScaledDotProductAttention(nn.Module):
    def __init__(self, d_k):
        super().__init__()
        self.d_k = d_k

    def forward(self, Q, K, V, mask=None):
        # Q, K, V shapes: (batch_size, num_heads, seq_len, d_k)
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        attn_weights = torch.softmax(scores, dim=-1)
        output = torch.matmul(attn_weights, V)
        return output, attn_weights
```

---

## 🛑 Common Pitfalls & Anti-Patterns

1. **Data Leakage in Target Encoding**: Encoding categorical features using target means computed on the combined dataset instead of strictly inside cross-validation folds.
2. **Evaluating Imbalanced Datasets with Accuracy or ROC-AUC Alone**:
   - High True Negative count inflates ROC-AUC even when the model misses key positive events. Use **Precision-Recall AUC (PR-AUC)** when evaluating severe class imbalance.
3. **Evaluating Generation Models with BLEU Alone**: BLEU measures n-gram overlap; it penalizes valid paraphrases and fails to capture semantic correctness. Combine with **ROUGE**, **BERTScore**, or human LLM-as-a-Judge evaluations.
4. **Ignoring Gradient Explosion**: Deep RNNs or unbalanced Transformers can produce infinite gradients (`NaN` loss). Solution: Apply **Gradient Clipping** (`clip_grad_norm_`).

---

## 🧠 Memory Tricks & Quick Notes

- **FAME-DL**: **F**undamentals, **A**rchitectures, **M**LOps, **E**valuation, **D**eep **L**earning (5 core ML interview pillars).
- **L1 vs L2**:
  - **L1 = Lasso = Lines = Corners = Sparse** (Drives weights to zero).
  - **L2 = Ridge = Round = Circles = Smooth** (Shrinks weights uniformly).
- **Precision vs Recall**:
  - **Precision** = $\frac{TP}{TP + FP}$ $\rightarrow$ *"When I predict positive, how often am I RIGHT?"* (Crucial for spam filtering where FP is costly).
  - **Recall** = $\frac{TP}{TP + FN}$ $\rightarrow$ *"Out of all actual positives, how many did I CATCH?"* (Crucial for medical/fraud detection where FN is dangerous).
- **Generative vs Discriminative**:
  - **Generative** learns $P(X, y)$ or $P(X)$ $\rightarrow$ Can generate new samples (Naive Bayes, GMM, VAE, GAN, LLM).
  - **Discriminative** learns conditional $P(y|X)$ $\rightarrow$ Draws decision boundary (Logistic Regression, SVM, Decision Trees, Neural Nets).
