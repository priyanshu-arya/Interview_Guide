# 📖 Machine Learning Interview Guide (Beginner to Advanced)

This guide provides a comprehensive 3-tier deep dive into Machine Learning interview concepts. It covers core statistical fundamentals, classic algorithms, deep neural network mechanisms, and modern foundation model engineering (LLMs/GenAI).

---

## 🟢 Section 1: Beginner Level

### 1. Fundamental Topics & Concepts

#### Supervised vs Unsupervised Learning
- **Supervised Learning**: Models learn from labeled training data containing input features $X$ and target outputs $y$. The goal is to learn a mapping function $f: X \rightarrow y$.
  - *Regression*: Target variable $y$ is continuous (e.g., predicting house prices).
  - *Classification*: Target variable $y$ is discrete/categorical (e.g., email spam detection).
- **Unsupervised Learning**: Models identify hidden patterns, structures, or distributions in unlabeled data $X$.
  - *Clustering*: Grouping similar data points together (e.g., $k$-means, DBSCAN).
  - *Dimensionality Reduction*: Projecting high-dimensional data into fewer dimensions while preserving variance/structure (e.g., PCA, t-SNE).

#### The Bias-Variance Tradeoff
Expected Generalization Error of an ML model decomposes into three distinct components:
$$\text{Expected Error} = \text{Bias}^2 + \text{Variance} + \text{Irreducible Error}$$

```
High Bias (Underfitting)                 Balanced Model                   High Variance (Overfitting)
  [ Simple linear fit ]               [ Smooth non-linear fit ]            [ Squiggly curve fitting noise ]
      ●  ●    ●                                ●  ●    ●                             ●--●    ●
    /        /                               /   /   /                             /    \  /
   /   ●    /                               /   ●   /                             /   ●  \/ 
  /        /                               /       /                             /       \
```

- **High Bias (Underfitting)**: The model makes overly simplistic assumptions about the underlying data structure (e.g., linear model fitted to non-linear data). Training error is high; test error is high.
- **High Variance (Overfitting)**: The model memorizes training noise and specifics rather than generalizable patterns. Training error is extremely low, but test error is high.
- **Irreducible Error ($\sigma^2$)**: Noise inherent in the data collection process that cannot be eliminated by any model.

#### Linear & Logistic Regression Basics
- **Linear Regression**: Predicts continuous target $\hat{y} = w^T X + b$ by minimizing Mean Squared Error (MSE):
  $$J(w, b) = \frac{1}{2n} \sum_{i=1}^n (\hat{y}^{(i)} - y^{(i)})^2$$
- **Logistic Regression**: Used for binary classification by mapping real numbers to probabilities $[0, 1]$ via the Sigmoid function:
  $$\sigma(z) = \frac{1}{1 + e^{-z}}, \quad \hat{y} = \sigma(w^T X + b)$$
  Minimizes Binary Cross-Entropy (Log Loss):
  $$J(w, b) = -\frac{1}{n} \sum_{i=1}^n \left[ y^{(i)} \log(\hat{y}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{y}^{(i)}) \right]$$

#### Feature Scaling: Normalization vs Standardization
- **Min-Max Normalization**: Rescales data to range $[0, 1]$. Sensitive to outliers.
  $$x_{\text{norm}} = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$
- **Standardization (Z-score)**: Transforms features to zero mean ($\mu=0$) and unit variance ($\sigma=1$).
  $$x_{\text{std}} = \frac{x - \mu}{\sigma}$$
- *When required*: Essential for distance-based algorithms ($k$-NN, $k$-means, SVM) and gradient-descent optimized models (Neural Networks, Logistic Regression). Tree-based algorithms (Random Forest, XGBoost) are scale-invariant.

---

### 2. Common Beginner Mistakes

