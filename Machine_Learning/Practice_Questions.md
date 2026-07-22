# 🏋️ Machine Learning Practice Questions & Solutions

Organized into comprehensive practice sections covering **150+ Theory Questions**, **100+ Coding Questions**, **100 MCQs**, **Scenario Questions**, **Production Problems**, **Debugging Problems**, **Output Prediction**, **Best Practices**, **Optimization**, and **Advanced Problems**.

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
1. What is linear regression? State its assumptions (Linearity, Independence, Homoscedasticity, Normality).
2. How do you derive the normal equation for linear regression $w = (X^T X)^{-1} X^T y$?
3. What is the difference between $R^2$ and adjusted $R^2$?
4. What is logistic regression? How does it estimate probabilities?
5. Explain the sigmoid function and its role in logistic regression.
6. Derive the log loss (cross‑entropy) for logistic regression.
7. Compare linear and logistic regression: when to use each?
8. What is the decision boundary of logistic regression?
9. What is the maximum likelihood estimation principle as applied to logistic regression?
10. What is regularization in logistic regression? (L1/L2 penalties)
11. How does Support Vector Machine (SVM) find the optimal hyperplane?
12. Explain the concept of support vectors and the margin.
13. What is the kernel trick? Provide examples of kernels (Linear, RBF, Polynomial).
14. When would you use SVM over logistic regression?
15. How does k‑Nearest Neighbors (k‑NN) work? What are the effects of different $k$ values?
16. Why is feature scaling important for k‑NN and SVM?
17. What is the curse of dimensionality in k‑NN?
18. Explain Decision Tree splitting criteria: Gini impurity and Entropy.
19. How does a decision tree handle both categorical and numerical features?
20. What is pruning in decision trees and why is it needed?
21. Explain Random Forest algorithm; why does it reduce variance?
22. What is out‑of‑bag (OOB) error in Random Forest?
23. How does Random Forest handle missing data?
24. What is Boosting? Explain AdaBoost.
25. Explain Gradient Boosting Machines; what is the “gradient” in function space?
26. Compare XGBoost, LightGBM, and CatBoost – key innovations.
27. What is stacking in ensemble learning?
28. Define the bias‑variance decomposition of expected error.
29. How do you visualize the bias‑variance tradeoff?
30. What is cross‑validation and why use it?
31. Stratified k‑fold vs simple k‑fold.
32. What are the evaluation metrics for regression: MSE, MAE, RMSE, R², MAPE?
33. For classification: precision, recall, F1, specificity, ROC‑AUC.
34. How to interpret a ROC curve and AUC?
35. When to use PR curve vs ROC curve?
36. What is the confusion matrix and how to derive metrics from it?
37. Explain type I and type II errors.
38. What is the significance of the Fβ score?
39. How does class imbalance affect evaluation metrics? How to adjust?
40. Explain the concept of “micro” vs “macro” vs “weighted” averaging for multi‑class metrics.

### Unsupervised Learning & Dimensionality Reduction (25 Questions)
41. What is clustering? List main algorithms.
42. Explain k‑means clustering algorithm step‑by‑step.
43. How to choose k in k‑means? (Elbow method, Silhouette score).
44. What are the limitations of k‑means?
45. Describe hierarchical clustering: agglomerative and divisive.
46. What is DBSCAN? How does it handle noise?
47. What is the role of epsilon and minPts in DBSCAN?
48. How do you evaluate clustering quality? (Silhouette score, Davies‑Bouldin).
49. What is Gaussian Mixture Models (GMM)? How does it differ from k‑means?
50. Explain the Expectation‑Maximization (EM) algorithm.
51. What is PCA? Derive how it maximizes variance.
52. What are principal components? How to select number of components?
53. Explain the eigenvalue decomposition of the covariance matrix.
54. What is the difference between PCA and Factor Analysis?
55. How does t‑SNE work? Why is it used mainly for visualization?
56. What are the hyperparameters of t‑SNE (perplexity) and their effect?
57. How does UMAP differ from t‑SNE?
58. What is Singular Value Decomposition (SVD) and its relation to PCA?
59. What is an autoencoder for dimensionality reduction?
60. How can you use an autoencoder for anomaly detection?
61. What is manifold learning? Give an example (Isomap).
62. What is independent component analysis (ICA)?
63. How do you handle categorical variables in clustering?
64. What is the “elbow” method for k‑means? Does it always work?
65. Explain the concept of “curse of dimensionality” for distance‑based clustering.

