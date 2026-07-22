# 📖 Generative AI & LLM Comprehensive Interview Guide

This guide is organized into three progressive levels: **Beginner**, **Intermediate**, and **Advanced**. Each section breaks down theoretical concepts, mathematical foundations, coding patterns, real-world interview expectations, and follow-up questions.

---

## 🟢 Section 1: Beginner Level (Foundations)

### 1. Transformer Architecture & Self-Attention

#### Conceptual Overview
The Transformer architecture (Vaswani et al., 2017) discarded recurrent and convolutional networks in favor of pure **Self-Attention**. It computes direct relationships between all pairs of tokens in a sequence simultaneously, eliminating sequential bottlenecking and allowing massive parallelization during training.

```mermaid
graph TD
    Input[Input Tokens] --> Embed[Embedding + Positional Encoding]
    Embed --> QKV[Linear Projections: Q, K, V]
    QKV --> MatMul1[Q x K^T / sqrt(d_k)]
    MatMul1 --> Mask[Causal / Padding Mask]
    Mask --> Softmax[Softmax]
    Softmax --> MatMul2[Attention Weights x V]
    MatMul2 --> OutLinear[Output Linear Projection]
```

#### Scaled Dot-Product Attention Equation
$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

Where:
- $Q \in \mathbb{R}^{N \times d_k}$: Query matrix representing the target sequence tokens.
- $K \in \mathbb{R}^{M \times d_k}$: Key matrix representing the input sequence tokens.
- $V \in \mathbb{R}^{M \times d_v}$: Value matrix containing contextual representations.
- $\frac{1}{\sqrt{d_k}}$: Scaling factor to prevent large dot products from pushing softmax logits into regions with vanishing gradients.

#### Code Implementation: Scaled Dot-Product Attention in PyTorch
```python
import torch
import torch.nn as nn
import torch.nn.functional as F

def scaled_dot_product_attention(Q, K, V, mask=None):
    """
    Q, K, V shape: (batch_size, num_heads, seq_len, d_k)
    """
    d_k = Q.size(-1)
    scores = torch.matmul(Q, K.transpose(-2, -1)) / (d_k ** 0.5)
    
    if mask is not None:
        scores = scores.masked_fill(mask == 0, float('-inf'))
        
    attn_weights = F.softmax(scores, dim=-1)
    output = torch.matmul(attn_weights, V)
    return output, attn_weights
```

---

### 2. Tokenization & Subword Algorithms

Subword tokenization bridges raw text and numerical inputs. It balances vocabulary size and out-of-vocabulary (OOV) representation.

| Tokenizer | Algorithm | Key Characteristics | Usage |
| :--- | :--- | :--- | :--- |
| **BPE (Byte Pair Encoding)** | Iteratively merges most frequent character pairs | Frequency-driven, handles arbitrary subwords | GPT-2, GPT-4, Llama |
| **WordPiece** | Merges pairs maximizing likelihood of training data | Likelihood-driven | BERT |
| **SentencePiece** | Treats input as raw byte stream (includes spaces ` `) | Language-agnostic, whitespace-independent | T5, Llama, Mistral |

---

### 3. Beginner Expected Interview Questions & Follow-ups

#### Q1: Why is the scaling factor $\sqrt{d_k}$ necessary in scaled dot-product attention?
- **Answer**: Assuming Query $q$ and Key $k$ are independent random variables with mean 0 and variance 1, their dot product $q \cdot k = \sum_{i=1}^{d_k} q_i k_i$ has mean 0 and variance $d_k$. As $d_k$ grows large, the values of the dot product grow in magnitude, forcing the `softmax` function into regions with extremely small gradients (gradient vanishing). Dividing by $\sqrt{d_k}$ scales the variance back to 1.
- **Follow-up**: *What happens if you omit $\sqrt{d_k}$ during training?* The network suffers from slow convergence or unstable gradients early in pre-training.

---

## 🟡 Section 2: Intermediate Level (Core LLM & PEFT Engineering)

### 1. Encoder-Decoder vs. Decoder-Only vs. Encoder-Only

```mermaid
graph LR
    subgraph Encoder-Only (BERT)
        E1[Bidirectional Masked Attention]
    end
    subgraph Decoder-Only (GPT / Llama)
        D1[Causal Autoregressive Masking]
    end
    subgraph Encoder-Decoder (T5 / BART)
        ED1[Bidirectional Encoder] --> ED2[Cross-Attention Decoder]
    end
```

