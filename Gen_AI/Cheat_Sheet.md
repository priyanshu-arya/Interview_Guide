# ⚡ Generative AI & LLMs Rapid Revision Cheat Sheet

*Revisable within 5 minutes before your interview.*

---

## 📐 Core Equations & Formulas

| Concept | Mathematical Equation | Key Variables & Notes |
| :--- | :--- | :--- |
| **Scaled Dot-Product Attention** | $\text{Attn}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$ | $\sqrt{d_k}$ prevents vanishing gradients in softmax |
| **LoRA Weight Update** | $W = W_0 + \frac{\alpha}{r}(B \cdot A)$ | $A \sim \mathcal{N}(0, \sigma^2)$, $B = 0$, $r \ll d$ |
| **KV-Cache Memory Size** | $\text{Size} = 2 \times b \times s \times l \times h \times d_{\text{head}} \times \text{bytes}$ | 2 factors for Key and Value tensors |
| **Temperature Scaling** | $P(y_i) = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$ | $T \to 0$: Greedy; $T > 1$: Higher randomness |
| **Nucleus (Top-p) Sampling** | $\sum_{i \in S^{(p)}} P(y_i) \ge p$ | Truncates tail probabilities dynamically |
| **DPO Loss** | $\mathcal{L}_{\text{DPO}} = -\mathbb{E}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w)}{\pi_{\text{ref}}(y_w)} - \beta \log \frac{\pi_\theta(y_l)}{\pi_{\text{ref}}(y_l)}\right)\right]$ | Closed-form preference optimization without Reward Model |

---

## 📊 High-Value Comparison Tables

### 1. Architecture Comparison
| Attribute | Encoder-Only (BERT) | Decoder-Only (GPT/Llama) | Encoder-Decoder (T5) |
| :--- | :--- | :--- | :--- |
| **Attention Type** | Full Bidirectional | Causal (Masked) | Bidirectional (Enc) + Cross (Dec) |
| **Objective** | Masked LM (MLM) | Next-Token Prediction | Sequence-to-Sequence Denoising |
| **Best For** | Embeddings, Classification | Text Generation, Code, Reasoning | Translation, Summarization |

---

### 2. PEFT Methods Comparison
| Method | Trainable Params | Storage Footprint | Latency Overhead | Performance |
| :--- | :--- | :--- | :--- | :--- |
| **Full Fine-Tuning** | 100% | 100% (Full Checkpoint) | None | Baseline (Highest) |
| **LoRA** | 0.01% – 0.1% | Megabytes (Adapter) | Zero (if merged) | Near-Full Fine-Tuning |
| **QLoRA** | 0.01% – 0.1% | Megabytes (4-bit Base) | Minor quantization delay | Matches LoRA quality |
| **Prefix Tuning** | 0.1% – 1% | Small | Reduces effective seq len | Sensitive to learning rate |

---

### 3. Sampling & Decoding Strategies
| Strategy | Mechanism | Pros | Cons | Best Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **Greedy Decoding** | Selects $\text{argmax}(P(y_i))$ | Fast, deterministic | Repetitive, robot-like | Math, Coding, Classification |
| **Beam Search** | Keeps top $K$ hypothesis paths | Finds optimal global sequence | High memory, repetition loops | Translation, Summarization |
| **Top-K Sampling** | Keeps top $K$ most likely tokens | Truncates low-probability noise | Static $K$ fails on flat distributions | Creative Writing |
| **Top-P (Nucleus)** | Keeps tokens cumulative prob $\le p$ | Adapts vocabulary dynamically | Can still sample rare noise | Chatbots, Conversational AI |

---

### 4. RAG vs. Fine-Tuning Decision Matrix
| Dimension | Retrieval-Augmented Generation (RAG) | Fine-Tuning (SFT / LoRA) |
| :--- | :--- | :--- |
| **Knowledge Currency** | Instant (Dynamic DB updates) | Static (Requires retraining) |
| **Hallucination Risk** | Low (Grounded in context) | Medium-High (Parametric memory) |
| **Domain Style Adaptation** | Poor | Excellent (Learns style/syntax) |
| **Latency Cost** | Adds vector search & context overhead | Faster generation (No added tokens) |
| **Source Citation** | Direct link to document chunks | Impossible to trace exact sources |

---