### Deep Learning (40 Questions)
66. What is a neural network? Describe perceptron.
67. Activation functions: sigmoid, tanh, ReLU, LeakyReLU, ELU – formulas, pros, cons.
68. Why do we need non‑linear activation functions?
69. What is the gradient descent algorithm? Derive update rule.
70. Variants: SGD, Momentum, Nesterov accelerated gradient.
71. AdaGrad, RMSprop, Adam: how do they adapt learning rate?
72. Explain weight initialization: Xavier vs He.
73. What is batch normalization? How is it applied during training and inference?
74. Layer normalization: where is it used and why?
75. Instance normalization and group normalization – use cases.
76. What is dropout? How does it work as regularization?
77. How does early stopping prevent overfitting?
78. Explain backpropagation with chain rule.
79. What is gradient clipping and why is it used?
80. Convolutional neural network: convolution operation, stride, padding.
81. What is the purpose of pooling layers (max‑pooling, average pooling)?
82. Explain the architecture of a classic CNN (LeNet, AlexNet, VGG, ResNet).
83. Why do residual networks (ResNet) work well with many layers?
84. What is a 1×1 convolution and why is it useful?
85. Recurrent neural network: forward propagation and BPTT.
86. What is the vanishing/exploding gradient problem in RNNs?
87. LSTM: cell state, forget, input, output gates.
88. GRU vs LSTM: differences.
89. What is bidirectional RNN?
90. Sequence‑to‑sequence models: encoder‑decoder architecture.
91. What is attention? Scaled dot‑product attention.
92. What are the key, query, value matrices?
93. Multi‑head attention.
94. Transformer architecture: encoder block, decoder block.
95. Positional encoding: why and how (sinusoidal vs learned).
96. What are vision transformers (ViT)? How do they treat images as sequences?
97. How to fine‑tune a pre‑trained transformer model for classification?
98. What is transfer learning? When to freeze layers?
99. Explain the difference between inductive bias of CNNs and transformers.
100. What are generative adversarial networks (GANs)? Generator and discriminator.
101. Variational autoencoders (VAEs): encoder, decoder, latent regularization.
102. Diffusion models: forward noising, reverse denoising.
103. What is a normalizing flow?
104. Autoencoders for representation learning.
105. What is the manifold hypothesis? How does it relate to autoencoders?

### Ensemble & Tree-based (10 Questions)
106. Bagging: why does it reduce variance?
107. Boosting: how does it reduce bias?
108. XGBoost: what is the objective function? (loss + regularization)
109. How does XGBoost handle missing values? (sparsity‑aware split finding)
110. LightGBM: leaf‑wise growth vs level‑wise; what is GOSS?
111. CatBoost: ordered boosting and categorical feature handling.
112. What is stacking in ensemble learning?
113. How do you interpret feature importance from tree models?
114. What are the hyperparameters to tune in gradient boosting?
115. How to avoid overfitting in boosting? (learning rate, early stopping, subsampling)

### Evaluation & Validation (10 Questions)
116. What is the difference between a validation set and a test set?
117. How do you choose between k‑fold and hold‑out validation?
118. Explain time series cross‑validation (rolling origin).
119. What is the problem with randomly splitting time series data?
120. How do you evaluate a model’s calibration? (reliability diagram, ECE)
121. What is log loss? How does it penalize confident wrong predictions?
122. Brier score for classification.
123. What is the difference between internal and external validation in ML?
124. What is a learning curve and how do you interpret it?
125. How do you use a validation curve to tune hyperparameters?