- **Encoder-Only (BERT)**: Uses bidirectional self-attention. Pre-trained via Masked Language Modeling (MLM). Ideal for classification, NER, and sentence embeddings.
- **Decoder-Only (GPT, Llama, Mistral)**: Uses causal masking (token $t$ only attends to tokens $\le t$). Pre-trained via Causal Language Modeling (CLM). Standard for autoregressive text generation.
- **Encoder-Decoder (T5, BART)**: Encoder processes full input bidirectionally; decoder generates autoregressively using cross-attention over encoder outputs. Ideal for translation and summarization.

---

### 2. Parameter-Efficient Fine-Tuning (PEFT) & LoRA / QLoRA

#### Low-Rank Adaptation (LoRA)
Full fine-tuning updates all parameters $W_0 \in \mathbb{R}^{d \times k}$. LoRA decomposes the weight update $\Delta W$ into two low-rank matrices $A$ and $B$:

$$W = W_0 + \Delta W = W_0 + \frac{\alpha}{r} (B \cdot A)$$

Where:
- $A \in \mathbb{R}^{r \times k}$, initialized with Gaussian distribution $\mathcal{N}(0, \sigma^2)$.
- $B \in \mathbb{R}^{d \times r}$, initialized to zero (ensuring $\Delta W = 0$ at start of training).
- $r \ll \min(d, k)$: Rank parameter (typically $r \in [8, 64]$).
- $\alpha$: Constant scaling hyperparameter.

```python
import torch
import torch.nn as nn

class LoRALinear(nn.Module):
    def __init__(self, base_layer: nn.Linear, r: int = 8, alpha: float = 16.0):
        super().__init__()
        self.base_layer = base_layer
        self.base_layer.weight.requires_grad = False  # Freeze base weights
        
        in_features = base_layer.in_features
        out_features = base_layer.out_features
        
        self.r = r
        self.alpha = alpha
        self.scaling = alpha / r
        
        self.lora_A = nn.Parameter(torch.zeros(r, in_features))
        self.lora_B = nn.Parameter(torch.zeros(out_features, r))
        
        nn.init.kaiming_uniform_(self.lora_A, a=math.sqrt(5))
        nn.init.zeros_(self.lora_B)

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        base_out = self.base_layer(x)
        lora_out = (x @ self.lora_A.T @ self.lora_B.T) * self.scaling
        return base_out + lora_out
```

#### QLoRA (Quantized LoRA) Innovations
1. **NormalFloat4 (NF4)**: Information-theoretically optimal quantile quantization data type for normally distributed weights.
2. **Double Quantization (DQ)**: Quantizes quantization constants themselves, saving ~0.37 bits per parameter.
3. **Paged Optimizers**: Uses CUDA Unified Memory to prevent memory spikes during gradient updates by swapping memory to CPU.

---

### 3. Retrieval-Augmented Generation (RAG) Architecture

```mermaid
graph TD
    Docs[Document Corpus] --> Chunk[Semantic Chunking]
    Chunk --> Embed[Embedding Model]
    Embed --> VDB[(Vector Database)]
    
    Query[User Query] --> EmbedQ[Embed Query]
    EmbedQ --> Search[Dense Vector Search: Top-K]
    Search --> Rerank[Cross-Encoder Reranker]
    Rerank --> Context[Top-N Relevant Contexts]
    Context --> LLM[LLM Generator]
    LLM --> Answer[Grounded Response]
```

#### Critical RAG Concepts
- **Chunking Strategies**: Fixed-size with overlap, Recursive character splitting, Semantic parsing (by headers/markdown).
- **Hybrid Search**: Combines Dense Retrieval (embeddings via cosine distance) with Sparse Retrieval (BM25 for exact keyword/entity matches) using Reciprocal Rank Fusion (RRF).
- **Re-ranking**: Cross-Encoders evaluate $(Query, Document)$ pairs jointly to score semantic relevance before passing to LLM context.

---

### 4. Intermediate Expected Interview Questions & Follow-ups

#### Q2: How does KV-Caching reduce autoregressive decoding time from $O(N^2)$ to $O(N)$ per token?
- **Answer**: During autoregressive text generation, at step $t$, the key vector $K_{\le t}$ and value vector $V_{\le t}$ of previous tokens do not change. Without caching, the model recomputes $K$ and $V$ for all previous $t-1$ tokens at every new step. KV-Cache stores $K$ and $V$ in GPU memory and appends only the newly computed $k_t$ and $v_t$, reducing matrix operations per step to a single token vector multiplication.
- **Follow-up**: *What is the GPU memory consumption of KV-Cache for a 70B parameter model?*
  $$\text{Memory}_{\text{KV}} = 2 \times b \times s \times l \times h \times d_{\text{head}} \times \text{bytes\_per\_elem}$$
  For Batch $b=1$, Seq Len $s=4096$, Layers $l=80$, Heads $h=64$, Head Dim $d=128$, FP16 (2 bytes):
  $$2 \times 1 \times 4096 \times 80 \times 64 \times 128 \times 2 \approx 10.73 \text{ GB of VRAM per sequence!}$$

