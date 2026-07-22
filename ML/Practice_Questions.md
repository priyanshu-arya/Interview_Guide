# ✏️ Machine Learning (ML) Exhaustive Practice Questions & Problem Bank

This file contains an exhaustive, complete problem bank categorized strictly into **Theory Questions (150+)**, **Coding Questions (100+)**, **MCQs (100+)**, **Scenario Questions (75+)**, **Production Problems (50+)**, **Debugging Problems (50+)**, **Output Prediction Questions (50+)**, **Best Practices Questions (50+)**, **Optimization Questions (50+)**, and **Advanced Questions (50+)**.

---

## 📌 Table of Contents

1. [Theory Questions (150+)](#1-theory-questions)
2. [Coding Questions (100+)](#2-coding-questions)
3. [Multiple Choice Questions (100 MCQs)](#3-multiple-choice-questions)
4. [Scenario Questions (75+)](#4-scenario-questions)
5. [Production Problems (50+)](#5-production-problems)
6. [Debugging Problems (50+)](#6-debugging-problems)
7. [Output Prediction Questions (50+)](#7-output-prediction-questions)
8. [Best Practices Questions (50+)](#8-best-practices-questions)
9. [Optimization Questions (50+)](#9-optimization-questions)
10. [Advanced & Research Questions (50+)](#10-advanced--research-questions)

---

## 1. Theory Questions

### Supervised Learning (40 Questions)
1. What is linear regression? State its assumptions (Linearity, Homoscedasticity, Normality, Independence, No multicollinearity).
2. How do you derive the normal equation $\theta = (X^T X)^{-1} X^T y$ for linear regression?
3. What is the difference between $R^2$ and adjusted $R^2$?
4. What is logistic regression? How does it estimate probabilities using the log-odds transformation?
5. Explain the sigmoid function $\sigma(z) = \frac{1}{1 + e^{-z}}$ and its role in logistic regression.
6. Derive the log loss (cross-entropy) for logistic regression.
7. Compare linear and logistic regression: specify when to use each.
8. What is the decision boundary of logistic regression? Prove it is linear.
9. Explain maximum likelihood estimation (MLE) as applied to logistic regression.
10. What is regularization in logistic regression? Contrast L1 and L2 penalties.
11. How does Support Vector Machine (SVM) find the maximum margin hyperplane?
12. Explain the concept of support vectors and functional vs geometric margins.
13. What is the kernel trick? Provide examples of linear, polynomial, and RBF kernels.
14. When would you choose SVM over Logistic Regression?
15. How does $K$-Nearest Neighbors ($K$-NN) work? What are the effects of small vs large $K$?
16. Why is feature scaling mandatory for $K$-NN and SVM?
17. What is the curse of dimensionality in $K$-NN?
18. Explain Decision Tree splitting criteria: Gini Impurity vs Entropy/Information Gain.
19. How does a decision tree handle both categorical and numerical continuous features?
20. What is pruning in decision trees and why is post-pruning effective against overfitting?
21. Explain the Random Forest algorithm; why does bagging decision trees reduce variance?
22. What is Out-Of-Bag (OOB) error in Random Forest and how does it replace validation sets?
23. How does Random Forest handle missing feature data during training and inference?
24. What is Boosting? Explain the AdaBoost exponential loss weighting mechanism.
25. Explain Gradient Boosting Machines (GBM); what is the "gradient" in functional gradient space?
26. Compare XGBoost, LightGBM, and CatBoost in terms of split finding algorithms and efficiency.
27. What is Stacking in ensemble learning? How do meta-learners prevent data leakage?
28. Define the mathematical bias-variance decomposition of expected mean squared error.
29. How do you diagnose bias and variance using learning curves?
30. What is $K$-Fold Cross-Validation and why is it superior to a single train-test split?
31. Compare Stratified $K$-Fold vs Simple $K$-Fold cross-validation.
32. List metrics for regression: MSE, MAE, RMSE, $R^2$, MAPE. Explain outlier sensitivity for each.
33. List metrics for classification: Precision, Recall, F1, Specificity, ROC-AUC, PR-AUC.
34. How do you interpret a Receiver Operating Characteristic (ROC) curve and AUC score?
35. When should you use Precision-Recall curves over ROC curves?
36. Construct a Confusion Matrix and derive TP, FP, TN, FN metrics.
37. Explain Type I error (False Positive) vs Type II error (False Negative) in hypothesis testing.
38. What is the significance of the $F_\beta$ score? How does $\beta$ balance precision vs recall?
39. How does class imbalance distort evaluation metrics?
40. Explain Micro vs Macro vs Weighted averaging for multi-class classification evaluation.

### Unsupervised Learning & Dimensionality Reduction (25 Questions)
1. What is clustering? List main partitioning, density-based, and hierarchical algorithms.
2. Explain the $K$-Means clustering algorithm step-by-step.
3. How do you choose $K$ in $K$-Means? Explain Elbow method, Silhouette analysis, and Gap statistic.
4. What are the core limitations of $K$-Means? (Sensitivity to initial centroids, spherical cluster assumption).
5. Describe Hierarchical Agglomerative Clustering (Single, Complete, Average, Ward linkage).
6. What is DBSCAN? How does it define Core points, Border points, and Noise points?
7. What are the roles of parameters $\epsilon$ and $\text{minPts}$ in DBSCAN?
8. How do you evaluate clustering quality without ground truth labels? (Silhouette score, Davies-Bouldin index).
9. What is Gaussian Mixture Modeling (GMM)? How does soft assignment differ from $K$-Means hard assignment?
10. Explain the Expectation-Maximization (EM) algorithm for GMMs.
11. What is PCA? Derive how it finds orthogonal axes that maximize projected variance.
12. What are principal components? How do you select the number of components using explained variance ratio?
13. Explain the eigenvalue decomposition of the sample covariance matrix.
14. What is the difference between PCA and Factor Analysis?
15. How does t-SNE work? Why is it primarily used for 2D/3D visual clusters rather than feature reduction?
16. What are the main hyperparameters of t-SNE (perplexity, learning rate) and their effects?
17. How does UMAP differ from t-SNE in preserving global vs local structure?
18. What is Singular Value Decomposition (SVD) and its mathematical relationship to PCA?
19. How does an Autoencoder perform non-linear dimensionality reduction?
20. How can Autoencoders be used for anomaly detection using reconstruction error thresholding?
21. What is Manifold Learning? Explain Isomap and Locally Linear Embedding (LLE).
22. What is Independent Component Analysis (ICA) and the blind source separation problem?
23. How do you compute distance metrics for categorical features in clustering? (Gower's distance, Hamming distance).
24. What is the "Elbow" method in $K$-Means? Why does it fail when cluster inertia decreases smoothly?
25. Explain the curse of dimensionality for distance-based clustering metrics in high dimensions.

### Deep Learning (40 Questions)
1. What is a neural network? Describe the Single-Layer Perceptron and decision boundary.
2. Activation functions: Sigmoid, Tanh, ReLU, LeakyReLU, ELU, GELU – write formulas, output ranges, pros, cons.
3. Why are non-linear activation functions necessary in multi-layer neural networks?
4. Explain standard Gradient Descent for neural network parameter updates.
5. Optimization variants: SGD, SGD with Momentum, Nesterov Accelerated Gradient.
6. Adaptive optimizers: AdaGrad, RMSprop, Adam, AdamW – how do they adapt per-parameter learning rates?
7. Explain weight initialization strategies: Xavier (Glorot) vs He (Kaiming) initialization.
8. What is Batch Normalization? How is it calculated during training vs inference?
9. Explain Layer Normalization: where is it used (Transformers) and why is it preferred over BatchNorm for NLP?
10. Contrast Instance Normalization and Group Normalization use cases.
11. What is Dropout? Explain inverted dropout scaling and regularization mechanics.
12. How does Early Stopping act as a regularizer during training?
13. Derive Backpropagation using the multivariate chain rule on a computational graph.
14. What is Gradient Clipping (norm vs value clipping) and why is it essential for sequence training?
15. Convolutional Neural Networks: explain convolution operation, kernel, stride, and padding.
16. What is the purpose of Pooling layers (Max-pooling vs Average-pooling)?
17. Describe classic CNN architectures: LeNet, AlexNet, VGG, ResNet, EfficientNet.
18. Why do Residual Networks (ResNet) enable successful training of 100+ layer deep networks?
19. What is a $1\times 1$ Convolution filter and why is it used for cross-channel feature pooling?
20. Recurrent Neural Networks: unrolled forward pass and Backpropagation Through Time (BPTT).
21. Why do vanilla RNNs suffer from vanishing and exploding gradients over long sequences?
22. LSTM architecture: detail Cell State $C_t$, Forget Gate, Input Gate, Output Gate.
23. Contrast GRU (Reset & Update gates) vs LSTM (3 gates + cell state).
24. What is a Bidirectional RNN/LSTM and when is it applicable?
25. Sequence-to-Sequence (Seq2Seq) Encoder-Decoder architecture.
26. What is Attention? Formulate Scaled Dot-Product Attention.
27. Define Queries ($Q$), Keys ($K$), and Values ($V$) in attention mechanisms.
28. Explain Multi-Head Attention and subspace projection.
29. Transformer Architecture: detail Encoder block vs Decoder block.
30. Positional Encodings: why are they required? Contrast sinusoidal vs learned vs RoPE embeddings.
31. Vision Transformers (ViT): how do they process image patches as sequence tokens?
32. How to fine-tune a pre-trained Vision Transformer / ResNet for downstream classification?
33. What is Transfer Learning? When to freeze early feature layers vs fine-tune all weights?
34. Compare the inductive biases of CNNs (locality, translation invariance) vs Transformers.
35. Generative Adversarial Networks (GANs): Generator vs Discriminator minimax optimization.
36. Variational Autoencoders (VAEs): Encoder, Decoder, ELBO loss, KL divergence regularization.
37. Diffusion Models: Forward noising process vs Reverse score-based denoising process.
38. What is a Normalizing Flow model?
39. Explain Autoencoders for unsupervised representation learning.
40. What is the Manifold Hypothesis and how does it relate to deep representation learning?

### Ensemble, Evaluation, Preprocessing, LLMs & Math (45 Questions)
1. Bagging: why does averaging independent bootstrap trees reduce variance without altering bias?
2. Boosting: how does sequential fitting of weak learners on residual errors reduce bias?
3. XGBoost: detail the regularized objective function $\mathcal{L} + \Omega(f)$ and Taylor expansion approximation.
4. How does XGBoost handle missing values natively using sparsity-aware split finding?
5. LightGBM: leaf-wise (best-first) tree growth vs level-wise; explain Gradient-based One-Side Sampling (GOSS).
6. CatBoost: ordered boosting and online target encoding for categorical features.
7. What is Stacking and how to construct out-of-fold predictions for stage-2 meta-models?
8. How to interpret feature importance metrics: Gini impurity reduction vs Permutation Importance.
9. Key hyperparameter tuning strategies for XGBoost / LightGBM.
10. How to prevent overfitting in boosting models (learning rate shrinkage, subsample, colsample_bytree).
11. Difference between Validation set and Test set operations.
12. Choosing between $K$-Fold cross-validation vs Hold-out validation.
13. Explain Time-Series Cross-Validation (rolling window / expanding origin split).
14. What is the severe look-ahead bias created by randomly shuffling time-series data?
15. Model calibration: Reliability diagrams, Expected Calibration Error (ECE), Platt Scaling.
16. Log loss analysis: why does cross-entropy heavily penalize confident incorrect predictions?
17. Brier score metric for evaluating probabilistic predictions.
18. Internal vs External validation techniques in ML pipelines.
19. Interpreting Learning Curves: diagnosing high bias vs high variance visually.
20. Validation curves: plotting train vs val score against hyperparameter range.
21. Why split data prior to preprocessing transformations to avoid data leakage?
22. Categorical feature encoding choices: One-Hot, Ordinal, Target Encoding, Frequency Encoding.
23. Handling high-cardinality features without sparse matrix explosion.
24. Outlier treatment methods: IQR clipping, Z-score truncation, Winsorization.
25. Feature binning / discretization use cases and pitfalls.
26. Feature crossing: constructing non-linear feature interactions manually.
27. Encoding cyclic temporal features (hour of day, month) using Sine and Cosine transformations.
28. What is the Hashing Trick (Feature Hashing) for large text spaces?
29. Feature extraction from unstructured text: Bag-of-Words, TF-IDF, N-grams, Embeddings.
30. Pre-training objectives for LLMs: Causal Language Modeling (GPT) vs Masked Language Modeling (BERT).
31. Explain Instruction Tuning and Supervised Fine-Tuning (SFT) for LLMs.
32. Detail RLHF (Reinforcement Learning from Human Feedback) with Reward Modeling and PPO.
33. Detail Retrieval-Augmented Generation (RAG) end-to-end pipeline.
34. Document chunking strategies (fixed size, semantic splitting) and impact on RAG retrieval accuracy.
35. Vector databases: Approximate Nearest Neighbor (ANN) search via HNSW graphs.
36. LLM output evaluation frameworks: Ragas, TruLens, G-Eval, Human Preference.
37. Prompt Engineering techniques: Few-shot, Chain-of-Thought (CoT), ReAct prompting.
38. Fine-tuning vs In-Context Learning (ICL) trade-offs.
39. Probability Chain Rule formulation.
40. Bayes' Theorem derivation and real-world diagnostic test application.
41. Probability Mass Function (PMF) vs Probability Density Function (PDF).
42. Central Limit Theorem (CLT) formulation and significance in statistics.
43. Mathematical definitions of Information Entropy, Cross-Entropy, and KL Divergence.

---

## 2. Coding Questions

### Easy Coding Questions (20 Problems)
1. **MSE Calculation**: Write a NumPy function `compute_mse(y_true, y_pred)` that returns mean squared error.
2. **Min-Max Scaling**: Implement min-max normalization for a 1D NumPy array to scale values between $[0, 1]$.
3. **One-Hot Encoding**: Write a function to one-hot encode a list of categorical integer labels.
4. **Train-Test Split**: Implement a custom `train_test_split(X, y, test_size=0.2)` using NumPy indices without sklearn.
5. **Accuracy Score**: Write a function `calculate_accuracy(confusion_matrix)` to compute accuracy from a $2\times2$ matrix.
6. **Numerically Stable Softmax**: Implement a function `softmax(logits)` using `np.exp(z - np.max(z))`.
7. **Sigmoid Function**: Implement element-wise `sigmoid(z)` for a vector or matrix.
8. **Gini Impurity**: Write a function `gini_impurity(y)` for a set of class labels.
9. **Entropy Calculation**: Implement `binary_entropy(y)` using base-2 logarithm.
10. **Stratified Split**: Write a function to perform stratified train-test splitting for binary labels.
11. **Z-Score Normalization**: Standardize a 2D NumPy array column-wise to have $\mu=0, \sigma=1$.
12. **Covariance Matrix**: Write a function `compute_covariance_matrix(X)` from scratch.
13. **Euclidean Distance**: Compute pairwise Euclidean distance matrix between two sets of vectors.
14. **Majority Class Finder**: Write a Python function to return the most frequent element in a list.
15. **Log Loss Function**: Write `compute_log_loss(y_true, y_prob)` with numeric clipping (`eps=1e-15`).
16. **K-Fold Index Generator**: Write a function `generate_kfold_indices(n_samples, k=5)`.
17. **Synthetic Classification Dataset**: Generate a binary classification dataset using `np.random.randn`.
18. **Precision, Recall, F1**: Implement `eval_metrics(y_true, y_pred)` returning precision, recall, and F1.
19. **Synchronized Array Shuffle**: Write a function `shuffle_in_unison(a, b)` for features and labels.
20. **Threshold binarizer**: Convert continuous predicted probabilities to binary predictions using threshold $T$.

### Medium & Hard Coding Questions (70 Problems)
1. **Linear Regression SGD**: Implement `LinearRegressionSGD(lr, epochs)` from scratch in NumPy.
2. **Logistic Regression Gradient Descent**: Build `LogisticRegression(lr, n_iters)` with binary cross-entropy loss.
3. **K-NN Classifier**: Implement custom `KNeighborsClassifier(k)` with `fit(X, y)` and `predict(X)`.
4. **Decision Tree Classifier**: Implement a recursive `DecisionTree` with Gini impurity splitting.
5. **Random Forest Classifier**: Build a `RandomForest` by ensembling bagged decision trees.
6. **AdaBoost Classifier**: Implement AdaBoost from scratch using decision stumps ($1$-level decision trees).
7. **NumPy 2-Layer MLP**: Implement forward pass and backprop for a 2-layer neural network in pure NumPy.
8. **Adam Optimizer**: Code the step update equations for the Adam optimizer class in Python.
9. **BatchNorm Layer**: Write a Python class `BatchNorm1D` implementing training and evaluation forward passes.
10. **Inverted Dropout Layer**: Implement `Dropout(p)` forward pass with inverted scaling during training.
11. **Conv2D Forward Pass**: Code a 2D Convolutional layer forward pass using `im2col` matrix multiplication.
12. **Max-Pooling Forward/Backward**: Write forward and backward passes for a $2\times2$ Max-Pooling layer.
13. **Vanilla RNN Cell**: Code forward and BPTT backward pass for a vanilla RNN cell.
14. **LSTM Cell Forward Pass**: Implement an LSTM cell forward pass with forget, input, output, and candidate gates.
15. **Scaled Self-Attention**: Write a PyTorch function `scaled_dot_product_attention(Q, K, V, mask)`.
16. **Transformer Encoder Block**: Build a single Transformer Encoder layer in PyTorch (Attention + FFN + LayerNorm).
17. **K-Means Clustering**: Implement `KMeans(k, max_iters)` with random initialization and centroid updates.
18. **DBSCAN Algorithm**: Build a DBSCAN density-based clustering class in NumPy.
19. **PCA via SVD**: Write `PCA(n_components)` using `np.linalg.svd` to project data onto principal axes.
20. **PyTorch Autoencoder**: Implement an image autoencoder in PyTorch with encoder and decoder blocks.
21. **PyTorch CNN Training Loop**: Write a full PyTorch training loop (loader, model, criterion, optimizer, eval).
22. **ResNet Fine-Tuning**: Load pre-trained ResNet-18 in PyTorch, replace final FC layer, and freeze base weights.
23. **Hugging Face Text Classifier**: Write code to fine-tune `bert-base-uncased` for sequence classification.
24. **PyTorch Custom Dataset**: Create a `CustomTextDataset(torch.utils.data.Dataset)` with tokenizer padding.
25. **Batch Data Generator**: Write a Python generator yielding mini-batches from a large CSV file.
26. **Softmax + Cross-Entropy Gradient**: Implement the analytical gradient $(p - y)$ for combined Softmax+CE.
27. **ReLU Derivative**: Write element-wise forward and backward functions for LeakyReLU.
28. **Early Stopping Callback**: Write a PyTorch early stopping helper tracking validation loss with patience.
29. **Cross-Validation Scorer**: Implement a custom `cross_val_score(model, X, y, cv=5)` loop.
30. **Grid Search CV**: Code a basic grid search optimizer iterating over hyperparameter dictionaries.
31. **PyTorch GAN for MNIST**: Code Generator and Discriminator PyTorch modules with adversarial loss loop.
32. **PyTorch VAE**: Train a Variational Autoencoder on Fashion-MNIST using BCELoss + KL Divergence.
33. **ROC-AUC from Scratch**: Write a function to compute ROC-AUC score without sklearn using trapezoidal integration.
34. **Collaborative Filtering**: Implement matrix factorization using SGD for user-item rating predictions.
35. **Class-Weighted Loss**: Implement custom binary cross-entropy with positive weight factor $W_p$.
36. **SMOTE Oversampling**: Code a simplified SMOTE algorithm interpolating between minority class neighbors.
37. **Mutual Information Feature Selection**: Implement feature selection using mutual information from scratch.
38. **Focal Loss in PyTorch**: Write a PyTorch custom loss class for `FocalLoss(alpha, gamma)`.
39. **Simple Text Tokenizer**: Write a regex-based tokenizer returning token IDs and vocabulary mappings.
40. **Bag-of-Words Vectorizer**: Build a `CountVectorizer` class with `fit_transform()` and `transform()`.
41. **TF-IDF Transformer**: Implement TF-IDF calculation given term frequencies and document count $N$.
42. **L2 Regularized Logistic Regression**: Add L2 weight decay penalty directly to logistic regression SGD updates.
43. **Polynomial Feature Expansion**: Create degree-2 polynomial interactions $[x_1, x_2] \to [x_1, x_2, x_1^2, x_1 x_2, x_2^2]$.
44. **IQR Outlier Detector**: Write a function returning boolean mask for outliers beyond $1.5 \times \text{IQR}$.
45. **Multi-Class One-vs-Rest**: Implement a multi-class wrapper training $K$ binary logistic regression models.
46. **Silhouette Score Calculator**: Write a function computing mean silhouette coefficient for clusters.
47. **Perceptron Classifier**: Implement Rosenblatt's Perceptron learning algorithm.
48. **Transformer Encoder Layer (Pure NumPy)**: Build a multi-head self-attention module using only NumPy matrix operations.
49. **Mini-Autograd Engine**: Implement a basic autograd engine with scalar `Value` objects supporting `+`, `*`, `relu`, `backward()`.
50. **Gradient Clipping Operator**: Implement norm clipping for a list of PyTorch parameter tensors.
51. **Q-Learning Agent**: Build a tabular $Q$-learning agent with $\epsilon$-greedy exploration for a grid world.
52. **REINFORCE Policy Gradient**: Implement policy gradient RL training on CartPole environment.
53. **DDPM Training Step**: Write a 2D diffusion training step adding noise and predicting noise with an MLP.
54. **Distributed PyTorch DDP**: Write a distributed data parallel script using `torch.distributed.init_process_group`.
55. **GCN Layer PyTorch**: Implement a Graph Convolutional Network layer ($A_{\text{norm}} X W$).
56. **Custom CUDA Kernel Wrapper**: Write PyTorch C++ extension binding for element-wise operation.
57. **LoRA Layer PyTorch**: Implement a `LoRALinear` module injecting trainable rank-$r$ matrices $A$ and $B$.
58. **Byte-Pair Encoding (BPE) Tokenizer**: Write a BPE algorithm learning pair merge rules from text corpus.
59. **Online SGD Logistic Regression**: Implement streaming online learning model updating on individual data rows.
60. **KS-Test Concept Drift**: Write a monitoring script performing Kolmogorov-Smirnov test on streaming feature windows.
61. **Full Computation Graph Autodiff**: Implement automatic differentiation supporting matrix multiplication and backprop.
62. **WGAN-GP Training Loop**: Code Wasserstein GAN with Gradient Penalty ($\lambda (\Vert\nabla_{\hat{x}} D(\hat{x})\Vert_2 - 1)^2$).
63. **RAG Pipeline Integration**: End-to-end RAG script integrating SentenceTransformers, FAISS index, and HuggingFace LLM.
64. **DQN Agent with Replay Buffer**: Code Deep $Q$-Network with target network updates and experience replay buffer.
65. **DDPM Denoising Sampling**: Write the full sampling loop generating data samples from Gaussian noise.
66. **Mixture-of-Experts Layer**: Code an MoE layer with softmax Top-2 gating router and expert linear networks.
67. **Mini Tensor Deep Learning Framework**: Write a lightweight PyTorch alternative with multi-dimensional `Tensor` and autograd.
68. **Unstructured Weight Pruning**: Implement magnitude-based weight pruning zeroing smallest $K\%$ parameters and fine-tuning.

---

## 3. Multiple Choice Questions

### 100 Mandatory MCQs with Answers

1. Which of the following is NOT an activation function?  
   a) ReLU b) Leaky ReLU c) MaxPool d) Tanh  
   **Answer:** c) MaxPool
2. L1 regularization tends to produce:  
   a) Dense weights b) Sparse weights c) No change d) Negative weights  
   **Answer:** b) Sparse weights
3. Which metric is most appropriate for a severely imbalanced dataset?  
   a) Accuracy b) Precision c) F1-score d) ROC-AUC  
   **Answer:** c) F1-score