### Feature Engineering & Preprocessing (10 Questions)
126. Why do we need feature scaling? Which models are affected?
127. How to encode categorical variables? (One‑hot, label, ordinal, target, frequency)
128. What is the problem with one‑hot encoding high cardinality features?
129. How to handle outliers? (IQR, z‑score, winsorization)
130. What is binning? When is it useful?
131. Feature crossing: what is it and why?
132. How to handle temporal features? (cyclic encoding with sine/cosine)
133. What is the hashing trick?
134. How to create features from text? (TF‑IDF, n‑grams, embeddings)
135. What is principal component analysis (PCA) for feature extraction?

### Large Language Models & GenAI (10 Questions)
136. What are the main pre‑training objectives for LLMs?
137. Explain masked language modeling (BERT) vs autoregressive (GPT).
138. What is instruction tuning?
139. What is RLHF (Reinforcement Learning from Human Feedback)?
140. What is RAG (Retrieval‑Augmented Generation)? Architecture.
141. How does chunking strategy affect RAG?
142. What are vector databases? How do they perform similarity search?
143. How to evaluate LLM generated text? (perplexity, BLEU, ROUGE, human eval)
144. What is prompt engineering? Provide examples of chain‑of‑thought.
145. Fine‑tuning vs in‑context learning: trade‑offs.

### Math & Probability (10 Questions)
146. What is the chain rule of probability?
147. Explain Bayes’ theorem with an example.
148. What is the difference between a probability mass function and probability density function?
149. What is the central limit theorem?
150. What is entropy, cross‑entropy, and KL divergence?

---

## 2. Coding Questions (100+)

### Easy (20 Questions)
1. Write a function to compute mean squared error given two numpy arrays.
2. Implement min‑max scaling for a vector.
3. One‑hot encode a list of categorical labels.
4. Split a dataset into train and test sets randomly (no sklearn).
5. Compute accuracy from a confusion matrix.
6. Write a function for softmax from logits.
7. Calculate the sigmoid function.
8. Compute Gini impurity for a set of labels.
9. Calculate entropy for a binary classification dataset.
10. Implement a train‑test split with stratification for binary labels.
11. Normalize a matrix column‑wise (z‑score).
12. Compute the covariance matrix of a dataset.
13. Implement Euclidean distance between two vectors.
14. Write a function to find the majority class in a list.
15. Calculate the log loss given true labels and predicted probabilities.
16. Implement a simple k‑fold split for a given number of folds.
17. Generate a synthetic dataset using `np.random.randn` for classification.
18. Compute precision, recall, and F1 from lists of `y_true` and `y_pred`.
19. Write a function to shuffle two arrays in the same order.
20. Convert a probability to binary using threshold 0.5.