### 5. Generative Image Models: GAN vs VAE vs Diffusion
| Model | Training Stability | Output Quality | Diversity | Sampling Speed |
| :--- | :--- | :--- | :--- | :--- |
| **GAN** | Low (Mode collapse) | Extremely High | Low | Instant (1 pass) |
| **VAE** | High (ELBO optimization) | Blurry | High | Instant (1 pass) |
| **Diffusion** | High (MSE noise loss) | State-of-the-Art | High | Slow (Multi-step iterative) |

---

## 🎯 Common Pitfalls & Best Practices

- ❌ **Mistake**: Using standard LayerNorm in Llama models.
  - ✅ **Correction**: Llama uses **RMSNorm** (Root Mean Square Normalization), which omits mean-centering to save ~7% training computation.
- ❌ **Mistake**: Applying LoRA only to Attention matrices ($W_q, W_k, W_v$).
  - ✅ **Correction**: Applying LoRA to FFN layers ($W_{gate}, W_{up}, W_{down}$) alongside Attention doubles fine-tuning quality on complex tasks.
- ❌ **Mistake**: Forgetting `padding_side="left"` in batch autoregressive generation.
  - ✅ **Correction**: Autoregressive decoding requires **left padding** so generated tokens align naturally at sequence ends.

---

## 🧠 Memory Aids & Mnemonics

- **PALER**: **P**rompting, **A**rchitectures, **L**oRA/PEFT, **E**valuation, **R**AG.
- **Q-K-V Metaphor**:
  - **Query ($Q$)**: The search query typed into Google.
  - **Key ($K$)**: The indexing keywords of indexed web pages.
  - **Value ($V$)**: The actual content of the web page returned.

---

### 6. Agentic AI & Prompting Patterns

| Pattern | Full Name | Mechanism | When to Use |
| :--- | :--- | :--- | :--- |
| **Zero-Shot** | Direct Prompting | No examples; relies on model's parametric knowledge | Simple factual Q&A |
| **Few-Shot** | In-Context Learning | 2–5 input-output examples in the prompt | When output format is critical |
| **CoT** | Chain-of-Thought | "Think step by step" prompt elicits explicit reasoning trace | Math, logic, multi-step reasoning |
| **ToT** | Tree of Thoughts | Model explores multiple reasoning branches, evaluates, backtracks | Complex problem solving requiring search |
| **ReAct** | Reason + Act | Interleaves reasoning steps with tool calls and observation parsing | Agentic tool use, RAG agents |
| **Self-Consistency** | Majority Voting | Sample multiple CoT paths; take majority vote answer | Improves reliability on reasoning tasks |

### 7. Hallucination Mitigation Strategies

| Strategy | Mechanism | Effectiveness | Cost |
| :--- | :--- | :--- | :--- |
| **RAG** | Ground generation in retrieved context | High (factual accuracy) | Medium (vector search latency) |
| **RLHF / DPO Alignment** | Train model to prefer truthful responses | Medium-High | Very High (requires training) |
| **Constrained Decoding** | Force model to choose from valid options/schema only | High for structured outputs | Low |
| **Self-Consistency** | Sample multiple outputs; vote for most consistent | Medium | Medium (3–10x more inference) |
| **Llama Guard / NeMo** | Post-hoc safety/fact classifier on outputs | Medium | Low (separate inference pass) |
| **Tool Use (Grounding)** | Force model to compute facts via calculator/search API | Very High | Medium |
| **Factuality Training** | Fine-tune on attribution-labeled factual data | Medium | High |

### 8. Open-Source LLM Landscape (2025–2026)

| Model Family | Creator | Params | Architecture Highlights | Best For |
| :--- | :--- | :--- | :--- | :--- |
| **Llama-3.1 / 3.2** | Meta | 8B, 70B, 405B | GQA, RoPE, SwiGLU, RMSNorm, 128K context | General reasoning, open deployment |
| **Mistral / Mixtral** | Mistral AI | 7B / 8x7B MoE | Sliding Window Attention, Grouped QA, 32K ctx | Efficient inference, long context |
| **Gemma-2** | Google DeepMind | 2B, 9B, 27B | Interleaved local/global attention, logit softcapping | Edge deployment, multilingual |
| **Phi-3 / Phi-4** | Microsoft | 3.8B, 14B | Trained on curated high-quality data ("textbooks are all you need") | Small model excellence |
| **DeepSeek-R1** | DeepSeek AI | 7B–671B | MoE with GRPO RL alignment, reasoning-focused | Math, coding, STEM reasoning |
| **Qwen-2.5** | Alibaba | 7B, 72B | Long-context (128K), multimodal variants | Multilingual, code generation |