4. What does the ROC curve plot?  
   a) Precision vs Recall b) TPR vs FPR c) Sensitivity vs Specificity d) Accuracy vs Threshold  
   **Answer:** b) TPR vs FPR
5. The vanishing gradient problem is most heavily associated with:  
   a) ReLU b) Tanh / Sigmoid c) Leaky ReLU d) ELU  
   **Answer:** b) Tanh / Sigmoid
6. Which optimizer maintains per-parameter adaptive learning rates?  
   a) SGD b) Momentum c) Adam d) Vanilla GD  
   **Answer:** c) Adam
7. A model with high variance and low bias is likely:  
   a) Underfitting b) Overfitting c) Optimal d) Linear  
   **Answer:** b) Overfitting
8. In $K$-Means, the number of clusters can be chosen using:  
   a) Cross-entropy b) Elbow method c) $R^2$ d) AUC  
   **Answer:** b) Elbow method
9. What is a core limitation of $K$-Means?  
   a) Sensitive to outliers b) Cannot handle categorical data natively c) Assumes spherical clusters d) All of the above  
   **Answer:** d) All of the above
10. Dropout works by:  
    a) Adding noise to inputs b) Randomly dropping neurons during training c) Increasing learning rate d) Early stopping  
    **Answer:** b) Randomly dropping neurons during training
11. The primary purpose of a validation set is:  
    a) Final model evaluation b) Hyperparameter tuning & model selection c) Weight updates d) None  
    **Answer:** b) Hyperparameter tuning & model selection
12. Which of the following is an ensemble bagging method?  
    a) Decision Tree b) Random Forest c) Logistic Regression d) $K$-NN  
    **Answer:** b) Random Forest
13. In a CNN, pooling operations help to:  
    a) Increase parameters b) Reduce spatial dimensions c) Increase overfitting d) None  
    **Answer:** b) Reduce spatial dimensions
14. What is the key structural advantage of ResNet?  
    a) Fewer parameters b) Avoid vanishing gradients via skip connections c) Faster training d) Eliminates activation functions  
    **Answer:** b) Avoid vanishing gradients via skip connections
15. The standard loss function for binary classification is:  
    a) MSE b) Binary Cross-Entropy c) Hinge Loss d) MAE  
    **Answer:** b) Binary Cross-Entropy
16. Which of these is NOT a boosting algorithm?  
    a) AdaBoost b) Gradient Boosting c) XGBoost d) Random Forest  
    **Answer:** d) Random Forest
17. In Support Vector Machines, support vectors are:  
    a) All data points b) Points far from margin c) Points lying on or violating margin boundaries d) Outliers  
    **Answer:** c) Points lying on or violating margin boundaries
18. What does the learning rate control?  
    a) Number of epochs b) Step size in weight updates c) Batch size d) Model capacity  
    **Answer:** b) Step size in weight updates
19. Which technique directly mitigates overfitting?  
    a) Adding more features b) Increasing model depth c) Dropout regularization d) Higher learning rate  
    **Answer:** c) Dropout regularization
20. What is the output range of ROC-AUC?  
    a) $-1$ to $1$ b) $0$ to $1$ c) $0$ to $\infty$ d) $-\infty$ to $\infty$  
    **Answer:** b) $0$ to $1$
21. Which of the following is TRUE about PCA?  
    a) Supervised technique b) Maximizes variance of projections c) Non-linear algorithm d) Works on categorical data  
    **Answer:** b) Maximizes variance of projections
22. What is the main difference between bagging and boosting?  
    a) Bagging reduces bias, boosting reduces variance b) Bagging reduces variance, boosting reduces bias c) No difference d) Bagging is sequential  
    **Answer:** b) Bagging reduces variance, boosting reduces bias
23. Which of the following is a hyperparameter?  
    a) Weights of a neural network b) Learning rate c) Coefficients of linear regression d) Biases  
    **Answer:** b) Learning rate
24. In logistic regression, the raw output after sigmoid is interpreted as:  
    a) Class label b) Probability c) Distance d) Log-odds  
    **Answer:** b) Probability
25. Why is ReLU preferred over Sigmoid in deep hidden layers?  
    a) Outputs probabilities b) Mitigates vanishing gradient c) Is bounded d) Is linear  
    **Answer:** b) Mitigates vanishing gradient
26. What is a correct statement about Gradient Descent?  
    a) Guarantees global minimum for any function b) Updates parameters in direction opposite to gradient c) Always converges instantly d) Requires no loss function  
    **Answer:** b) Updates parameters in direction opposite to gradient
27. The "curse of dimensionality" refers to:  
    a) Excessively large sample size b) Data sparsity in high dimensions c) Small feature sets d) Low computational speed  
    **Answer:** b) Data sparsity in high dimensions
28. Which of the following models is non-parametric?  
    a) Linear regression b) Logistic regression c) $K$-NN d) SVM with linear kernel  
    **Answer:** c) $K$-NN