1. **Data Leakage during Preprocessing**: Scaling or imputing the dataset using global metrics calculated across both train and test sets simultaneously.
   - *Correct Approach*: Compute mean/std or fit encoders **only** on the training set, then transform test and validation sets using those learned training parameters.
2. **Confusing Accuracy with Model Quality on Imbalanced Data**: Reporting 99% accuracy on a dataset where 99% of samples belong to the negative class (predicting all zeros achieves 99% accuracy but zero recall).
3. **Evaluating Regression with $R^2$ alone**: Ignoring Mean Absolute Error (MAE) or residual plots, missing heteroscedasticity or non-linear patterns.

---

### 3. Expected Entry-Level Interview Questions

#### Q: How do you split data for cross-validation when dealing with classification tasks?
- **Answer**: Use **Stratified $k$-Fold Cross-Validation**. Standard $k$-fold randomly splits data, which can result in folds with unequal class distributions (or zero instances of minority classes). Stratified $k$-fold ensures that every fold maintains the exact class distribution percentage present in the complete dataset.

#### Q: What happens if your learning rate $\alpha$ in Gradient Descent is too small or too large?
- **Answer**:
  - *Too small*: Gradient updates take tiny steps, causing extremely slow convergence, high computational time, and getting trapped in shallow local minima or saddle points.
  - *Too large*: The optimization steps overshoot the global minimum, causing loss oscillations, numerical divergence (`NaN` values), or failure to converge.

---

## 🟡 Section 2: Intermediate Level

### 1. Frequently Tested Concepts

#### Regularization: L1 (Lasso) vs L2 (Ridge)
Regularization adds a penalty term to the cost function to penalize large model weights and prevent overfitting.

$$\text{Loss}_{\text{total}} = \text{Loss}_{\text{data}} + \lambda \cdot \text{Penalty}$$

| Property | L1 Regularization (Lasso) | L2 Regularization (Ridge) |
|:---|:---|:---|
| **Penalty Term** | $\lambda \sum \|w_i\|$ | $\lambda \sum w_i^2$ |
| **Geometry** | Diamond / Rhombus contour | Circular / Spherical contour |
| **Sparsity** | Drives weights **exactly to zero** (built-in feature selection). | Shrinks weights close to zero, but never exactly zero. |
| **Differentiability** | Non-differentiable at $w_i=0$ (requires subgradients). | Smooth and differentiable everywhere. |
| **Use Case** | High-dimensional data with many redundant/irrelevant features. | When all features contribute small predictive power without multicollinearity. |

```python
# PyTorch weight decay maps directly to L2 regularization in Adam/SGD
import torch.optim as optim

optimizer = optim.Adam(model.parameters(), lr=1e-3, weight_decay=1e-4) # L2 penalty
```

#### Ensemble Methods: Bagging vs Boosting

```mermaid
flowchart LR
    subgraph Bagging["Bagging (Random Forest)"]
        D1[Data] --> B1[Bootstrap Sample 1] --> M1[Tree 1]
        D1 --> B2[Bootstrap Sample 2] --> M2[Tree 2]
        M1 & M2 --> Agg[Average / Majority Vote]
    end

    subgraph Boosting["Boosting (XGBoost / LightGBM)"]
        D2[Data] --> T1[Tree 1] --> R1[Residuals 1] --> T2[Tree 2 (Fits R1)] --> R2[Residuals 2]
    end
```

- **Bagging (Bootstrap Aggregating)**:
  - Trains multiple independent base estimators in parallel on random sub-samples created with replacement (bootstrapping).
  - Main objective: **Reduces Variance** without increasing bias.
  - Example: *Random Forest* (also samples random feature subsets at each node split).
- **Boosting**:
  - Trains base estimators sequentially where each new model targets the residual errors / mistakes of previous estimators.
  - Main objective: **Reduces Bias** (and variance when regularized).
  - Examples: *AdaBoost*, *Gradient Boosting Machines (GBM)*, *XGBoost*, *LightGBM*, *CatBoost*.