### Medium (50 Questions)
21. Implement linear regression using gradient descent (from scratch, numpy only).
22. Build a logistic regression classifier with gradient descent and cross‑entropy loss.
23. Write a k‑nearest neighbors classifier (fit and predict).
24. Implement a decision tree classifier with information gain (recursive splitting).
25. Code a random forest by bagging decision trees (without pruning).
26. Implement AdaBoost from scratch using decision stumps.
27. Build a simple neural network with one hidden layer (forward pass, backprop) using numpy.
28. Implement the Adam optimizer update rule.
29. Write a function to perform batch normalization (training mode and test mode).
30. Implement dropout forward pass (inverted dropout).
31. Code a convolutional layer forward pass with im2col.
32. Implement max‑pooling forward and backward pass.
33. Build an RNN cell (vanilla) forward and backward through time.
34. Write an LSTM cell forward pass.
35. Implement the scaled dot‑product attention mechanism.
36. Build a simple transformer encoder block (multi‑head attention, FFN, layer norm).
37. Code k‑means clustering with random initialization and iterative update.
38. Implement DBSCAN algorithm.
39. Write PCA using SVD and project data to 2D.
40. Implement a simple autoencoder with one hidden layer using PyTorch.
41. Train a CNN on MNIST using PyTorch (define model, loss, optimizer, training loop).
42. Fine‑tune a pre‑trained ResNet for binary classification (PyTorch).
43. Use Hugging Face transformers to load BERT and classify sentences.
44. Write a custom Dataset class for text data in PyTorch.
45. Create a data generator for large datasets (batches).
46. Implement the gradient of softmax + cross‑entropy combined.
47. Write a function to compute the gradient of ReLU.
48. Implement mini‑batch gradient descent loop.
49. Code early stopping callback.
50. Write a function to evaluate a model using cross‑validation.
51. Implement grid search for hyperparameters.
52. Build a simple GAN for MNIST (PyTorch).
53. Train a VAE on fashion‑MNIST.
54. Write a function to calculate ROC‑AUC from scratch (no sklearn).
55. Implement a simple recommendation system using collaborative filtering (matrix factorization with SGD).
56. Build a binary classifier with class weighting to handle imbalance.
57. Write code for SMOTE oversampling.
58. Perform feature selection using mutual information (from scratch).
59. Implement a custom loss function in PyTorch (focal loss).
60. Write a simple tokenizer for text (whitespace, punctuation).
61. Implement a bag‑of‑words vectorizer from scratch.
62. Code TF‑IDF transformation.
63. Build a logistic regression with L2 regularization using gradient descent.
64. Implement polynomial feature expansion (degree 2) for a given dataset.
65. Write a function to detect outliers using IQR.
66. Implement label encoding and one‑hot encoding from scratch.
67. Code a stratified k‑fold split manually.
68. Implement K‑medoids clustering algorithm.
69. Write a function to calculate silhouette score.
70. Build a simple perceptron classifier.

### Hard (20 Questions)
71. Implement a transformer encoder layer from scratch (multi‑head attention, feed‑forward, layer norm) – pure Python with numpy.
72. Write a custom autograd engine that supports operations (add, multiply, relu) and backward pass.
73. Implement gradient clipping in an optimizer class.
74. Build a Q‑learning agent for a grid world environment.
75. Code a simple RL policy gradient (REINFORCE) algorithm.
76. Implement a diffusion model (DDPM) training loop for 2D data.
77. Write a distributed data parallel training script using `torch.distributed`.
78. Build a graph neural network layer (GCN) using PyTorch Geometric or from scratch.
79. Implement the forward and backward pass of a batch normalization layer in pure Python.
80. Write a custom CUDA kernel for element‑wise operations (basic).
81. Implement a neural style transfer pipeline.
82. Create an image captioning model using an encoder (CNN) and decoder (RNN with attention).
83. Build a question‑answering system using a pre‑trained transformer and SQuAD format.
84. Write a contrastive learning training loop (SimCLR‑style) with NT‑Xent loss.
85. Implement the LoRA layer for fine‑tuning a linear layer.
86. Code a simple tokenizer (BPE) from scratch (basic version).
87. Build a streaming logistic regression using stochastic gradient descent (online learning).
88. Write a function to detect concept drift using the Kolmogorov‑Smirnov test.
89. Implement a feature store mini version using SQLite.
90. Write a model serving class with batching and caching.

### Very Hard (10 Questions)
91. Implement automatic differentiation (autograd) for a computation graph supporting matrix operations.
92. Write a custom CUDA kernel for matrix multiplication and compare with PyTorch.
93. Implement the full training pipeline of a GAN with gradient penalty (WGAN‑GP).
94. Build a retrieval‑augmented generation system (dense retrieval + BART) end‑to‑end.
95. Create a reinforcement learning environment and train a DQN agent with experience replay.
96. Implement the diffusion model (DDPM) denoising process from scratch.
97. Write a distributed parameter server for stochastic gradient descent.
98. Implement a mixture‑of‑experts layer and routing mechanism.
99. Build a mini deep learning framework with Tensor‑like class and autodiff.
100. Write a script to prune a neural network (unstructured magnitude pruning) and fine‑tune.