29. In a decision tree, a leaf node represents:  
    a) A split condition b) Final class label / value c) A feature name d) None  
    **Answer:** b) Final class label / value
30. The component of an LSTM that controls what information to discard from cell state:  
    a) Input gate b) Forget gate c) Output gate d) Candidate gate  
    **Answer:** b) Forget gate
31. What does the BLEU score measure in machine translation?  
    a) Sentence fluency b) $N$-gram overlap with reference translations c) Semantic embeddings d) Perplexity  
    **Answer:** b) $N$-gram overlap with reference translations
32. Which of the following is NOT a step in an ML pipeline?  
    a) Data ingestion b) Model deployment c) Corporate strategy formulation d) Feature engineering  
    **Answer:** c) Corporate strategy formulation
33. A confusion matrix is used to:  
    a) Tune hyperparameters b) Visualize classification performance breakdown c) Eliminate missing data d) Speed up training  
    **Answer:** b) Visualize classification performance breakdown
34. The kernel trick is utilized in:  
    a) Linear regression b) SVM c) Decision trees d) $K$-NN  
    **Answer:** b) SVM
35. The primary goal of regularization is to:  
    a) Increase model capacity b) Minimize training error c) Prevent overfitting d) Accelerate data loading  
    **Answer:** c) Prevent overfitting
36. A model performing exceptionally well on train data but poorly on test data is:  
    a) Underfitting b) Overfitting c) Well-calibrated d) Robust  
    **Answer:** b) Overfitting
37. Which of the following is an unsupervised learning task?  
    a) Linear regression b) Logistic classification c) Clustering d) Reinforcement learning  
    **Answer:** c) Clustering
38. The Adam optimizer combines ideas from:  
    a) SGD and Nesterov b) Momentum and RMSprop c) AdaGrad and SGD d) None  
    **Answer:** b) Momentum and RMSprop
39. What is Early Stopping?  
    a) Stop training when train loss is 0 b) Halt training when validation loss stops improving c) Stop after fixed epochs d) Stop when accuracy reaches 100%  
    **Answer:** b) Halt training when validation loss stops improving
40. Which algorithm is best suited for reconstruction-based anomaly detection?  
    a) Linear regression b) Autoencoder c) Decision tree d) $K$-Means  
    **Answer:** b) Autoencoder
41. In a neural network, what is a "Dead ReLU"?  
    a) Output is always 0 b) Output is negative c) Output is infinitely large d) Output is random  
    **Answer:** a) Output is always 0
42. The mathematical function $\max(0, x)$ represents:  
    a) Sigmoid b) Tanh c) ReLU d) Softmax  
    **Answer:** c) ReLU
43. Primary advantage of using a pre-trained model:  
    a) Guarantees 100% accuracy b) Reduces training time and labeled data requirements c) Eliminates validation set d) Removes need for fine-tuning  
    **Answer:** b) Reduces training time and labeled data requirements
44. In Transformer architectures, "self-attention" means:  
    a) Attending to external database b) Each token attends to all tokens within the same sequence c) Attending only to target labels d) No token interaction  
    **Answer:** b) Each token attends to all tokens within the same sequence
45. Which of the following architectures can process sequential data?  
    a) 1D CNN b) RNN c) Transformer d) All of the above  
    **Answer:** d) All of the above
46. A feature has high correlation with target but is missing 80% of values. What to do?  
    a) Drop feature b) Impute values and create missing indicator column c) Ignore missingness d) Use raw  
    **Answer:** b) Impute values and create missing indicator column
47. How does mini-batch size affect training?  
    a) Larger batch size always improves generalization b) Smaller batch size adds noise acting as a regularizer c) Batch size has zero effect d) None  
    **Answer:** b) Smaller batch size adds noise acting as a regularizer
48. Which of the following is a generative model?  
    a) Logistic regression b) SVM c) GAN d) Decision tree  
    **Answer:** c) GAN
49. In Reinforcement Learning, the $Q$-function estimates:  
    a) Policy probability b) Expected cumulative future reward for taking action $a$ in state $s$ c) Value of state d) Transition dynamics  
    **Answer:** b) Expected cumulative future reward for taking action $a$ in state $s$
50. Purpose of bottleneck layer in Autoencoders:  
    a) Increase parameter count b) Force compressed latent representation c) Add non-linearity d) None  
    **Answer:** b) Force compressed latent representation
51. Correct sequence in an ML workflow:  
    a) Training $\to$ Data Cleaning $\to$ EDA b) Data Cleaning $\to$ EDA $\to$ Model Training c) EDA $\to$ Training $\to$ Data Cleaning d) Deployment $\to$ EDA  
    **Answer:** b) Data Cleaning $\to$ EDA $\to$ Model Training
52. Difference between $R^2$ and MSE:  
    a) No difference b) $R^2$ is scale-independent, MSE is in squared target units c) MSE is scale-independent d) $R^2$ measures classification error  
    **Answer:** b) $R^2$ is scale-independent, MSE is in squared target units
53. A model with 0 bias and 0 variance in real-world ML is:  
    a) Overfit b) Underfit c) Impossible in practice d) Standard  
    **Answer:** c) Impossible in practice
54. Which of the following is NOT a tree ensemble method?  
    a) Random Forest b) Gradient Boosting c) SVM d) XGBoost  
    **Answer:** c) SVM
55. When should feature normalization be applied?  
    a) Only for neural networks b) For distance-based and gradient-based models c) For tree models d) Never  
    **Answer:** b) For distance-based and gradient-based models
56. The term "Epoch" in deep learning means:  
    a) One forward pass b) One backward update c) One complete pass through the entire training dataset d) One mini-batch  
    **Answer:** c) One complete pass through the entire training dataset
57. Which of the following is NOT an unsupervised method?  
    a) $K$-Means b) PCA c) Linear Regression d) DBSCAN  
    **Answer:** c) Linear Regression
58. In Early Stopping, "patience" refers to:  
    a) Number of epochs to wait without validation improvement before halting b) Learning rate decay rate c) Batch size d) Number of folds  
    **Answer:** a) Number of epochs to wait without validation improvement before halting
59. Model Bias refers to:  
    a) Error from variance b) Error from overly simplistic assumptions in learning algorithm c) Noise d) Overfitting  
    **Answer:** b) Error from overly simplistic assumptions in learning algorithm
60. Data Augmentation refers to:  
    a) Adding new feature columns b) Generating synthetic training samples via transformations c) Outlier removal d) Dimensionality reduction  
    **Answer:** b) Generating synthetic training samples via transformations
61. Common metric for evaluating regression models:  
    a) F1-score b) AUC c) RMSE d) Accuracy  
    **Answer:** c) RMSE
62. Key computational advantage of LightGBM over XGBoost:  
    a) Higher accuracy b) Leaf-wise tree growth and GOSS for faster speed and lower memory c) Simpler math d) None  
    **Answer:** b) Leaf-wise tree growth and GOSS for faster speed and lower memory
63. In Random Forest, "Bootstrap" sampling means:  
    a) Sampling features without replacement b) Sampling data points with replacement for each tree c) Out-of-bag validation d) None  
    **Answer:** b) Sampling data points with replacement for each tree
64. Which setting causes high model variance?  
    a) High regularization b) Deep unpruned decision tree c) Linear model d) Massive training set  
    **Answer:** b) Deep unpruned decision tree
65. Purpose of a Loss Function:  
    a) Measure test accuracy b) Guide weight optimization during training c) Increase overfitting d) Speed up data loading  
    **Answer:** b) Guide weight optimization during training
66. Difference between Softmax and Sigmoid:  
    a) Sigmoid for multi-class, Softmax for binary b) Sigmoid for binary classification, Softmax for multi-class c) No difference d) Softmax for regression  
    **Answer:** b) Sigmoid for binary classification, Softmax for multi-class
67. Role of Embedding layers:  
    a) Reduce overfitting b) Project sparse discrete IDs into dense continuous vector space c) Increase sparsity d) None  
    **Answer:** b) Project sparse discrete IDs into dense continuous vector space
68. Which algorithm is deterministic given fixed inputs?  
    a) $K$-Means with random init b) PCA c) Random Forest d) Mini-batch SGD  
    **Answer:** b) PCA
69. What is Gradient Accumulation?  
    a) Summing gradients over multiple mini-batches before executing `optimizer.step()` b) Increasing learning rate c) Reducing batch size d) None  
    **Answer:** a) Summing gradients over multiple mini-batches before executing `optimizer.step()`
70. Typical activation inside Transformer FFN layers:  
    a) Sigmoid b) Tanh c) GELU or ReLU d) Softmax  
    **Answer:** c) GELU or ReLU
71. What is Perplexity in language modeling?  
    a) Exponentiated cross-entropy loss $e^{\mathcal{L}}$ b) Parameter count c) Training accuracy d) BLEU score  
    **Answer:** a) Exponentiated cross-entropy loss $e^{\mathcal{L}}$
72. Function of Query ($Q$) in self-attention:  
    a) Store context b) Represent current token asking what other tokens to attend to c) Provide target labels d) Normalize outputs  
    **Answer:** b) Represent current token asking what other tokens to attend to
73. Which of the following is NOT typically used as a hidden layer activation?  
    a) ReLU b) Tanh c) Softmax d) GELU  
    **Answer:** c) Softmax
74. Ensemble learning means:  
    a) Using 1 large neural net b) Combining predictions from multiple models c) Data cleaning d) Feature scaling  
    **Answer:** b) Combining predictions from multiple models
75. In RL, discount factor $\gamma \in [0, 1]$ weighs:  
    a) Future rewards relative to immediate rewards b) Exploration rate c) Overfitting penalty d) Learning rate  
    **Answer:** a) Future rewards relative to immediate rewards
76. Generator vs Discriminator in GANs:  
    a) Generator classifies, Discriminator creates b) Generator creates fake samples, Discriminator classifies real vs fake c) Both generate d) None  
    **Answer:** b) Generator creates fake samples, Discriminator classifies real vs fake
77. Concept Drift refers to:  
    a) Changes in model code b) Changes in statistical distribution of target variable over time $P(Y|X)$ c) Gradient vanishing d) Data corruption  
    **Answer:** b) Changes in statistical distribution of target variable over time $P(Y|X)$
78. Characteristic of a strong feature:  
    a) High correlation with target variable b) High noise c) 90% missing values d) Zero variance  
    **Answer:** a) High correlation with target variable
79. Skip connections in U-Net serve to:  
    a) Reduce parameter count b) Pass high-resolution spatial feature maps directly to decoder layers c) Speed up GPU d) None  
    **Answer:** b) Pass high-resolution spatial feature maps directly to decoder layers
80. The term "Logits" refers to:  
    a) Probabilities b) Un-normalized raw output scores before Softmax/Sigmoid c) Loss values d) Dataset rows  
    **Answer:** b) Un-normalized raw output scores before Softmax/Sigmoid
81. An Underfit model displays:  
    a) High training error and high test error b) Low training error and high test error c) Low training error and low test error d) High precision  
    **Answer:** a) High training error and high test error
82. Purpose of Temperature $T$ in Softmax $p_i = \frac{e^{z_i/T}}{\sum e^{z_j/T}}$:  
    a) Smooth/soften output probability distribution b) Speed up training c) Reduce memory d) None  
    **Answer:** a) Smooth/soften output probability distribution
83. Why apply Position-wise FFN independently in Transformers?  
    a) Attention already mixed spatial information across sequence tokens b) Speed up GPU c) Reduce memory d) None  
    **Answer:** a) Attention already mixed spatial information across sequence tokens
84. Which of these is a non-linear activation?  
    a) Linear $f(x)=x$ b) Identity c) ReLU d) None  
    **Answer:** c) ReLU
85. A model with ROC-AUC of 1.0 on test set indicates:  
    a) Perfect classifier ranking (or severe data leakage) b) Underfitting c) Random guessing d) High bias  
    **Answer:** a) Perfect classifier ranking (or severe data leakage)
86. Advantage of t-SNE over linear PCA:  
    a) Preserves local non-linear neighborhood relationships b) Preserves global distances c) Faster d) Deterministic  
    **Answer:** a) Preserves local non-linear neighborhood relationships
87. Hyperparameter of a Decision Tree:  
    a) Split values b) Max depth c) Leaf values d) Feature weights  
    **Answer:** b) Max depth
88. Role of `random_state` / seed setting:  
    a) Speed up training b) Ensure exact operational reproducibility c) Scale features d) Tune parameters  
    **Answer:** b) Ensure exact operational reproducibility
89. Most interpretable algorithm:  
    a) Deep Neural Net b) Random Forest c) Single Decision Tree d) RBF Kernel SVM  
    **Answer:** c) Single Decision Tree
90. Effect of an excessively high learning rate:  
    a) Slow convergence b) Loss divergence / numerical instability / NaN values c) Perfect fit d) None  
    **Answer:** b) Loss divergence / numerical instability / NaN values
91. Symptom of Vanishing Gradient:  
    a) NaN loss b) Parameters in early layers update extremely slowly or stop changing c) Memory OOM d) Overfitting  
    **Answer:** b) Parameters in early layers update extremely slowly or stop changing
92. In a confusion matrix, False Positive means:  
    a) Actual Negative predicted as Positive b) Actual Positive predicted as Negative c) Actual Positive predicted as Positive d) Actual Negative predicted as Negative  
    **Answer:** a) Actual Negative predicted as Positive
93. Technique for reducing model footprint for edge deployment:  
    a) Quantization b) Dropout c) BatchNorm d) Augmentation  
    **Answer:** a) Quantization
94. Main objective of Word2Vec (Skip-Gram):  
    a) Classify documents b) Predict surrounding context words given a target word c) Cluster text d) Translate language  
    **Answer:** b) Predict surrounding context words given a target word
95. In CNNs, a "Kernel" / "Filter" refers to:  
    a) Activation function b) Weight matrix sliding over input spatial grid c) Pooling window d) Learning rate  
    **Answer:** b) Weight matrix sliding over input spatial grid
96. "Freezing" layers in transfer learning means:  
    a) Deleting layers b) Disabling gradient computation (`requires_grad=False`) for pre-trained weights c) Setting weights to 0 d) Adding dropout  
    **Answer:** b) Disabling gradient computation (`requires_grad=False`) for pre-trained weights
97. Type II error in hypothesis testing corresponds to:  
    a) False Positive b) False Negative c) True Positive d) True Negative  
    **Answer:** b) False Negative