#### Deep Learning Building Blocks: Activation Functions & Normalization
- **ReLU**: $f(x) = \max(0, x)$. Efficient, avoids vanishing gradients for positive inputs. *Con*: "Dying ReLU" problem where negative inputs produce zero gradient forever.
- **Leaky ReLU / ELU**: $f(x) = \max(\alpha x, x)$. Allows small negative gradients to flow, preventing dead neurons.
- **Batch Normalization (BatchNorm)**: Normalizes activations across the **mini-batch dimension** for each channel independently:
  $$\hat{x}^{(i)} = \frac{x^{(i)} - \mu_B}{\sqrt{\sigma_B^2 + \epsilon}}, \quad y^{(i)} = \gamma \hat{x}^{(i)} + \beta$$
  Accelerates convergence, acts as mild regularizer, reduces sensitivity to weight initialization.

---

### 2. Coding Expectations (NumPy & PyTorch)

Candidates are expected to write core algorithms from scratch without relying on scikit-learn helper wrappers.

```python
import numpy as np

class LogisticRegressionFromScratch:
    def __init__(self, lr=0.01, num_iters=1000):
        self.lr = lr
        self.num_iters = num_iters
        self.weights = None
        self.bias = None

    def _sigmoid(self, z):
        return 1.0 / (1.0 + np.exp(-np.clip(z, -250, 250)))

    def fit(self, X, y):
        num_samples, num_features = X.shape
        self.weights = np.zeros(num_features)
        self.bias = 0.0

        for _ in range(self.num_iters):
            linear_model = np.dot(X, self.weights) + self.bias
            y_predicted = self._sigmoid(linear_model)

            # Gradient computation
            dw = (1 / num_samples) * np.dot(X.T, (y_predicted - y))
            db = (1 / num_samples) * np.sum(y_predicted - y)

            # Weight update
            self.weights -= self.lr * dw
            self.bias -= self.lr * db

    def predict_proba(self, X):
        return self._sigmoid(np.dot(X, self.weights) + self.bias)

    def predict(self, X, threshold=0.5):
        return (self.predict_proba(X) >= threshold).astype(int)
```

---

### 3. Real Interview Follow-Up Questions

- **"Why does XGBoost outperform traditional GBM in speed and accuracy?"**
  - *Answer*: XGBoost uses **exact/approximate second-order Taylor expansion** for loss minimization (utilizing both gradients and hessians), incorporates explicit $L_1$ and $L_2$ regularization directly into tree split objectives, supports out-of-core block computing, and implements built-in missing value handling (sparsity-aware split finding).
- **"What is the difference between Batch Normalization and Layer Normalization?"**
  - *Answer*: BatchNorm computes mean and variance across the batch dimension $(N)$ for each feature channel separately, making it dependent on batch size (fails for $N<4$). LayerNorm computes mean and variance across all feature channels for **each individual sequence token / sample** independently, making it invariant to batch size and ideal for sequence models (Transformers/RNNs).

---

## 🔴 Section 3: Advanced Level

### 1. Production Concepts & Architectures

#### Modern Transformer Architecture & Self-Attention
Self-Attention allows every sequence token to interact dynamically with all other tokens, calculating context-aware representations without recurrence.

Given input matrix $X \in \mathbb{R}^{T \times d_{\text{model}}}$, we project using learnable weight matrices $W_Q, W_K, W_V$:
$$Q = X W_Q, \quad K = X W_K, \quad V = X W_V$$

$$\text{Attention}(Q, K, V) = \text{softmax}\left( \frac{Q K^T}{\sqrt{d_k}} \right) V$$

```
Query (Q) ──┐
            ├──> Dot Product (Q K^T) ──> Scale ( / √d_k ) ──> Masking (Optional) ──> Softmax ──┐
Key (K)   ──┘                                                                                   ├──> MatMul ──> Output
Value (V) ──────────────────────────────────────────────────────────────────────────────────────┘
```