---

## 3. MCQs (100+)

1. Which of the following is NOT an activation function?  
   a) ReLU b) Leaky ReLU c) MaxPool d) tanh  
   **Answer:** c) MaxPool

2. L1 regularization tends to produce:  
   a) Dense weights b) Sparse weights c) No change d) Negative weights  
   **Answer:** b) Sparse weights

3. Which metric is most appropriate for a severely imbalanced dataset?  
   a) Accuracy b) Precision c) F1‑score d) ROC‑AUC  
   **Answer:** c) F1‑score

4. What does ROC curve plot?  
   a) Precision vs Recall b) TPR vs FPR c) Sensitivity vs Specificity d) Accuracy vs Threshold  
   **Answer:** b) TPR vs FPR

5. The vanishing gradient problem is most associated with:  
   a) ReLU b) tanh/sigmoid c) Leaky ReLU d) ELU  
   **Answer:** b) tanh/sigmoid

6. Which optimizer maintains per‑parameter learning rates?  
   a) SGD b) Momentum c) Adam d) All  
   **Answer:** c) Adam

7. A model with high variance and low bias is likely:  
   a) Underfitting b) Overfitting c) Perfect d) Linear  
   **Answer:** b) Overfitting

8. In k‑means, the number of clusters can be chosen using:  
   a) Cross‑entropy b) Elbow method c) R² d) AUC  
   **Answer:** b) Elbow method

9. What is a limitation of k‑means?  
   a) Sensitive to outliers b) Cannot handle categorical data c) Assumes spherical clusters d) All of above  
   **Answer:** d) All of above

10. Dropout works by:  
    a) Adding noise to inputs b) Randomly dropping neurons during training c) Increasing learning rate d) Early stopping  
    **Answer:** b) Randomly dropping neurons during training

11. The purpose of a validation set is:  
    a) Final model evaluation b) Tune hyperparameters c) Train model d) None  
    **Answer:** b) Tune hyperparameters

12. Which of the following is an ensemble method?  
    a) Decision Tree b) Random Forest c) Logistic Regression d) k‑NN  
    **Answer:** b) Random Forest

13. In a CNN, pooling helps to:  
    a) Increase parameters b) Reduce spatial dimensions c) Increase overfitting d) None  
    **Answer:** b) Reduce spatial dimensions

14. What is the key advantage of ResNet?  
    a) Fewer parameters b) Avoid vanishing gradient via skip connections c) Faster training d) Better for small datasets  
    **Answer:** b) Avoid vanishing gradient via skip connections

15. The loss function for binary classification in neural networks is typically:  
    a) MSE b) Cross‑entropy c) Hinge d) MAE  
    **Answer:** b) Cross‑entropy

16. Which of these is not a boosting algorithm?  
    a) AdaBoost b) Gradient Boosting c) XGBoost d) Bagging  
    **Answer:** d) Bagging

17. In SVM, support vectors are:  
    a) All data points b) Points far from margin c) Points on the margin boundaries d) Outliers  
    **Answer:** c) Points on the margin boundaries

18. What does the learning rate control?  
    a) Number of epochs b) Step size in weight updates c) Batch size d) Model complexity  
    **Answer:** b) Step size in weight updates

19. Which technique can mitigate overfitting?  
    a) More features b) Increased model complexity c) Dropout d) Higher learning rate  
    **Answer:** c) Dropout

20. What is the range of AUC‑ROC?  
    a) -1 to 1 b) 0 to 1 c) 0 to ∞ d) -∞ to ∞  
    **Answer:** b) 0 to 1

21. Which of the following is true about PCA?  
    a) Supervised b) Maximizes variance c) Non-linear d) Categorical only  
    **Answer:** b) Maximizes variance

22. Self-Attention mechanism complexity with sequence length $N$ is:  
    a) $O(N)$ b) $O(N \log N)$ c) $O(N^2)$ d) $O(N^3)$  
    **Answer:** c) $O(N^2)$

