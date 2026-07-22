# 🏢 Real Company ML Interview Questions & Hiring Patterns

This document details curated Machine Learning interview questions and hiring themes based on patterns observed across **Big Tech (Google, Meta, Amazon, Apple, Microsoft)**, **AI Research Labs (OpenAI, Anthropic, DeepMind)**, **Streaming & Ad Tech (Netflix, Spotify, ByteDance)**, and **Ride-Hailing & Logistics (Uber, DoorDash, Swiggy)**.

---

## 📌 Table of Contents

1. [AI Research Labs & AI Infrastructure (OpenAI, Anthropic, DeepMind, Nvidia)](#1-ai-research-labs--ai-infrastructure)
2. [Big Tech Search & Ad Ranking (Google, Meta, Microsoft)](#2-big-tech-search--ad-ranking)
3. [E-Commerce & Recommendation Systems (Amazon, Netflix, Spotify)](#3-e-commerce--recommendation-systems)
4. [Autonomous Systems & Vision (Tesla, Waymo, Apple)](#4-autonomous-systems--vision)
5. [Logistics, Fraud & Financial ML (Uber, DoorDash, Stripe, Razorpay)](#5-logistics-fraud--financial-ml)

---

## 1. AI Research Labs & AI Infrastructure

### Question 1: Self-Attention Memory & FlashAttention Mechanics
- **Commonly Asked By**: OpenAI, Anthropic, DeepMind, Nvidia
- **Why This Question Matters**: Evaluates whether candidates understand GPU memory hierarchy (HBM vs SRAM) and hardware bottlenecks in scaling LLMs.
- **Expected High-Score Answer**:
  Standard self-attention computes and stores intermediate attention matrix $S = Q K^T \in \mathbb{R}^{N \times N}$ in High Bandwidth Memory (HBM), incurring $O(N^2)$ memory reads/writes. **FlashAttention** reorganizes the computation using tiling: it loads blocks of $Q, K, V$ from HBM into fast SRAM, computes local softmax incrementally using online softmax normalization, and writes the output back without materializing the full $N \times N$ attention matrix in HBM. Reduces memory IO complexity from $O(N^2)$ to $O(N)$.
- **Follow-up Questions**: How does FlashAttention-2 improve warp-level parallelization across GPU SMs?
- **Interview Tips**: Draw GPU memory hierarchy (Global Memory / HBM vs L1/L2 Cache vs SRAM) to demonstrate deep hardware awareness.

---

### Question 2: RLHF vs. DPO in LLM Alignment
- **Commonly Asked By**: OpenAI, Anthropic, Google DeepMind
- **Why This Question Matters**: Direct Preference Optimization (DPO) has emerged as a lightweight alternative to traditional RLHF with PPO.
- **Expected High-Score Answer**:
  - **RLHF**: Trains a separate Reward Model $r_\phi(x,y)$ on human preference pairs $(y_w, y_l)$, then optimizes policy $\pi_\theta(y|x)$ using PPO with a KL-divergence penalty against reference policy $\pi_{\text{ref}}$. Complex, stable training requires 4 active models in memory (Actor, Critic, Reward, Reference).
  - **DPO**: Mathematically reparameterizes the reward function as $r(x,y) = \beta \log \frac{\pi_\theta(y|x)}{\pi_{\text{ref}}(y|x)}$. Optimizes policy directly on preference pairs using binary cross-entropy loss without explicit reward model training or PPO actor-critic sampling.
- **Follow-up Questions**: What are the failure modes of DPO when preference pairs contain noise?

---

## 2. Big Tech Search & Ad Ranking

### Question 3: Click-Through Rate (CTR) Prediction under Severe Class Imbalance
- **Commonly Asked By**: Google, Meta, Microsoft, ByteDance
- **Why This Question Matters**: Ad click rates are typically $<1\%$, creating massive class imbalance in multi-billion row daily pipelines.
- **Expected High-Score Answer**:
  1. **Architecture**: Deep & Cross Network (DCN) or DeepFM combining explicit feature cross-interactions with deep representations.
  2. **Loss Function**: Focal Loss or Weighted Binary Cross-Entropy with negative down-sampling.
  3. **Calibration**: If down-sampling negative instances by ratio $\beta$, raw predictions $p'$ must be calibrated to true probability $p$:
     $$p = \frac{p'}{p' + \frac{1 - p'}{\beta}}$$
- **Follow-up Questions**: Why is log-loss calibration vital for downstream ad auction bidding?

---

## 3. E-Commerce & Recommendation Systems

### Question 4: Two-Tower Architecture for Real-Time Candidate Retrieval
- **Commonly Asked By**: Amazon, Netflix, Spotify, Pinterest
- **Why This Question Matters**: Millions of items must be filtered down to top-100 candidates in under 20 milliseconds.
- **Expected High-Score Answer**:
  Use a Two-Tower Deep Neural Network:
  - **User Tower**: Maps user features (history, demographic, context) to user embedding vector $\mathbf{u} \in \mathbb{R}^d$.
  - **Item Tower**: Maps item features (title, category, tags) to item embedding vector $\mathbf{v} \in \mathbb{R}^d$.
  - **Offline**: Pre-compute item embeddings for all millions of items and index them into an Approximate Nearest Neighbor (ANN) vector index (FAISS, HNSW).
  - **Online**: Compute User embedding $\mathbf{u}$ on the fly and perform $O(\log N)$ ANN vector search to retrieve top candidate IDs.
- **Follow-up Questions**: How do you handle the "Cold-Start" problem for newly added items?

---

## 4. Autonomous Systems & Vision

### Question 5: 3D Object Detection & Sensor Fusion
- **Commonly Asked By**: Tesla, Waymo, Apple, Cruise
- **Why This Question Matters**: Tests multi-modal feature fusion and spatial geometry transformation.
- **Expected High-Score Answer**:
  Combine 2D Camera images and 3D LiDAR point clouds.
  - **BEV (Bird's-Eye-View) Projection**: Transform 2D perspective image features into 3D voxel space using camera intrinsic and extrinsic calibration matrices.
  - **Fusion Architecture**: Transformer cross-attention mechanism queries spatial BEV features across temporal frames to predict bounding boxes, velocities, and trajectories.
- **Follow-up Questions**: How do you maintain temporal consistency across occlusion events?

---

## 5. Logistics, Fraud & Financial ML

### Question 6: Real-Time Fraud Detection & Data Drift Handling
- **Commonly Asked By**: Stripe, Razorpay, Uber, PayPal
- **Why This Question Matters**: Financial fraud patterns evolve rapidly to bypass existing rule engines and static models.
- **Expected High-Score Answer**:
  - **Feature Store**: Maintain real-time streaming feature aggregations (e.g. number of transactions in last 5 mins) using Flink/Redis.
  - **Model Architecture**: XGBoost / LightGBM baseline coupled with Graph Neural Networks (GNNs) to detect fraud rings.
  - **Drift Monitoring**: Monitor Population Stability Index (PSI) and Kolmogorov-Smirnov (KS) tests on input features hourly. Trigger automated shadow deployment pipelines when PSI $> 0.25$.
- **Follow-up Questions**: How do you handle label latency when fraud confirmation takes 30 days?