- **Scaling Factor $\sqrt{d_k}$**: Prevents the dot product from growing extremely large for high embedding dimensions, which would push softmax into regions with near-zero gradients (vanishing gradient during backprop).
- **Multi-Head Attention (MHA)**: Runs attention in parallel across $h$ distinct projection subspaces, allowing the model to jointly attend to information from different representation spaces at different positions.

#### LLM Fine-Tuning: PEFT & LoRA (Low-Rank Adaptation)
Full parameter fine-tuning of 7B+ parameter models requires massive GPU VRAM. **LoRA** decomposes weight updates into two low-rank matrices:

$$\Delta W = B \cdot A, \quad W_{\text{new}} = W_0 + \frac{\alpha}{r} (B \cdot A)$$
where $W_0 \in \mathbb{R}^{d \times k}$, $A \in \mathbb{R}^{r \times k}$, $B \in \mathbb{R}^{d \times r}$, with rank $r \ll \min(d, k)$.

```
   Input X
    │       \
    │        \  Matrix A (r × k)
    │         │
    │  W_0    │  Matrix B (d × r)
    │ (Frozen)│  (Trainable parameters ~0.1%)
    │         /
    ▼        ▼
    Out_0  + Out_LoRA  ===> Combined Output Y
```
- **Benefits**: Reduces trainable parameters by $>99\%$ and memory requirements during backprop by $>3\times$, while maintaining competitive task performance.

---

### 2. Mathematics & Optimization Derivations

#### Derivation of Softmax + Cross-Entropy Loss Gradient
For a single multi-class sample with target $y$ (one-hot vector) and logits $z$:
$$p_i = \text{softmax}(z)_i = \frac{e^{z_i}}{\sum_k e^{z_k}}, \quad \mathcal{L} = -\sum_k y_k \log(p_k)$$

We compute the gradient with respect to logit $z_i$:
$$\frac{\partial \mathcal{L}}{\partial z_i} = -\sum_k y_k \frac{1}{p_k} \frac{\partial p_k}{\partial z_i}$$

Note that $\frac{\partial p_k}{\partial z_i} = p_i(1 - p_i)$ when $k=i$, and $-p_k p_i$ when $k \neq i$.
Substituting these partial derivatives into the equation:
$$\frac{\partial \mathcal{L}}{\partial z_i} = -y_i(1 - p_i) + \sum_{k \neq i} y_k p_i = -y_i + p_i \left( y_i + \sum_{k \neq i} y_k \right)$$
Since $y$ is a one-hot vector, $\sum_k y_k = 1$. Therefore:
$$\frac{\partial \mathcal{L}}{\partial z_i} = p_i - y_i$$

> 💡 The gradient of Cross-Entropy Loss with respect to softmax logits simplifies down to **Predicted Probability minus True One-Hot Vector** ($p - y$)!

---

### 3. Senior Interview Expectations

1. **System Design & Latency Constraints**: Designing real-time recommendations (sub-20ms SLAs) using multi-stage architecture: Candidate Generation (Retrieval via Vector DBs) $\rightarrow$ Heavy Ranking (Transformer/GBDT) $\rightarrow$ Re-Ranking (Diversity, Business Rules).
2. **Handling Concept Drift & Data Shift**:
   - *Covariate Shift*: Input distribution $P(X)$ changes while $P(y|X)$ remains constant.
   - *Prior Probability Shift*: Target distribution $P(y)$ changes while $P(X|y)$ remains constant.
   - *Concept Drift*: Conditional distribution $P(y|X)$ changes over time (e.g., fraud patterns evolving).
3. **MLOps & Evaluation Pipeline**: Monitoring ECE (Expected Calibration Error), latency percentiles (p99), GPU utilization, model rollbacks via canary deployments, and automated retraining triggers based on KS-tests or PSI (Population Stability Index).