98. Purpose of a Learning Rate Scheduler:  
    a) Keep LR constant b) Systematically decrease LR over training to fine-tune convergence c) Randomize LR d) None  
    **Answer:** b) Systematically decrease LR over training to fine-tune convergence
99. Generative vs Discriminative distinction:  
    a) Generative models joint distribution $P(X, Y)$; Discriminative models $P(Y|X)$ b) Generative is non-linear c) Discriminative generates data d) None  
    **Answer:** a) Generative models joint distribution $P(X, Y)$; Discriminative models $P(Y|X)$
100. True statement regarding Batch Normalization:  
     a) Applied to layer activations b) Uses mini-batch mean and variance during training c) Contains learnable scale $\gamma$ and shift $\beta$ parameters d) All of the above  
     **Answer:** d) All of the above

---

## 4. Scenario Questions

### 75 Complete Real-World Scenario Problems
1. **Credit Card Fraud Architecture**: Design a fraud detection pipeline handling 99.9:0.1 class imbalance and real-time concept drift. Detail feature engineering (Flink 5-min aggregations), model choice (XGBoost + Focal Loss + GNN), and KS-test monitoring.
2. **Post-Deployment Error Spikes**: A deployed model's error rate spikes suddenly after 3 months. Walk through diagnostic steps (check data drift, feature pipeline schema changes, upstream API failures, label definition shifts).
3. **Customer Churn System**: Design an end-to-end customer churn prediction engine for a telecom provider. Specify data collection, RFM features, model baseline, and A/B test campaign integration.
4. **Enterprise Support RAG Bot**: Architect a customer support chatbot utilizing an LLM over internal PDF documentation. Detail chunking, embedding generation, vector DB index (FAISS), RAG retrieval, and hallucination evaluation.
5. **Cold-Start Recommender**: A movie streaming service adds 10,000 new users with zero history. Design a multi-armed bandit (Thompson Sampling) + demographic hybrid recommendation engine.
6. **High-Precision Satellite Vision**: Build an automated building damage detector from satellite imagery. Detail UNet / Segment Anything Model (SAM) choice, spatial resolution matching, and IoU metric choice.
7. **High Feature-to-Sample Ratio ($P \gg N$)**: You have 10,000 genomic features but only 500 patient samples. How do you construct a non-overfitting predictor? (L1 Lasso feature selection, PCA, ElasticNet, strict nested cross-validation).
8. **Social Graph Friend Recommendation**: Design a "People You May Know" graph ML engine using Graph Neural Networks (Node2Vec / GraphSAGE) and mutual connection features.
9. **Sub-10ms Transformer CPU Inference**: Optimizing an LLM/BERT inference API from 50ms to $<10\text{ms}$ on CPU. Detail ONNX export, INT8 quantization, layer pruning, and OpenVINO thread optimization.
10. **Automated Continuous Retraining Safety**: Design a CI/CD MLOps pipeline that retrains daily but guards against deploying degraded models using shadow deployments and automated A/B gate checks.
11. **Unsupervised Customer Segmentation**: Segment 1 million retail customers without labels. Walk through RFM feature extraction, PCA reduction, K-Means++ clustering, and Silhouette / business validation.
12. **Machine Translation Service**: Build an automated English-to-Spanish translation system. Detail Seq2Seq Transformer model, Byte-Pair Encoding, and BLEU / COMET evaluation.
13. **Offline vs Online Metric Disconnect**: Model achieves 95% offline ROC-AUC but conversion drops in production online A/B testing. Detail reasons (data leakage, feedback loop delay, latency timeout drops, metric mismatch).
14. **Automated Invoice Information Extraction**: Design an OCR + NLP pipeline extracting total amount, date, and vendor from scanned PDF invoices using LayoutLM.
15. **Fair Loan Approval System**: Build an ML credit scoring system ensuring demographic fairness. Detail bias auditing, equalized odds constraints, and model explainability (SHAP).
16. **Medical Image Small Data Diagnostics**: Build a chest X-ray pneumonia classifier with only 500 labeled images. Detail transfer learning (ImageNet pre-trained ResNet), heavy data augmentation, and Coss-Validation.
17. **Real-Time Retail Foot Traffic Counting**: Design an edge vision pipeline counting shoppers entering a store using YOLOv8 object detection and DeepSORT multi-object tracking.
18. **Centralized Production Model Monitoring**: Architect an enterprise ML monitoring stack tracking latency, error rates, feature drift (PSI/KS-test), and output distributions across 50 microservices.
19. **News Article Feed Personalization**: Design a personalized news feed ranking engine. Track offline NDCG@10 and online Dwell Time, CTR, and Diversity metrics.
20. **City-Wide Traffic Congestion Prediction**: Build a spatio-temporal traffic forecasting system combining IoT loop detectors, weather data, and Temporal Graph Convolutional Networks (T-GCN).
21. **Search Query Auto-Complete**: Design an ML-powered search auto-complete system using trie structures, fine-tuned GPT language models, and user context signals under 15ms latency.
22. **Spam Email Filtering Engine**: Architect a spam detection pipeline using TF-IDF + Naive Bayes baseline evolving to BERT embeddings with real-time domain reputation signals.
23. **Deepfake Video Detection**: Design a deep learning pipeline detecting facial forgery in video using spatial CNN feature extractors + temporal LSTM frame consistency checks.
24. **Unstructured Text Knowledge Graph Construction**: Extract entities and relations from technical documents using NER (Spacy/BERT) + Relation Extraction to populate a Neo4j Knowledge Graph.
25. **E-Commerce Product Categorization**: Classify 10 million product titles into 5,000 hierarchical categories using hierarchical softmax and fine-tuned Sentence-Transformers.
26. **AI Music Generation**: Architect an autoregressive music generation model using Symbolic MIDI representations, Music Transformer, and self-attention.
27. **Hospital Readmission Risk Predictor**: Build a clinical model predicting 30-day readmission from electronic health records (EHR) containing missing data, irregular time intervals, and class imbalance.
28. **Ride-Sharing ETA Prediction**: Build a real-time Estimated Time of Arrival (ETA) system for Uber combining route graph algorithms, historical speed features, and XGBoost / Deep learning models.
29. **Real-Time Online Learning Architecture**: Architect an online learning system updating model weights continuously from streaming user click logs using Flink and online SGD.
30. **Legal Document Summarization**: Design a multi-document summarization engine processing 100-page contracts using Longformer / Hierarchical Transformers with extractive and abstractive heads.
31. **Visual Question Answering (VQA)**: Build a VQA model answering natural language queries about uploaded images using multimodal fusion of Vision Transformers (ViT) and LLMs.
32. **Multilingual Offensive Language Detection**: Build a content moderation system covering 50 languages using fine-tuned XLM-RoBERTa embeddings and cross-lingual transfer learning.
33. **Massive User Browsing Clustering**: Cluster 100 million user clickstream paths using Session Embeddings (Word2Vec on URLs) and scalable Mini-Batch $K$-Means.
34. **Server Infrastructure Anomaly Detection**: Build a real-time server metric anomaly detector using Isolation Forests and Autoencoders processing streaming CPU/Memory/IO metrics.
35. **Recommendation Algorithm A/B Test Design**: Plan a 2-week A/B test evaluating a new two-tower recommender against baseline. Detail sample sizing, random assignment, novelty bias mitigation, and metrics.
36. **Training under Noisy Labels**: Train an accurate image classifier on a dataset where 20% of labels are incorrect. Detail Co-teaching, Loss Correction matrices, and sample re-weighting.
37. **Secure Facility Face Recognition**: Design a high-security biometric facial recognition pipeline incorporating Liveness Detection (antispoofing), ArcFace embeddings, and cosine vector search.
38. **Financial Time-Series Stock Predictor**: Build a stock directional forecasting system addressing non-stationarity, low signal-to-noise ratio, walk-forward validation, and market impact fees.
39. **Accent-Robust Speech Recognition**: Build an ASR system robust to diverse regional accents using Conformer architectures, SpecAugment data augmentation, and accent-specific fine-tuning datasets.
40. **Real-Time Social Media Trending Topic Detection**: Build a real-time trending topic detector over Twitter/X streams using Locality-Sensitive Hashing (LSH), TF-IDF keyword extraction, and online clustering.
41. **Synthetic Data Generation for Privacy**: Generate privacy-preserving tabular medical datasets for external research using Conditional GANs (CTGAN) audited with Differential Privacy bounds.
42. **Unlabeled Corpus Self-Supervised Pre-Training**: You have 1TB of raw domain text but zero labels. Detail how to pre-train a domain-specific Transformer (Domain-Adaptive Pre-training) before fine-tuning.
43. **Industrial Predictive Maintenance**: Build a sensor failure prediction model predicting equipment breakdown 48 hours in advance using time-domain feature extraction and Random Forests.
44. **Mobile Mobile OCR & Text Recognition**: Design an edge OCR app executing text detection (EAST/CRAFT) and recognition (CRNN) under 100ms on iOS/Android devices.
45. **Customer Service Request Routing**: Architect an NLP ticket routing service classifying customer emails into 50 department queues with high precision requirements.
46. **Feed Ranking with Dwell Time Integration**: Incorporate continuous user dwell time into a feed ranking model using Multi-Task Learning (predicting both binary Click and continuous Dwell Time).
47. **Automated Retraining MLOps Pipeline**: Design an Airflow DAG automating data extraction, validation, training, evaluation against baseline, model registry registration, and Kubernetes deployment.
48. **Covariate Shift Mitigation**: The feature distribution $P(X)$ changes post-deployment while $P(Y|X)$ remains fixed. Detail Importance Weighting using domain classifier ratios $\frac{P_{\text{target}}(x)}{P_{\text{source}}(x)}$.
49. **CI/CD Software Build Failure Predictor**: Build an internal developer tool predicting git commit build failures using diff size, modified files, author history, and XGBoost.
50. **Sarcasm Detection in Text**: Build a sarcasm classifier handling contextual ambiguity using multimodal text + punctuation + user history context embeddings.
51. **Real-Estate Price Estimation**: Build a housing price predictor incorporating spatial GIS embeddings, school district metadata, text listing descriptions, and Gradient Boosting.
52. **Entity Resolution & Linking in News**: Build a pipeline extracting mentioned companies and linking them to Wikidata IDs using Named Entity Disambiguation (NED).
53. **Duplicate Question Detection (Quora-style)**: Build a duplicate question identification model using Siamese Transformers (SBERT) and Cosine Similarity thresholding.
54. **Bank Transaction Categorization**: Build a transaction classification model handling ambiguous raw merchant strings ("TST* STARBUCKS 1234") using character N-gram embeddings.
55. **Smartphone Sensor Mood Prediction**: Predict user mood from passive mobile sensor logs (screen time, step count, typing speed) respecting user privacy on-device.
56. **Image Super-Resolution System**: Design a real-time image upscaling pipeline using Super-Resolution Generative Adversarial Networks (SRGAN).
57. **Automatic Video Voice Dubbing**: Architect an automated video translation and dubbing pipeline combining ASR $\to$ NMT $\to$ Text-to-Speech (TTS) $\to$ Lip-sync GAN (Wav2Lip).
58. **Large Corpus PDF Question-Answering**: Architect a enterprise RAG system querying 10,000 corporate PDF documents with exact page-number citation linking.
59. **Low False-Positive Credit Card Fraud**: Build a high-throughput fraud engine enforcing a strict False Positive Rate threshold $<0.01\%$ using Neyman-Pearson decision rules.
60. **Zero-History Cold-Start Recommender**: Detail a 3-step strategy for recommending articles to a guest user with zero session history (Popularity baseline $\to$ Geo/Device features $\to$ Session RNN).
61. **High Precision Low Recall Diagnosis**: Model exhibits 95% precision but 20% recall on medical diagnosis. Walk through parameter adjustments (lower decision threshold, adjust class weights, re-sample data).
62. **Multi-Label Image Tagging**: Build an image classifier predicting multiple non-mutually-exclusive tags per image using Binary Cross-Entropy loss on multiple sigmoid outputs.
63. **Temporal Feature Engineering for Fraud**: Extract temporal transaction velocity features (e.g. number of distinct cards used on same IP in past 1 hour) for fraud detection.
64. **Customer Lifetime Value (LTV) Prediction**: Predict 1-year spending for new users using a Zero-Inflated Tweedie Regression model handling excess zero spenders.
65. **Automated Drift-Triggered Retraining**: Implement an automated monitoring daemon measuring Population Stability Index (PSI) on production inputs to trigger retraining DAGs when PSI $> 0.25$.
66. **Inappropriate Image Content Moderation**: Build a multi-class NSFW image filter operating under 50ms per upload using fine-tuned EfficientNet.
67. **Resume Structured Data Extraction**: Extract candidate name, education history, skills, and work experience from raw PDF resumes using LayoutLMv3 and CRF sequence taggers.
68. **Document Keyword Extraction**: Compare statistical (TF-IDF, RAKE), graph-based (TextRank), and embedding-based (KeyBERT) algorithms for keyword extraction.
69. **Active Learning for Label Reduction**: Reduce labeling budget by 80% using Uncertainty Sampling (least confidence, margin sampling) to select most informative un-annotated instances.
70. **Federated Learning across Siloed Banks**: Train a joint fraud detection model across 5 independent banks without sharing customer data using Federated Averaging (FedAvg) and PySyft.
71. **Food Delivery App Contextual Recommender**: Build a food recommendation model factoring in past order preferences, current time of day, location weather, and prep time SLA.
72. **Anti-Money Laundering (AML) Pattern Detection**: Detect complex money laundering transaction chains using Graph Convolutional Networks (GCN) on financial network graphs.
73. **Reference-Free Generative Quality Audit**: Audit the quality of an LLM generation pipeline using Semantic Entropy, Self-Consistency checking, and Perplexity scoring.
74. **Real-Time Sign Language Translation**: Build a computer vision system translating American Sign Language (ASL) video into text in real time using MediaPipe hand tracking and LSTM.
75. **Interpretable Multicollinear Regression**: Build an economic forecasting model with highly correlated features ensuring stable interpretable coefficients using Ridge Regression and VIF pruning.

---

## 5. Production Problems