---

## 🔴 Section 3: Advanced Level (System Architecture, Optimization & Alignment)

### 1. Advanced Attention Variants: MHA vs MQA vs GQA

```
Multi-Head Attention (MHA)    Multi-Query Attention (MQA)    Grouped-Query Attention (GQA)
Q Q Q Q  K K K K  V V V V      Q Q Q Q  K  V                   Q Q Q Q  K K  V V
 (8 Query, 8 Key, 8 Value)      (8 Query, 1 Key, 1 Value)       (8 Query, 2 Key, 2 Value)
```

- **MHA**: Each query head has a dedicated Key and Value head. High accuracy, maximum KV-cache footprint.
- **MQA**: All query heads share a single Key and Value head. 8x-64x KV-cache memory reduction, slight quality degradation.
- **GQA (Llama-3, Mistral)**: Groups query heads into $G$ partitions, each sharing a Key/Value head. Achieves MHA quality at near-MQA speed.

---

### 2. High-Performance Inference Engines: vLLM & PagedAttention

Traditional KV-cache allocates contiguous memory blocks based on `max_seq_len`, causing **60%–80% memory fragmentation** (internal and external).

**PagedAttention** (vLLM) resolves this by applying Virtual Memory Paging concepts to KV-caching:
- Divides KV-cache into fixed-size physical memory blocks.
- Uses a **Block Table** to map logical sequence blocks to non-contiguous physical GPU RAM blocks.
- Enables dynamic memory allocation, continuous batching, and instant memory sharing across beam search branches and copy-on-write requests.

---

### 3. Model Alignment: RLHF (PPO) vs. DPO

```mermaid
graph TD
    SFT[SFT Model] --> PairData[(Preference Pairs: y_w > y_l)]
    
    subgraph Traditional RLHF
        PairData --> RM[Train Reward Model r_phi]
        RM --> PPO[Optimize Policy pi_theta via PPO + KL Penalty]
    end
    
    subgraph Direct Preference Optimization (DPO)
        PairData --> DPO_Loss[Closed-Form DPO Loss Optimization]
    end
```

#### Direct Preference Optimization (DPO) Mathematical Derivation
DPO eliminates the explicit Reward Model and PPO reinforcement learning loop by reparameterizing the reward function $r(x, y)$ directly through the implicit language model policy $\pi_\theta$:

$$r(x, y) = \beta \log \frac{\pi_\theta(y|x)}{\pi_{\text{ref}}(y|x)}$$

Substituting into the Bradley-Terry preference model yields the objective:

$$\mathcal{L}_{\text{DPO}}(\theta; \pi_{\text{ref}}) = -\mathbb{E}_{(x, y_w, y_l)} \left[ \log \sigma \left( \beta \log \frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)} \right) \right]$$

Where $y_w$ is the winning (preferred) response and $y_l$ is the losing (dispreferred) response.

---

### 4. Advanced Expected Interview Questions & Senior Scenarios

#### Q3: Explain FlashAttention and why it yields 2x-4x speedups without changing the exact math of attention.
- **Answer**: Standard attention writes intermediate matrices $S = QK^T \in \mathbb{R}^{N \times N}$ and $P = \text{softmax}(S) \in \mathbb{R}^{N \times N}$ to High Bandwidth Memory (HBM) on the GPU, requiring $O(N^2)$ memory reads/writes. FlashAttention tiles the input matrices $Q, K, V$ into blocks that fit inside fast High-Speed SRAM (L1/L2 cache). Using online softmax normalization (Tiling algorithm), it computes attention iteratively block-by-block without ever materializing the $N \times N$ matrix in HBM, reducing memory access from $O(N^2)$ to $O(N)$.

#### Q4: System Design: How would you deploy a 70B parameter model with a latency SLA under 50ms per token for 1,000 concurrent users?
- **Strategy**:
  1. **Quantization**: Apply AWQ or GPTQ 4-bit quantization, shrinking model size from 140GB (FP16) to ~35GB, allowing model to fit on a single A100 (80GB) or 2x L40S GPUs.
  2. **KV-Cache Optimization**: Use **vLLM** with PagedAttention and Grouped-Query Attention (GQA).
  3. **Continuous Batching**: Use iteration-level batching (inflight batching) instead of request-level batching to prevent long prompts from stalling short completion streams.
  4. **Speculative Decoding**: Deploy a small 1B draft model to generate candidate token sequences, verified in parallel by the 70B target model in a single forward pass.