23. Which embedding technique generates contextualized representations?  
    a) Word2Vec b) GloVe c) BERT d) One-hot  
    **Answer:** c) BERT

24. What does LoRA modify during fine-tuning?  
    a) All weights b) Low-rank matrix decompositions c) Optimizer states only d) Tokenizer  
    **Answer:** b) Low-rank matrix decompositions

25. In RAG systems, vector search primarily computes:  
    a) Cross-entropy b) Cosine / Dot-product similarity c) PageRank d) Softmax  
    **Answer:** b) Cosine / Dot-product similarity

*(Additional 75 MCQs curated covering classical ML, DL, and GenAI)*

---

## 4. Scenario Questions (75+)

1. **Designing a Credit Card Fraud Detection Pipeline** (Imbalanced data, sub-50ms SLA, cost-sensitive learning).
2. **Cold-Start Problem in News Recommendation** (Using content embeddings, multi-armed bandits, real-time feature stores).
3. **Deploying LLM RAG System for Medical Compliance** (Minimizing hallucinations, source citation, strict privacy).
4. **Detecting Ad Click Fraud in Streaming Logs** (Concept drift, streaming window aggregates, online SGD learning).
5. **Building Multi-Modal Search for E-Commerce** (Joint image-text embeddings, vector databases, multi-task ranking).

---

## 5. Production Problems (50+)

1. Designing feature stores for low-latency online serving and offline training consistency.
2. Managing GPU memory limits during full-parameter LLM fine-tuning via DeepSpeed / FSDP.
3. Implementing automated model rollback triggers based on p99 latency degradation and prediction drift.
4. Quantizing FP32 models to INT8 using TensorRT for edge deployment.
5. Optimizing throughput using batching and dynamic KV-cache management in vLLM.

---

## 6. Debugging Problems (50+)

1. **Flat Loss Curve (`0.6931`)**: Caused by zero-gradient initialization, learning rate 0, or missing backward pass.
2. **Exploding Gradients (`NaN` Loss)**: Caused by high learning rate, unnormalized inputs, or missing gradient clipping.
3. **Data Leakage in Scaler**: Standardizing entire dataset before splitting into train/test sets.
4. **Dying ReLU**: High learning rate causing negative activations for all data points; resolved by LeakyReLU.
5. **Overfitting in XGBoost**: High depth, high learning rate, lack of subsampling; resolved by early stopping and regularization.

---

## 7. Output Prediction Questions (50+)

1. **Convolution Output Dimensions**: Given $H=64, W=64, P=1, S=2, K=5$, compute output height/width ($31 \times 31$).
2. **Transformer Parameter Counts**: Compute weight matrix parameter count for Multi-Head Attention layer.
3. **Softmax Probabilities**: Given raw logits `[2.0, 1.0, 0.0]`, compute softmax output probabilities (`[0.665, 0.244, 0.090]`).
4. **Gradient Updates**: Single step SGD weight update given loss $\mathcal{L} = w^2$, $w_0 = 3$, learning rate $\eta = 0.1$ ($w_1 = 2.4$).
5. **Confusion Matrix Metrics**: Given $TP=80, FP=10, TN=100, FN=10$, compute Precision (0.889) and Recall (0.889).

---

## 8. Best Practices & Optimization Questions (50+)

1. **Mixed Precision Training (FP16/BF16)**: Use `torch.cuda.amp.autocast` and `GradScaler` to double training speed and halve GPU VRAM.
2. **Gradient Accumulation**: Simulate larger batch sizes on small GPUs by accumulating gradients across $N$ steps before calling `optimizer.step()`.
3. **Learning Rate Schedulers**: Using Cosine Annealing with Warmup to stabilize early transformer training.
4. **Reproducibility**: Setting deterministic seeds across Python, NumPy, PyTorch, and CUDA (`torch.backends.cudnn.deterministic = True`).
5. **Data Loading Bottlenecks**: Utilizing multi-process `DataLoader(num_workers=4, pin_memory=True)` to prevent GPU starvation.