### 50 Real Production Issues & Solutions
1. **Sudden Accuracy Drop**: Deployed model accuracy drops from 92% to 65%. *Diagnosis*: Upstream feature API schema modified string values to lowercase, breaking categorical encoding. *Fix*: Add schema validation gate in data pipeline.
2. **GPU Training OOM Error**: Training job crashes with `CUDA Out of Memory`. *Fix*: Reduce mini-batch size, enable mixed precision (`torch.cuda.amp`), use gradient accumulation, or apply `torch.utils.checkpoint`.
3. **High API Inference Latency**: Serving latency exceeds 200ms SLA. *Fix*: Convert model to ONNX runtime, apply INT8 quantization, introduce Redis inference caching, scale worker replicas.
4. **Non-Deterministic Model Predictions**: Model returns different outputs for identical input. *Fix*: Set explicit random seeds for PyTorch, NumPy, CUDA (`torch.backends.cudnn.deterministic=True`), and disable Dropout during inference (`model.eval()`).
5. **Stale Data Pipeline Ingestion**: Feature store pipeline halts silently. *Fix*: Implement Great Expectations data freshness assertions and Airflow PagerDuty alerts.
6. **Negative A/B Test Results**: New model performs worse in online A/B test despite higher offline validation score. *Fix*: Audit for offline data leakage, evaluate latency impact on user drop-off, execute automated rollback.
7. **Serving Service Crash Under Load**: Triton / FastAPI server crashes during traffic spikes. *Fix*: Add Kubernetes Horizontal Pod Autoscaler (HPA), configure rate limiting, move to async queue processing.
8. **Input Feature Distribution Shift**: Customer demographic shifts post-marketing campaign. *Fix*: Re-weight training data using domain adaptation density ratio estimation and retrain model.
9. **Prediction Errors for Specific User Segment**: High error rate exclusively on Android users. *Fix*: Audit feature pipeline for OS-specific missing features; re-balance training representation across user devices.
10. **CI/CD Deployment Conflict**: Model version mismatch between service container and feature store. *Fix*: Enforce strict semantic versioning and MLflow registry binding in Docker deployment pipeline.
11. **GPU Underutilization during Training**: GPU usage stays $<20\%$. *Fix*: Data loader is CPU bottlenecked; increase `num_workers`, set `pin_memory=True`, preload data to NVMe SSD.
12. **Incompatible Dependency Crash**: Container fails due to `numpy` version mismatch. *Fix*: Lock environment dependencies using Docker containers and `poetry.lock` / `requirements.txt` hashes.
13. **Model Bias Against Demographic**: Audit reveals elevated false rejection rate for minority subgroup. *Fix*: Apply demographic parity post-processing threshold adjustments or re-balance training data using fair representation learning.
14. **Dynamic Multi-Model Routing**: Serving multiple fine-tuned models per client request. *Fix*: Implement API Gateway router directing traffic based on tenant ID to dedicated model endpoints.
15. **DB Connection Leak in Inference Server**: API leaks connections under load. *Fix*: Implement database connection pooling (SQLAlchemy Pool) and close connections in `finally` blocks.
16. **REST API Timeouts**: Heavy model inference causes client gateway timeout. *Fix*: Switch endpoint from synchronous REST to asynchronous WebSockets or Task Queue (Celery + Redis).
17. **Model Footprint Too Large for Edge**: 2GB PyTorch model fails on mobile device. *Fix*: Apply post-training INT4/INT8 quantization, structured pruning, or distill model into a lightweight MobileNet student architecture.
18. **Retraining Pipeline Data Leakage**: Automated retraining script includes target labels from future test periods. *Fix*: Enforce strict time-based data splitting in Airflow pipeline DAG.
19. **Null Prediction Explosion**: Model predicts default class 90% of the time due to missing input fields. *Fix*: Add API input payload validation returning 400 Bad Request for unpopulated required fields.
20. **Inconsistent Canary Deployment Experience**: Users see flickering model variants on page refresh. *Fix*: Enforce sticky session routing using hashed User ID in load balancer.
21. **Minimal Impact Shadow Deployment**: Evaluating model in production without affecting users. *Fix*: Deploy new model in Shadow Mode: duplicate live request traffic to shadow container and log predictions asynchronously without returning them to user.
22. **Stale Feature Store Serving**: Feature store returns 24-hour-old aggregations for real-time model. *Fix*: Upgrade feature store pipeline from daily batch ETL to streaming Flink pipeline connected to Redis.
23. **Adversarial Data Poisoning in Online Learning**: Malicious user sends fake feedback to corrupt online learning model. *Fix*: Add anomaly detection filter on incoming training samples and enforce robust loss functions.
24. **Slow Batch Inference Job**: Daily scoring of 50 million records takes 12 hours. *Fix*: Parallelize batch inference using PySpark or Ray across a distributed GPU cluster.
25. **Un-calibrated Probabilities Distorting Business Logic**: Output probabilities cluster at extreme values 0.01 and 0.99. *Fix*: Apply Platt Scaling or Isotonic Regression on validation predictions to calibrate probabilities.
26. **GPU Memory Fragmentation**: CUDA OOM despite free memory shown in `nvidia-smi`. *Fix*: Call `torch.cuda.empty_cache()` and set environment variable `PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:128`.
27. **Model Registry Mismatch**: Deployed Docker container uses different weights than tracked Git commit. *Fix*: Embed MLflow Run ID directly into Docker image metadata during build phase.
28. **Auditing Historical Predictions**: Compliance requires reproducing prediction from 6 months ago. *Fix*: Maintain immutable logs recording Model Version, Feature Version, Input Payload, and Prediction Output in S3 data lake.
29. **Train-Serve Skew in Feature Code**: Python pandas feature code in training differs from C++ feature code in serving. *Fix*: Unify feature engineering code using a shared Feature Store (Feast / Tecton).
30. **Schema Shift Crash**: Upstream database renamed column `user_age` to `age`. *Fix*: Implement contract testing (Pydantic schema validation) at API ingestion layer.
31. **Geographic Performance Variance**: Model accurate in US but fails in Europe. *Fix*: Train country-specific sub-models or include localized economic features and region embeddings.
32. **GDPR Data Deletion Compliance**: User exercises "Right to be Forgotten" requiring removal from trained model. *Fix*: Machine Unlearning techniques (SISA architecture - Sharded, Isolated, Sliced, and Aggregated training).
33. **Inference Latency SLA Violation on 99th Percentile**: $P_{99}$ latency spikes to 2 seconds while $P_{50}$ is 10ms. *Fix*: Identify tail latency causes (garbage collection pauses, thread contention) and set strict timeout cancellation limits.
34. **Disk Space Exhaustion from Model Logs**: Verbose prediction logging fills server disk in 2 days. *Fix*: Log predictions asynchronously to streaming Kafka topic and archive to compressed S3 cold storage.
35. **Segfault on Specific Input Payload**: Model crashes C++ runtime on certain inputs. *Fix*: Sanitize input strings for null bytes, enforce array bounds checking, run model inside isolated worker processes.
36. **Massive Docker Image Size**: Model serving Docker image is 15GB, causing slow deployment pulls. *Fix*: Use multi-stage Docker builds, base images (`python:3.10-slim`), and download model weights at runtime from S3 rather than baking them into image.
37. **Unseen Categorical Values in Production**: Model encounters new category string not seen in training. *Fix*: Map unseen categories to an explicit `__UNK__` token during encoding.
38. **Silent Feature Pipeline Failure**: Pipeline fails silently and passes zero vectors to model. *Fix*: Implement data validation checks testing for zero-variance or missing data spikes prior to model inference.
39. **Seed Variance in Model Retraining**: Retrained model accuracy fluctuates by 5% across random seeds. *Fix*: Enforce fixed random seeds across Python, NumPy, PyTorch, and CUDA, and average metrics across multiple seeds during validation.
40. **GDPR Explainability Requirement**: Regulation requires providing human-understandable reason for loan denial. *Fix*: Compute local SHAP values per inference request and return top 3 negative contributing features in API response.
41. **High Memory Overhead from Optimizer States**: Adam optimizer consumes $2\times$ model weight memory. *Fix*: Switch to 8-bit Adam (`bitsandbytes`) or Adafactor optimizer.
42. **Handling Missing Fields in Client Payloads**: Client API request omits optional fields. *Fix*: Use Pydantic models with default fallback values and missing indicators.
43. **Deadlock in Distributed Training**: Multi-GPU training hangs indefinitely at epoch end. *Fix*: Ensure all GPUs process identical batch counts per epoch or use `join()` context manager in PyTorch DDP.
44. **Biased Retraining Sample**: Retraining dataset sampled exclusively from clicked items. *Fix*: Use inverse propensity scoring (IPS) to correct for selection bias in logged feedback data.
45. **Multi-Replica Shared State Corruption**: Retraining online model across replicas corrupts weights. *Fix*: Centralize weight update parameters in a dedicated Parameter Server or Redis master.
46. **Network Failure Halting Long Training Job**: 3-day training job fails at hour 70 due to network glitch. *Fix*: Implement automated checkpoint saving every $N$ steps to persistent storage and resume automatically upon job restart.
47. **Incorrect Scaling Stats in Inference**: Inference code calculates mean and std from incoming batch instead of training set. *Fix*: Persist fit `StandardScaler` object as a serialized artifact alongside model weights.
48. **Changing Target Definition**: Business alters target variable definition. *Fix*: Re-label historical data according to new business logic; fine-tune model parameters starting from previous weights.
49. **Memory Leak in PyTorch Loop**: Training loop continuously consumes RAM until crash. *Fix*: Replace `total_loss += loss` with `total_loss += loss.item()` to unhook tensor from autograd graph.
50. **Model Retraining Stall on Cold Starts**: Retrained model has zero predictions for newly launched product categories. *Fix*: Fallback to content-based or popularity heuristics for new categories until interaction threshold is met.

---

## 6. Debugging Problems

### 50 Concrete Debugging Scenarios & Resolutions
1. **NaN Loss After Few Steps**: *Cause*: Exploding gradients or $\log(0)$ in loss calculation. *Fix*: Add gradient clipping `torch.nn.utils.clip_grad_norm_()`, add $\epsilon=1e-15$ inside log operations, lower learning rate.
2. **Train Loss Drops, Val Loss Increases**: *Cause*: Overfitting. *Fix*: Increase L2 weight decay, add Dropout, apply Early Stopping, reduce model capacity, add data augmentation.
3. **100% Train Accuracy, 50% Val Accuracy**: *Cause*: Severe model memorization or data leakage in training set. *Fix*: Audit for target leakage in features, add strong regularization, check for duplicate samples across splits.
4. **All Predictions Are Single Class**: *Cause*: Severe class imbalance, learning rate too high, or improper output layer initialization. *Fix*: Add class weights to loss function, lower learning rate, initialize output bias to log-prior $\log(P(Y=1)/P(Y=0))$.
5. **Probabilities Do Not Sum to 1**: *Cause*: Applied Sigmoid instead of Softmax across multi-class output dimension. *Fix*: Replace `torch.sigmoid()` with `torch.softmax(dim=-1)` on final layer outputs.
6. **Zero Gradients in Early Layers**: *Cause*: Vanishing gradients due to deep Sigmoid/Tanh layers or Dead ReLUs. *Fix*: Replace Sigmoid/Tanh with LeakyReLU or GELU; add Residual Skip Connections; apply He Initialization.
7. **Loss Oscillates Wildly**: *Cause*: Learning rate is set too high for optimization landscape. *Fix*: Reduce learning rate by factor of 10 or implement Cosine Annealing learning rate scheduler with warmup.
8. **Val Loss Spikes Suddenly**: *Cause*: Mini-batch gradient outlier or learning rate too high during late training phase. *Fix*: Enable gradient norm clipping and apply learning rate decay.
9. **Slow Training & Low GPU Usage**: *Cause*: CPU data loading bottleneck. *Fix*: Increase `DataLoader` workers (`num_workers=4`), enable `pin_memory=True`, convert raw image files to HDF5/TFRecords format.
10. **Dataset Generalization Failure**: Model performs well on Dataset A but fails completely on similar Dataset B. *Cause*: Domain shift or model learned spurious dataset-specific background correlations. *Fix*: Apply domain adversarial training or augment training data with varied backgrounds.
11. **Negative Predicted Prices**: Linear regression predicts negative continuous values for strictly positive target (price). *Fix*: Transform target variable using $\log(y)$ during training and project back using $\exp(\hat{y})$.
12. **Loss Decreases but Accuracy Plateaus**: *Cause*: Model predictions are becoming more confident on correctly classified samples while decision boundary is not moving for misclassified samples. *Fix*: Adjust loss function or tune classification threshold.
13. **Model Fails Exclusively on Minority Class**: *Cause*: Standard loss function treats all sample errors equally, allowing majority class optimization to dominate. *Fix*: Switch to Focal Loss or adjust `pos_weight` parameter in Cross-Entropy.
14. **Enormous Checkpoint File Size**: *Cause*: Checkpoint dictionary saves optimizer states, epoch history, and full autograd graph. *Fix*: Save only model state dict (`torch.save(model.state_dict(), path)`).
15. **Data Loading Bottleneck**: GPU waits for CPU to complete image transformations. *Fix*: Move image transformations to GPU using `kornia` or torchvision GPU transforms.
16. **OOM on Small Batch Size**: GPU OOM even with batch size 2. *Cause*: Extremely large input image resolution or memory leak in custom layer. *Fix*: Downsample input resolution, use mixed precision (FP16/BF16), check for unreleased CUDA tensors.
17. **Code Runs on CPU but Fails on GPU**: `RuntimeError: Expected all tensors to be on the same device`. *Fix*: Explicitly move all created input tensors and model modules to GPU (`x = x.to(device)`).
18. **Identical Embeddings for All Inputs**: Neural network outputs identical vector representations for every input token. *Cause*: Representation collapse (common in self-supervised learning without negative pairs). *Fix*: Add contrastive loss term (InfoNCE) or stop-gradient operations (SimSiam).
19. **LR Scheduler Stalls Training**: Scheduler drops learning rate to near-zero within first 2 epochs. *Fix*: Adjust `step_size` / `patience` settings or increase minimum learning rate (`eta_min`).
20. **BatchNorm Errors at Inference**: Model outputs gibberish during `.eval()`. *Cause*: BatchNorm running statistics ($\mu, \sigma^2$) were not updated during custom training loop or model trained with batch size 1. *Fix*: Ensure `model.train()` is called during training and train with batch size $> 8$.
21. **Dropout Active During Evaluation**: Model outputs non-deterministic validation predictions. *Cause*: Forgot to call `model.eval()`. *Fix*: Call `model.eval()` prior to running validation inference.
22. **Custom Loss Incorrect Gradients**: Custom PyTorch loss yields wrong parameter updates. *Cause*: Used non-differentiable NumPy operations inside loss function, breaking autograd. *Fix*: Rewrite loss operations using pure PyTorch tensor functions.
23. **Softmax Paired with MSE Loss**: Model trains extremely slowly. *Cause*: Derivative of Sigmoid/Softmax saturates when paired with MSE. *Fix*: Pair Softmax / Sigmoid output directly with Cross-Entropy Loss.
24. **Test Set Normalized using Test Statistics**: Validation accuracy looks unrealistically high. *Cause*: Data leakage from fitting `StandardScaler` on test set. *Fix*: Fit scaler ONLY on training data (`scaler.fit(X_train)`), then transform test data (`scaler.transform(X_test)`).
25. **Un-scaled Feature Gradient Explosion**: Feature with range $[0, 1,000,000]$ causes NaN weights. *Fix*: Apply `StandardScaler` or `Log1p` transformation to high-magnitude numerical features.
26. **Resolution Mismatch at Test Time**: Model trained on $224\times224$ images fails on $512\times512$ test images. *Fix*: Add Global Average Pooling layer before final classification head or resize test images to $224\times224$.
27. **Random Seed Inconsistency**: Code produces non-reproducible results across runs. *Cause*: Set seed for Python `random` but omitted `np.random.seed()` or `torch.manual_seed()`. *Fix*: Set seeds globally across all random generators.
28. **Augmentation Applied to Labels**: Image rotation transform rotated target bounding boxes incorrectly. *Fix*: Apply spatial transformations jointly to both input image and target mask/bounding box coordinates.
29. **Class Weights Omitted in Cross-Entropy**: Imbalanced multi-class dataset yields zero recall for rare class. *Fix*: Pass calculated class weights array to PyTorch `nn.CrossEntropyLoss(weight=class_weights)`.
30. **Checkpoint Weight Naming Mismatch**: Loading pre-trained weights fails with `Missing key(s) in state_dict`. *Cause*: Model wrapped in `nn.DataParallel` added `module.` prefix to parameter names. *Fix*: Strip `module.` prefix from state dictionary keys prior to loading.
31. **Model Predicts Random Garbage**: Model predictions are totally random during inference. *Cause*: Forgot to call `model.eval()`, leaving BatchNorm and Dropout in training mode. *Fix*: Always invoke `model.eval()` before running inference.
32. **Loss Computed on Full Batch but Backprop per Sample**: Training is extremely slow. *Cause*: Invoking `loss.backward()` inside inner loop over individual samples instead of mini-batch. *Fix*: Compute loss across entire mini-batch tensor and call `backward()` once per batch.
33. **Forgot `optimizer.zero_grad()`**: Loss explodes or decreases weirdly. *Cause*: PyTorch accumulates gradients across iterations by default. *Fix*: Call `optimizer.zero_grad()` at the start of every training step.
34. **Mixing Trainable & Frozen Parameters**: Optimizer updates weights that were intended to be frozen. *Fix*: Filter trainable parameters when constructing optimizer (`optim.Adam(filter(lambda p: p.requires_grad, model.parameters()), lr=1e-3)`).
35. **Conv Output Dimension Mismatch**: Spatial dimensions collapse to zero after 3 layers. *Cause*: `kernel_size` too large relative to input spatial size without padding. *Fix*: Add appropriate padding (`padding=kernel_size // 2`) to maintain spatial dimensions.
36. **Gradient Flow Broken by `.view()` or `.detach()`**: Early layers receive zero gradients. *Cause*: Called `.detach()` or converted tensor to NumPy array in forward pass. *Fix*: Use PyTorch tensor operations (`torch.reshape`) to preserve autograd graph connections.
37. **RNN Hidden State Retained Across Unrelated Batches**: RNN fails to learn sequence structure. *Cause*: Failed to detach hidden state between independent batch sequences. *Fix*: Detach hidden state at batch boundaries (`h = h.detach()`).
38. **Tokenizer Truncation Removes Key Context**: NLP classification accuracy drops on long documents. *Cause*: Tokenizer `max_length=512` truncated essential ending text. *Fix*: Use sliding window tokenization or long-context model (Longformer).
39. **NumPy Operations in Custom PyTorch Module**: Model forward pass executes without error, but weights never change. *Cause*: NumPy operations stripped autograd graph tracking. *Fix*: Replace all `np.*` calls with equivalent `torch.*` operations.
40. **Labels One-Hot Encoded for `nn.CrossEntropyLoss`**: PyTorch throws `Target size mismatch` error. *Cause*: `nn.CrossEntropyLoss` expects 1D class index tensor, not 2D one-hot encoded matrix. *Fix*: Pass class indices `y = torch.tensor([0, 2, 1])` directly.
41. **Wrong Softmax Axis**: Softmax normalization computed across batch dimension instead of feature dimension. *Fix*: Set explicit dimension parameter `F.softmax(x, dim=-1)`.
42. **Data Augmentation Flips Digit Labels**: Image rotation/flip turned digit '6' into digit '9'. *Fix*: Disable vertical flipping and 180-degree rotation augmentations for digit recognition tasks.
43. **Dtype Mismatch (`float32` vs `float64`)**: PyTorch throws `Expected object of scalar type Float but got Double`. *Fix*: Convert input tensors explicitly to `torch.float32` (`x = x.float()`).
44. **Sub-Module Not Registered as Layer**: PyTorch model ignores parameters of custom layer defined as plain Python list. *Fix*: Use `nn.ModuleList` or `nn.Sequential` to register sub-modules.
45. **Imbalanced Data Split Across GPUs in DDP**: Distributed training hangs or gives inconsistent metrics. *Cause*: Custom dataset length not divisible by world size. *Fix*: Use `torch.utils.data.distributed.DistributedSampler`.
46. **Shuffle Buffer Too Small in Streaming Data**: Model exhibits high batch-to-batch variance. *Cause*: `buffer_size` in streaming dataset was too small to break temporal correlations. *Fix*: Increase streaming shuffle buffer size.
47. **Initial Instability in Deep Transformer Training**: Transformer loss spikes to `NaN` in first 100 steps. *Cause*: Large initial learning rate without warmup. *Fix*: Implement linear learning rate warmup for first 1000 steps.
48. **Catastrophic Forgetting During Fine-Tuning**: Pre-trained model completely forgets general capabilities after fine-tuning on niche task. *Fix*: Lower fine-tuning learning rate ($10\times$ smaller), freeze early layers, or use LoRA adapters.
49. **Inadvertent Gradient Computation in Validation Loop**: GPU OOM during validation loop. *Cause*: Omitted `torch.no_grad()` context wrapper. *Fix*: Wrap validation evaluation inside `with torch.no_grad():`.
50. **Inference Script Runs in Training Mode**: Production predictions are inconsistent and slow. *Cause*: Forgot `model.eval()` call in deployment container. *Fix*: Call `model.eval()` immediately after loading model state dictionary.

---

## 7. Output Prediction Questions

### 50 Code & Math Output Prediction Problems
1. **Linear Regression Prediction**: Given model $\hat{y} = 2 + 3 x_1 + 4 x_2$. Input sample $x = [5, 10]$. What is $\hat{y}$? *Answer*: $\hat{y} = 2 + 3(5) + 4(10) = 57$.
2. **Logistic Regression Probability**: A binary logistic model outputs log-odds $z = 0$. What is predicted probability $p$? *Answer*: $p = \frac{1}{1 + e^0} = 0.5$.
3. **CNN Spatial Output Dimension**: Input image size $32\times32\times3$, filter size $5\times5$, stride 1, padding 0. Output spatial shape? *Answer*: $O = \frac{32 - 5 + 2(0)}{1} + 1 = 28 \times 28 \times 6$.
4. **Max-Pooling Dimension**: Feature map size $28\times28$, pooling window $2\times2$, stride 2. Output shape? *Answer*: $14 \times 14$.
5. **K-Means First Iteration Centroids**: Data points $(0,0), (0,2), (4,4), (4,6)$. Initial centroids $C_1=(0,0), C_2=(4,4)$. Assign points and find new $C_1, C_2$. *Answer*: Points $(0,0),(0,2) \to C_1=(0,1)$. Points $(4,4),(4,6) \to C_2=(4,5)$.
6. **Random Forest Voting**: Ensemble of 10 trees: 7 predict Class A, 3 predict Class B. What is ensemble prediction? *Answer*: Class A (Majority vote).
7. **Sigmoid to Log-Odds**: Given model output probability $p = 0.8$, what is log-odds $z$? *Answer*: $z = \ln(\frac{0.8}{0.2}) = \ln(4) \approx 1.386$.
8. **Decision Tree Split Output**: Dataset $x = [1, 2, 3, 4]$, target $y = [0, 0, 1, 1]$. Split rule $x \le 2.5$. What are leaf outputs? *Answer*: Left leaf: $0$. Right leaf: $1$.
9. **Softmax Class Prediction**: Softmax output probabilities $[0.1, 0.65, 0.25]$. Which class is predicted? *Answer*: Class 1 (0-indexed).
10. **BatchNorm Output Computation**: Training mini-batch mean $\mu = 2$, var $\sigma^2 = 4$ ($\sigma = 2$). Input sample $x = 6$, $\gamma = 1, \beta = 0, \epsilon = 0$. Normalized output? *Answer*: $\hat{x} = \frac{6 - 2}{2} = 2$.
11. **Inverted Dropout Scaling**: Dropout rate $p = 0.5$. Input activation value was $4.0$. If neuron is NOT dropped, what is output during training? *Answer*: $\frac{4.0}{1 - 0.5} = 8.0$.
12. **L2 Regularization Gradient**: L2 penalty term $\frac{1}{2} \lambda w^2$ with $\lambda = 0.01$ and $w = 4$. What is regularization gradient added to weight update? *Answer*: $\lambda w = 0.01 \times 4 = 0.04$.
13. **Norm Gradient Clipping**: Gradient vector $g = [3, 4]$ (norm $\|g\|_2 = 5$). Max allowed norm $= 1$. What is clipped gradient vector? *Answer*: $g_{\text{clipped}} = \frac{g}{\|g\|_2} \times 1 = [\frac{3}{5}, \frac{4}{5}] = [0.6, 0.8]$.
14. **LSTM Cell State Update**: Forget gate $f_t = 1$, previous cell $C_{t-1} = 3$, input gate $i_t = 0.5$, candidate $\tilde{C}_t = 4$. New cell state $C_t$? *Answer*: $C_t = (1 \times 3) + (0.5 \times 4) = 5.0$.
15. **Attention Scaling Calculation**: Dot product $Q K^T = 32$, Key dimension $d_k = 64$ ($\sqrt{d_k} = 8$). Scaled score before Softmax? *Answer*: $\frac{32}{8} = 4.0$.
16. **Log Loss Calculation**: True label $y = 1$, predicted probability $\hat{y} = 0.1$. What is Log Loss for this sample? *Answer*: $-\ln(0.1) \approx 2.302$.
17. **Standardized Linear Regression Intercept**: Features standardized ($\mu_x = 0$), target mean $\mu_y = 10$. What is model intercept $w_0$? *Answer*: $w_0 = \mu_y = 10$.
18. **Momentum Update Calculation**: Previous velocity $v_{t-1} = 0.2$, momentum $\beta = 0.9$, learning rate $\eta = 0.1$, current gradient $g_t = 0.5$. New velocity $v_t$? *Answer*: $v_t = 0.9(0.2) + 0.1(0.5) = 0.18 + 0.05 = 0.23$.
19. **Adam First Step Estimates**: First step ($t=1$), gradient $g_1 = 0.2$, $\beta_1 = 0.9$. Unbiased first moment estimate $\hat{m}_1$? *Answer*: $m_1 = 0.1(0.2) = 0.02$. Bias corrected $\hat{m}_1 = \frac{0.02}{1 - 0.9^1} = 0.2$.
20. **Early Stopping Patience Trigger**: Patience $= 2$. Epoch 1 val loss: 0.40. Epoch 2: 0.42. Epoch 3: 0.45. Which epoch triggers stopping? *Answer*: Epoch 3 (2 consecutive non-improving epochs).
21. **Majority Vote Ensemble Accuracy**: Ensemble of 3 independent classifiers each with $80\%$ accuracy. What is ensemble accuracy? *Answer*: $P(\ge 2 \text{ correct}) = 3(0.8)^2(0.2) + (0.8)^3 = 0.384 + 0.512 = 0.896$ ($89.6\%$).
22. **PCA Dimensionality Reduction Shape**: Dataset shape $(1000, 50)$. Projected onto 5 principal components. Output matrix shape? *Answer*: $(1000, 5)$.
23. **Naive Bayes Posterior Calculation**: Priors $P(C_1)=0.5, P(C_2)=0.5$. Likelihoods $P(x|C_1)=0.8, P(x|C_2)=0.2$. Posterior $P(C_1|x)$? *Answer*: $\frac{0.8 \times 0.5}{(0.8 \times 0.5) + (0.2 \times 0.5)} = \frac{0.4}{0.5} = 0.8$.
24. **Precision and Recall Calculation**: $TP = 80, FP = 20, FN = 10, TN = 890$. Precision and Recall? *Answer*: $\text{Precision} = \frac{80}{100} = 0.80$. $\text{Recall} = \frac{80}{90} \approx 0.889$.
25. **$K$-Fold Fold Size**: Dataset size $N = 1000$, $K = 5$. How many samples in validation fold? *Answer*: $\frac{1000}{5} = 200$ samples.
26. **IDF Score for Universal Term**: Word appears in all $N = 100$ documents in corpus. What is its $\text{IDF} = \ln(\frac{N}{\text{DF}})$? *Answer*: $\ln(\frac{100}{100}) = \ln(1) = 0$.
27. **Cosine Similarity of Orthogonal Vectors**: Vector $A = [1, 0]$, Vector $B = [0, 1]$. Cosine similarity? *Answer*: $\frac{A \cdot B}{\|A\| \|B\|} = \frac{0}{1 \times 1} = 0$.
28. **One-Hot Matrix Shape**: Feature with 50 unique categories across 1000 rows. One-hot encoded shape? *Answer*: $(1000, 50)$.
29. **Polynomial Feature Expansion Count**: 2 input features $[x_1, x_2]$ expanded to degree 2 with bias. Resulting feature count? *Answer*: 6 features ($1, x_1, x_2, x_1^2, x_1 x_2, x_2^2$).
30. **MLP Parameter Count**: Input size 10, Hidden layer 20 (with bias), Output 2 (with bias). Total parameters? *Answer*: Layer 1: $(10 \times 20) + 20 = 220$. Layer 2: $(20 \times 2) + 2 = 42$. Total $= 262$.
31. **Ideal Discriminator Output**: Ideal Discriminator output for a perfectly generated fake image in a converged GAN? *Answer*: $0.5$ (Complete uncertainty).
32. **Total VAE Loss Calculation**: Reconstruction Loss $= 0.20$, KL Divergence Loss $= 0.05$. Total ELBO loss? *Answer*: $0.25$.
33. **L1 Loss Subgradient**: Target $y = 5$, prediction $\hat{y} = 3$. Loss $L = |y - \hat{y}| = |5 - 3| = 2$. Subgradient $\frac{\partial L}{\partial \hat{y}}$? *Answer*: $-1$.
34. **Label Smoothing Target Adjustment**: Binary target $y = 1$, smoothing factor $\epsilon = 0.1$. Smooth label $y_{\text{smooth}}$? *Answer*: $1 - 0.5(0.1) = 0.95$.
35. **Multi-Head Attention Subspace Dimension**: Model hidden dimension $d_{\text{model}} = 512$, heads $h = 8$. Per-head dimension $d_k$? *Answer*: $d_k = \frac{512}{8} = 64$.
36. **Perplexity of Uniform Language Model**: Model assigns uniform probability $p = 0.01$ to next word across 100 vocabulary items. Perplexity? *Answer*: $\text{PPL} = \frac{1}{0.01} = 100$.
37. **DBSCAN Point Classification**: $\text{minPts} = 4, \epsilon = 0.5$. Point $P$ has 2 neighbors within $\epsilon$. Is $P$ a Core point? *Answer*: No ($2 < 4$).
38. **SMOTE Interpolation**: Minority sample $A = [2, 4]$, neighbor $B = [6, 8]$, random weight $\lambda = 0.5$. Synthetic sample $C$? *Answer*: $C = A + 0.5(B - A) = [2, 4] + 0.5[4, 4] = [4, 6]$.
39. **Gradient Boosting Residual Target**: True target $y = 10$, current ensemble prediction $\hat{y}^{(t-1)} = 7$. Target for next tree $t$? *Answer*: Residual $r = 10 - 7 = 3$.
40. **Huber Loss Calculation**: Residual $e = y - \hat{y} = 0.5$, threshold $\delta = 1.0$. Since $|e| \le \delta$, Huber loss formula? *Answer*: $\frac{1}{2} e^2 = \frac{1}{2} (0.5)^2 = 0.125$.
41. **BLEU 1-Gram Precision**: Candidate text: "the cat sat", Reference text: "the cat sat on mat". 1-gram precision? *Answer*: $\frac{3}{3} = 1.0$.
42. **Word2Vec Weight Matrix Shape**: Vocabulary size $V = 10,000$, embedding dimension $d = 300$. Embedding lookup weight matrix shape? *Answer*: $(10000, 300)$.
43. **Top-5 Accuracy Check**: True label index: 4. Model top 5 predicted indices: $[1, 7, 4, 9, 0]$. Is sample correct under Top-5 accuracy? *Answer*: Yes (index 4 is present in top 5).
44. **Autoencoder Latent Bottleneck Compression**: Input dimension $784$, latent code dimension $32$. Compression ratio? *Answer*: $\frac{784}{32} = 24.5\times$ compression.
45. **$\epsilon$-Greedy Action Selection Probability**: $\epsilon = 0.1$, action space size $|A| = 4$. Probability of taking random exploration action? *Answer*: $10\%$.
46. **Learning Curve Diagnosis Output**: High training loss ($0.8$) and high validation loss ($0.85$). Diagnosis? *Answer*: Underfitting (High Bias).
47. **SVM Circular Decision Boundary Kernel**: 2D dataset separated by a circular decision boundary. Which SVM kernel produces this boundary? *Answer*: RBF (Radial Basis Function) Kernel.
48. **Imbalanced Constant Predictor Accuracy**: Dataset with 990 negative samples and 10 positive samples. Model predicts negative for all samples. Accuracy? *Answer*: $\frac{990}{1000} = 99.0\%$.
49. **Zero Variance PCA Eigenvalue**: A feature has zero variance across all dataset samples. What is its corresponding PCA eigenvalue? *Answer*: $0$.
50. **Ridge Penalty Weight Decay Effect**: In Ridge regression, as regularization hyperparameter $\lambda \to \infty$, what do weight parameters $w$ approach? *Answer*: All weights $w \to 0$.

---

## 8. Best Practices Questions

### 50 Industry Best Practice Directives
1. **Pre-Split Data Processing**: Always execute train-test splitting BEFORE fitting scalers, encoders, or missing value imputers to eliminate data leakage.
2. **Dedicated Validation Set**: Maintain a dedicated validation set strictly for hyperparameter tuning, model architecture selection, and early stopping.
3. **Stratified Sampling**: Use Stratified K-Fold for classification tasks to guarantee preserved class ratio distributions across all evaluation folds.
4. **Reproducibility Protocol**: Set global random seeds across Python `random`, `numpy.random`, PyTorch (`torch.manual_seed`), and CUDA runtime.
5. **Selective Scaling**: Apply feature scaling exclusively to distance-based ($K$-NN, SVM, $K$-Means) and gradient-based models; omit for tree algorithms.
6. **No Free Lunch Theorem**: Never assume a complex model (e.g. Transformer) outperforms a simple baseline (XGBoost/Linear Regression) without empirical verification.
7. **Parsimony Principle**: Prefer simpler, highly interpretable models when performance metrics are comparable to complex deep architectures.
8. **Test Set Isolation**: Never inspect, fit, or iterate on the test set during model iteration; evaluate on test set ONCE at final sign-off.
9. **Exploratory Data Analysis (EDA)**: Conduct thorough EDA to audit feature distributions, missingness patterns, class imbalance, and duplicate rows prior to modeling.
10. **Temporal Data Splitting**: Use Time-Series rolling origin cross-validation (`TimeSeriesSplit`) for temporal datasets to prevent future-data lookahead bias.
11. **Imputation Best Practices**: Impute numerical missing values using median (robust to outliers) and categorical missing values using mode or explicit `Missing` category tokens.
12. **Encoding Selection Rules**: Use One-Hot encoding for nominal categories with low cardinality ($<10$), Target Encoding for high-cardinality nominals, and Ordinal encoding for ordered categories.
13. **Production Model Monitoring**: Continuously track feature drift (PSI / KS-test), prediction latency, error rates, and output distribution shifts in production.
14. **MLOps Pipeline Versioning**: Version code (Git), data datasets (DVC), and model artifacts (MLflow) synchronously for complete experiment lineage.
15. **Train-Serve Consistency**: Share identical feature engineering code between training and online inference using a unified Feature Store.
16. **Artifact Persistence**: Save fitted scalers, encoders, and vectorizers as serialized artifacts alongside model weights.
17. **Transfer Learning First**: Prefer fine-tuning pre-trained foundation models over training deep neural networks from scratch on small domain datasets.
18. **Imbalanced Learning Strategy**: Combine algorithmic cost-sensitive loss adjustments (Focal Loss, class weights) with PR-AUC evaluation on imbalanced targets.
19. **Capacity Control**: Regularize deep networks via Dropout, L2 weight decay, BatchNorm, and Early Stopping rather than arbitrarily restricting width/depth.
20. **Cross-Validation Rigor**: Report cross-validation mean metrics alongside standard deviation across folds to quantify model stability.
21. **Training Loop Auditing**: Monitor learning rate curves, gradient norms, and training vs validation loss live during model execution.
22. **Early Stopping Protocol**: Save model checkpoints based on validation loss, not training loss, specifying a reasonable patience window.
23. **Batch Size Selection**: Choose mini-batch sizes ($32, 64, 128$) that fit comfortably in GPU VRAM while providing stochastic gradient noise benefits.
24. **Automated Hyperparameter Tuning**: Replace manual parameter guessing with automated Bayesian Optimization (Optuna / Ray Tune).
25. **Ensemble Selection**: Combine diverse model families (e.g. XGBoost + Neural Net + Logistic Regression) for maximum ensembling variance reduction.
26. **Experiment Tracking**: Log hyperparameters, metrics, hardware usage, and confusion matrices using structured platforms (Weights & Biases, MLflow).
27. **Model Endpoint Security**: Validate API input payloads with strict schemas (Pydantic), apply rate limiting, and audit for adversarial prompt injections.
28. **Differential Privacy**: Incorporate DP-SGD when training deep models on sensitive personal, financial, or medical user data.
29. **Model Baseline First**: Establish a dummy constant or simple linear model baseline before building complex deep neural networks.
30. **Retraining Triggers**: Define explicit automated retraining triggers based on schedule (e.g. weekly), data volume, or performance drift detection.
31. **Safe Deployment Patterns**: Deploy new model versions using Blue-Green or Canary releases with automated rollback gates.
32. **Feature Store Centralization**: Store computed offline batch and online streaming features in a centralized feature store (Feast/Tecton).
33. **Modular Code Architecture**: Separate ML codebases cleanly into distinct modules: data loading, model architecture, training loop, and evaluation.
34. **Avoid Hardcoded Magic Numbers**: Extract magic numbers, thresholds, and paths into external YAML configuration files.
35. **Fairness Auditing**: Evaluate model performance metrics across protected demographic sub-groups to audit for disparate impact.
36. **Task-Specific Augmentation**: Apply domain-appropriate data augmentations that preserve original label semantics.
37. **Human Baseline Benchmark**: Compare ML model evaluation metrics against human expert performance benchmarks where available.
38. **Inference Context Managers**: Wrap PyTorch inference calls inside `with torch.no_grad():` to disable autograd memory overhead.
39. **Deterministic GPU Operations**: Set `torch.backends.cudnn.deterministic = True` for strict scientific reproducibility across GPU runs.
40. **Prediction Logging**: Log production input payloads alongside predictions to asynchronous storage for post-hoc debugging.
41. **Model Complexity Scaling**: Match model parameter capacity to available training dataset size to prevent underfitting or memorization.
42. **Distributed Training Choice**: Prefer `DistributedDataParallel` (DDP) over legacy `DataParallel` for multi-GPU training due to multi-process efficiency.
43. **Code Review Standards**: Subject ML code, data transformations, and SQL queries to standard peer code reviews.
44. **Spurious Correlation Auditing**: Use attribution methods (Integrated Gradients, SHAP) to confirm models learn true signal rather than background noise.
45. **Configuration Decoupling**: Pass hyperparameter configs via structured dataclasses or OmegaConf objects.
46. **Probability Calibration**: Apply Platt Scaling or Isotonic Regression if predicted probabilities directly drive business decision thresholds.
47. **Image Normalization Standards**: Standardize vision model input tensors using dataset-specific RGB channel mean and standard deviation.
48. **Microservice Isolation**: Containerize model inference code in standalone lightweight REST microservices.
49. **Multimodal Late Fusion**: Fuse different modalities (image + text) using projection layers into a shared embedding space before final classification heads.
50. **Unit Testing Pipeline**: Write automated unit tests for data cleaning logic, input tensor shapes, loss calculations, and API response schemas.

---

## 9. Optimization Questions

### 50 Model & System Optimization Techniques
1. **Automatic Mixed Precision (AMP)**: Speed up PyTorch GPU training by using FP16/BF16 tensor core operations via `torch.cuda.amp.autocast()`.
2. **Gradient Accumulation**: Simulate large mini-batch sizes on memory-constrained GPUs by accumulating gradients over $N$ steps before calling `optimizer.step()`.
3. **Gradient Checkpointing**: Trade compute for GPU memory by discarding intermediate activations during forward pass and recomputing them during backprop (`torch.utils.checkpoint`).
4. **AdamW vs SGD Optimizer Choice**: Use AdamW for fast convergence on Transformers/NLP; use SGD with Momentum for optimal out-of-distribution generalization on CNNs.
5. **Learning Rate Warmup**: Linearly ramp up learning rate over initial steps to stabilize early gradient updates in deep Transformers.
6. **Batch Size Tuning**: Scale learning rate linearly or via square root rule when increasing mini-batch size ($\eta' = \eta \times \sqrt{B'/B}$).
7. **On-the-Fly Data Augmentation**: Apply stochastic image transforms on CPU workers during data loading to avoid disk space overhead.
8. **PyTorch Profiler Bottleneck Identification**: Locate pipeline bottlenecks (CPU data loading vs GPU kernel execution) using `torch.profiler`.
9. **DataLoader Tuning**: Optimize GPU ingestion speed by setting `num_workers = 4 * num_gpus` and enabling `pin_memory = True`.
10. **ONNX Runtime Acceleration**: Export PyTorch/TensorFlow models to ONNX graph format to achieve $2-4\times$ faster CPU/GPU inference execution.
11. **Model Quantization (INT8)**: Quantize model weights and activations from FP32 to INT8 to reduce memory footprint by $75\%$ and accelerate inference speed.
12. **Magnitude Model Pruning**: Zero-out small magnitude weights to create sparse models suitable for compressed storage and sparse inference runtime.
13. **Knowledge Distillation**: Train a lightweight student model to match probability outputs of a massive teacher model, achieving faster inference with minimal accuracy drop.
14. **Depthwise Separable Convolutions**: Replace standard 3D convolutions with Depthwise Separable Convolutions (MobileNet) to reduce parameters and FLOPs by $8-9\times$.
15. **Bayesian Hyperparameter Optimization**: Use Gaussian Process acquisition functions (Optuna) to search hyperparameter spaces in $10\times$ fewer trials than Grid Search.
16. **Feature Pre-computation Caching**: Compute and save heavy static embeddings (e.g. BERT text representations) offline to avoid recomputing during iterative downstream model training.
17. **Distributed Data Parallel (DDP)**: Use PyTorch DDP to spawn single process per GPU and execute efficient AllReduce gradient synchronization.
18. **Cosine Annealing LR Scheduler**: Decay learning rate smoothly following a cosine curve to settle into wide, robust local minima.
19. **Transfer Learning Acceleration**: Freeze pre-trained feature extraction layers to reduce trainable parameters and speed up epoch training time by $80\%$.
20. **Kaiming / He Initialization Speedup**: Initialize ReLU network weights with He initialization to prevent vanishing/exploding gradients and accelerate initial convergence.
21. **8-Bit Optimizers (`bitsandbytes`)**: Store Adam optimizer momentum and variance states in 8-bit precision to save $75\%$ of optimizer memory.
22. **PyTorch Graph Compilation (`torch.compile`)**: Fuse PyTorch operation kernels into optimized CUDA kernels using TorchDynamo and Triton compiler.
23. **Data vs Model Capacity Scaling**: Prioritize acquiring more quality labeled data over expanding model parameters when operating in high-variance regimes.
24. **Streaming Data Pipeline Optimization**: Use Apache Arrow and parquet columnar storage for high-throughput sub-millisecond feature extraction.
25. **BatchNorm Training Acceleration**: Use Batch Normalization to smooth the optimization landscape, allowing $5-10\times$ higher learning rates.
26. **Large LR Minimum Escaping**: Use higher initial learning rates to bounce out of narrow, sharp local minima into broad, flat generalization regions.
27. **Early Stopping Compute Savings**: Halt unpromising hyperparameter trial runs early using Hyperband / Successive Halving algorithms.
28. **Evaluation Frequency Tuning**: Run validation checks every $N$ steps (e.g. every 500 steps) rather than every epoch on massive datasets to save compute overhead.
29. **DataLoader Worker Optimization**: Audit data loader worker counts to prevent CPU thread thrashing and memory paging issues.
30. **DDP vs DataParallel**: Always select `DistributedDataParallel` over legacy `DataParallel` to avoid Python GIL single-process bottlenecks.
31. **LARS / LAMB for Large Batch Training**: Apply Layer-wise Adaptive Rate Scaling to stabilize training with extreme mini-batch sizes ($>16,000$).
32. **MobileViT Architecture**: Combine lightweight CNNs with spatial Transformers to enable vision Transformer execution on mobile devices.
33. **DistilBERT Compression**: Distill BERT-base into DistilBERT, retaining $97\%$ of language understanding capabilities with $40\%$ fewer parameters and $60\%$ faster speed.
34. **Automatic Mixed Precision Autocasting**: Wrap forward pass operations in `torch.cuda.amp.autocast()` and scale loss using `GradScaler`.
35. **Sequence Packing / Padding**: Pack variable-length sequences into unified flattened tensors to eliminate wasted computation on zero-padding tokens.
36. **Neural Architecture Search (NAS)**: Automate discovery of optimal Pareto-efficient model sub-networks under strict latency and memory constraints.
37. **Structured Channel Pruning**: Prune full convolutional channels or attention heads to achieve real hardware speedups without sparse matrix hardware support.
38. **DDP Gradient Compression**: Compress gradient tensors (FP16 / PowerSGD) before network transmission to reduce multi-node communication latency.
39. **Memory-Bound vs Compute-Bound Profiling**: Profile arithmetic intensity (FLOPs per byte transferred) to determine whether model performance is bound by GPU memory bandwidth or compute TFLOPS.
40. **Operator Fusion**: Combine sequential operations (e.g. Conv + BatchNorm + ReLU) into a single fused GPU kernel to eliminate intermediate memory read/writes.
41. **Fused AdamW Optimizer**: Use `torch.optim.AdamW(fused=True)` to execute single combined CUDA kernel for parameter updates.
42. **Disk Caching for Data Generators**: Cache preprocessed dataset transformations to local NVMe SSD storage (`dataset.cache()`).
43. **Progressive Resizing Training**: Train vision models initially on small resolution images ($128\times128$) and progressively increase to full resolution ($224\times224$) to speed up early epochs.
44. **Random Search Efficiency**: Prefer Random Search over Grid Search for hyperparameter exploration as it samples important parameters more efficiently.
45. **Approximate Nearest Neighbor (ANN) Indexing**: Replace exact brute-force vector search ($O(N)$) with HNSW or IVF-PQ vector indexing ($O(\log N)$) for real-time recommendation retrieval.
46. **Histogram-Based XGBoost (`tree_method='hist'`)**: Bin continuous features into integer histograms to speed up tree split building by $10-20\times$ on large datasets.
47. **LightGBM GOSS Sampling**: Retain all samples with large gradients and randomly sample instances with small gradients to accelerate boosting split calculations.
48. **CatBoost Ordered Boosting**: Prevent target leakage and overfitting on categorical datasets using ordered target encoding transformations.
49. **NumPy Array Vectorization**: Eliminate Python `for` loops in custom loss and metric functions using vectorized NumPy array operations.
50. **Student-Teacher Distillation Ratio**: Set distillation loss balance $\alpha \mathcal{L}_{\text{soft}} + (1-\alpha) \mathcal{L}_{\text{hard}}$ with temperature $T=4.0$ to distill deep model representations effectively.

---

## 10. Advanced & Research Questions

### 50 Advanced, Research-Level & Staff-Level Questions
1. **Score-Based Diffusion Mathematics**: Formulate the stochastic differential equation (SDE) framework connecting Denoising Score Matching, Langevin Dynamics, and Continuous DDPMs.
2. **RLHF PPO Formulation**: Formulate the objective function of PPO in RLHF alignment: $\max_\theta \mathbb{E} [ r_\phi(x,y) - \beta D_{\text{KL}}(\pi_\theta(y|x) \parallel \pi_{\text{ref}}(y|x)) ]$.
3. **Multimodal LLM Architecture**: Architect a Vision-Language foundation model (e.g. LLaVA): CLIP ViT Image Encoder $\to$ Linear Projection Projection Matrix $\to$ Autoregressive Llama LLM.
4. **Mixture-of-Experts Gating & Load Balancing**: Formulate Top-$k$ Router Gating $G(x) = \text{Softmax}(\text{TopK}(H(x), k))$ and detail auxiliary load-balancing loss preventing expert collapse.
5. **Continual Learning & EWC**: Mathematically explain Elastic Weight Consolidation (EWC) loss $\mathcal{L}(\theta) = \mathcal{L}_B(\theta) + \sum \frac{\lambda}{2} F_i (\theta_i - \theta_{A, i}^*)^2$ using the Fisher Information Matrix.
6. **Lottery Ticket Hypothesis Verification**: Step-by-step algorithm for identifying winning lottery tickets: Train network $\to$ Prune smallest $p\%$ weights $\to$ Reset remaining weights to original random initialization $W_0$.
7. **Autoregressive vs Diffusion Image Generation**: Compare Discrete Vector Quantized Token Autoregressive models (ImageGPT, VQGAN) vs Continuous Denoising Diffusion Probabilistic Models (Stable Diffusion).
8. **FlashAttention Online Softmax Tiling**: Derive the online softmax rescaling formula enabling attention block updates without storing the full $N \times N$ matrix in GPU HBM.
9. **Chinchilla Compute-Optimal Scaling Laws**: Formulate the empirical power-law relations $L(N, D) = \frac{A}{N^\alpha} + \frac{B}{D^\beta} + E$ and explain why model parameters and training tokens should scale at equal rates.
10. **GELU Activation Mathematical Intuition**: Formulate GELU $\text{GELU}(x) = x \cdot \Phi(x) = x \cdot P(X \le x)$ where $X \sim \mathcal{N}(0, 1)$ and explain its smooth probabilistic gating property.
11. **Sequence Packing in FlashAttention**: Detail how flash-attention variable length sequence packing eliminates zero-padding memory consumption and wasted FLOPs.
12. **Perceiver Architecture Dual Attention**: Explain how the Perceiver architecture uses cross-attention to project high-dimensional inputs ($N \times D$) into a small latent bottleneck ($M \times d$ where $M \ll N$) to bypass Transformer $O(N^2)$ scaling.
13. **Graph Attention Networks (GAT)**: Formulate the masked self-attention spatial convolution mechanism across node neighbors in Graph Attention Networks.
14. **Neural Radiance Fields (NeRF)**: Explain how NeRF maps 5D spatial coordinates $(x, y, z, \theta, \phi)$ to color and volume density $(r, g, b, \sigma)$ using positional encoding and MLPs.
15. **CPC vs MAE Self-Supervised Learning**: Compare Contrastive Predictive Coding (predicting future sequence representations) vs Masked Autoencoding (reconstructing masked raw input patches).
16. **Prompt Tuning vs Prefix Tuning**: Contrast Prompt Tuning (prepended trainable input embedding vectors) vs Prefix Tuning (prepended trainable Key/Value prefix matrices inside every self-attention layer).
17. **QLoRA Quantization Mechanics**: Detail the 4 innovations in QLoRA: 4-bit NormalFloat (NF4) data type, Double Quantization, Paged Optimizers, and FP16 LoRA adapters.
18. **Causal Inference Difference-in-Differences**: Formulate a Difference-in-Differences (DiD) regression estimator to measure causal treatment effects while controlling for unobserved time-invariant confounders.
19. **Federated Learning with Differential Privacy**: Explain how local differential privacy (adding Gaussian noise to local parameter updates prior to transmission) protects user privacy in FedAvg.
20. **DETR Hungarian Loss Formulation**: Formulate the bipartite matching loss using Hungarian Algorithm matching predicted bounding boxes to ground truth annotations in DETR.
21. **Neural Tangent Kernel Infinite Limit**: Explain how overparameterized neural networks behave as linear models governed by a constant kernel (NTK) during gradient descent as width $\to \infty$.
22. **Softmax Simplex Calibration**: Explain how the mathematical constraint $\sum p_i = 1$ on the probability simplex forces overconfidence in un-calibrated deep neural networks.
23. **Offline Reinforcement Learning**: Explain the distribution shift challenge in Offline RL and how Conservative $Q$-Learning (CQL) penalizes $Q$-values on out-of-distribution actions.
24. **World Models in Model-Based RL**: Architect a World Model consisting of a Vision VAE (represents state), Recurrent MDN-RNN (predicts future states), and Controller (selects actions).
25. **Denoising Score Matching Equivalence**: Prove that learning to denoise samples with score matching is equivalent to estimating the score function $\nabla_x \log p(x)$ of the data distribution.
26. **Attention Entropy & Uncertainty**: How can self-attention matrix entropy $H(A) = -\sum a_{ij} \log a_{ij}$ be used to measure model confidence and prune redundant heads?
27. **Token Merging (ToMe) for ViT**: Explain how Token Merging uses bipartite graph matching to merge similar visual tokens during ViT forward passes without retraining.
28. **Group Normalization Independence**: Formulate Group Normalization equations and explain why dividing channels into independent groups bypasses Batch Normalization dependency on batch size.
29. **Distance-Based Self-Supervised Learning**: Explain how non-contrastive self-supervised algorithms (BYOL, SimSiam) prevent representation collapse without explicit negative sample pairs.
30. **Spiking Neural Networks (SNNs)**: Contrast event-driven temporal dynamics of Spiking Neural Networks using Leaky Integrate-and-Fire (LIF) neurons against continuous Artificial Neural Networks.
31. **Retentive Networks (RetNet)**: Explain how RetNet replaces Softmax attention with a retention mechanism supporting dual representations: parallel for training and recurrent for low-latency inference.
32. **Test-Time Adaptation (TTA)**: How does TTA update BatchNorm running statistics or fine-tune affine parameters on unlabeled test streams using entropy minimization (TENT)?
33. **Llama Architectural Innovations**: Detail the architectural modifications in Llama: Pre-Layer Normalization (RMSNorm), SwiGLU activation function, and Rotary Position Embeddings (RoPE).
34. **Semantic Entropy Hallucination Detection**: Explain how Semantic Entropy computes cluster entropy over multiple LLM generations to distinguish linguistic variation from true factual hallucinations.
35. **Constrained Decoding with FSMs**: How do Finite State Machines (FSMs) and Context-Free Grammars mask invalid token logits during LLM autoregressive decoding to guarantee valid JSON/SQL output?
36. **Energy-Based Models vs Diffusion**: Derive the mathematical link between un-normalized energy function gradients $-\nabla_x E_\theta(x)$ and score-based diffusion model noise predictors.
37. **Model Soup Weight Averaging**: Explain how Model Soup averages weight parameters of multiple models fine-tuned from the same pre-trained initialization to improve out-of-distribution generalization without inference latency penalty.
38. **Adversarial Training Formulations**: Formulate the Minimax Adversarial Training objective $\min_\theta \mathbb{E} [ \max_{\|\delta\| \le \epsilon} L(\theta, x + \delta, y) ]$ using Projected Gradient Descent (PGD).
39. **Grokking Phenomenon in Deep Nets**: Explain the "Grokking" phenomenon where validation accuracy suddenly jumps from random chance to $100\%$ long after training loss has reached near-zero overfitting.
40. **LLM Training Data Memorization**: Explain verbatim memorization in LLMs, privacy risks under membership inference attacks, and data deduplication mitigation techniques.
41. **In-Context Learning as Implicit Meta-Learning**: Discuss research hypotheses proposing that In-Context Learning during forward passes acts as implicit gradient descent on virtual activation states.
42. **Fourier Positional Features in NeRF**: Explain why mapping 3D input coordinates $x$ to high-frequency Fourier features $\gamma(x) = (\sin(2^0 \pi x), \cos(2^0 \pi x), \dots)$ enables MLPs to learn high-frequency spatial details.
43. **Switch Transformer Gating**: Detail the Switch Transformer router allocating each token to a single top-1 expert, and explain how capacity factor hyperparameter manages expert buffer overflow.
44. **Knowledge Editing in LLMs (ROME)**: Detail Rank-One Model Editing (ROME) for modifying specific factual associations in pre-trained LLM feed-forward weight matrices without full retraining.
45. **SPIN (Self-Play Fine-Tuning)**: Explain how SPIN allows an LLM to improve its own performance through self-play by generating synthetic responses and evaluating preference against prior iteration checkpoints.
46. **Direct Preference Optimization (DPO) Derivation**: Prove mathematically how DPO derives the closed-form implicit reward function $r(x,y) = \beta \log \frac{\pi_\theta(y|x)}{\pi_{\text{ref}}(y|x)}$ to eliminate explicit reward model training.
47. **Tree of Thoughts (ToT) Framework**: Architect a Tree of Thoughts reasoning loop combining LLM thought generators, state evaluators, and Breadth-First Search (BFS) / Depth-First Search (DFS) path exploration algorithms.
48. **Dense-Sparse Hybrid Retrieval Architecture**: Detail hybrid search scoring combining sparse lexical BM25 scores and dense vector similarity scores via Reciprocal Rank Fusion (RRF).
49. **Mixup Data Regularization**: Formulate Mixup data augmentation $\tilde{x} = \lambda x_i + (1-\lambda) x_j, \tilde{y} = \lambda y_i + (1-\lambda) y_j$ where $\lambda \sim \text{Beta}(\alpha, \alpha)$ and explain its linear interpolative regularization.
50. **Watermarking AI-Generated Text**: Explain how LLM output watermarking injects pseudo-random green/red list logit bias rules during autoregressive sampling to enable statistical detection of generated text.
