# 🔝 Top 275 Generative AI & LLM Interview Questions

*Complete collection of top-repeated, difficult, tricky, must-know, frequently rejected, and differentiating interview questions curated from FAANG and AI labs.*

---

## 📌 Section 1: Top 25 Most Repeated Questions

| # | Question | Difficulty | Level | Target Companies |
|---|----------|------------|-------|-------------------|
| 1 | Explain the Transformer architecture and self-attention mechanism in detail. | Medium | SDE2 | Big Tech, AI Labs |
| 2 | What is the difference between GPT and BERT? | Easy | Fresher / SDE1 | All Tech |
| 3 | How does fine-tuning of LLMs work? What is LoRA (Low-Rank Adaptation)? | Medium | SDE2 | Big Tech, AI |
| 4 | What is RLHF (Reinforcement Learning from Human Feedback) and how does it align LLMs? | Hard | Senior | OpenAI, Anthropic, Meta |
| 5 | What is Retrieval-Augmented Generation (RAG) and how does it reduce hallucination? | Medium | SDE2 | AI, Unicorns |
| 6 | How do diffusion models generate images? Compare DDPM and DDIM. | Hard | Senior | Stability AI, Midjourney, Google |
| 7 | What are prompt engineering techniques? (zero-shot, few-shot, chain-of-thought) | Easy | SDE1 | All Tech |
| 8 | How do you evaluate generative models? (perplexity, BLEU, ROUGE, BERTScore, human eval) | Medium | SDE2 | Big Tech, AI |
| 9 | What is the attention mechanism? Derive scaled dot-product attention mathematically. | Hard | SDE2/Senior | AI Research, Big Tech |
| 10 | Explain the training objective of GPT (autoregressive causal language modeling). | Medium | SDE2 | AI Labs |
| 11 | What are word embeddings and how are they used in LLMs? | Easy | Fresher | All Tech |
| 12 | How does the Transformer handle variable-length input? (padding, causal masking) | Medium | SDE2 | AI |
| 13 | What is the concept of temperature in text generation? How does it alter softmax? | Easy | SDE1 | All Tech |
| 14 | What is hallucination in LLMs? What architectural strategies mitigate it? | Medium | SDE2 | Big Tech, AI |
| 15 | Compare GANs, VAEs, and Diffusion models for generative modeling. | Hard | Senior | AI Labs |
| 16 | How does tokenization work? Compare BPE, WordPiece, and SentencePiece. | Medium | SDE2 | AI Labs |
| 17 | What is the difference between pre-training and fine-tuning? | Easy | Fresher | All Tech |
| 18 | Explain context length limits and how modern LLMs extend sequence length. | Medium | SDE2 | AI Labs |
| 19 | How do you prevent a model from generating toxic or harmful content? (Guardrails) | Medium | SDE2 | Big Tech |
| 20 | What is QLoRA? Explain NF4, double quantization, and paged optimizers. | Hard | Senior | AI Labs, Big Tech |
| 21 | What are vector databases and how are they used in RAG? (FAISS, HNSW) | Medium | SDE2 | AI, Enterprise |
| 22 | How does knowledge distillation work in LLMs? | Hard | Senior | AI Labs |
| 23 | What is the role of the KV-cache in transformer inference acceleration? | Hard | Senior | Infrastructure, AI |
| 24 | Explain the difference between autoregressive and masked language models. | Medium | SDE2 | AI Labs |
| 25 | How would you deploy a large language model for low-latency inference? (vLLM, PagedAttention) | Hard | Senior / Staff | Big Tech, Infrastructure |

---

### Detailed Solutions for Top Repeated Questions

#### 1. Explain the Transformer architecture and self-attention.
- **Answer**: The Transformer relies on **Self-Attention** to compute dependencies between all tokens in parallel:
  $$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$
  where Query $Q = X W_Q$, Key $K = X W_K$, and Value $V = X W_V$. Multi-Head Attention splits representations into $h$ heads, allowing the model to attend to information from different representation subspaces simultaneously.
- **Follow-up**: *Why scale by $\sqrt{d_k}$?* To scale the variance of dot products back to $1$, preventing small gradients in the softmax function when $d_k$ is large.

#### 3. How does LoRA (Low-Rank Adaptation) work?
- **Answer**: LoRA freezes the pre-trained weight matrix $W_0 \in \mathbb{R}^{d \times k}$ and injects trainable rank decomposition matrices $A \in \mathbb{R}^{r \times k}$ and $B \in \mathbb{R}^{d \times r}$ with $r \ll \min(d, k)$:
  $$h = W_0 x + \Delta W x = W_0 x + \frac{\alpha}{r} (B A) x$$
  This reduces trainable parameters by up to 99% while maintaining fine-tuning quality.

#### 4. What is RLHF and how does it align LLMs?
- **Answer**: RLHF aligns LLMs with human intent through 3 steps:
  1. **Supervised Fine-Tuning (SFT)**: Train model on high-quality instruction data.
  2. **Reward Model Training**: Train a scalar reward model $r_\phi(x, y)$ on human preference pairs $(y_w \succ y_l | x)$.
  3. **RL Optimization (PPO)**: Optimize SFT policy $\pi_\theta$ using Proximal Policy Optimization (PPO) with a KL penalty against baseline policy $\pi_{\text{SFT}}$.

#### 23. What is the role of the KV-cache in transformer inference?
- **Answer**: During autoregressive generation, keys and values of previous tokens remain static. KV-cache stores previous $K$ and $V$ tensors in GPU RAM, avoiding $O(N^2)$ matrix recomputations and enabling single-token updates per step.
  $$\text{Memory}_{\text{KV}} = 2 \times b \times s \times l \times h \times d_{\text{head}} \times \text{bytes}$$

---

## 🏋️ Section 2: Top 50 Most Difficult Questions

1. Derive the gradient flow of the self-attention mechanism and explain how FlashAttention accelerates it via SRAM tiling.
2. Compare and contrast the pre-training objectives of BERT, GPT, T5, and XLNet.
3. How does DPO (Direct Preference Optimization) work, and why is it more stable than PPO for alignment? Derive the DPO objective from Bradley-Terry.
4. Explain the internals of the KV-cache and derive its memory consumption for a 70B model with context length 4096.
5. How does the "mixture of experts" (MoE) architecture work? Discuss load balancing loss, auxiliary loss, and expert capacity factor.
6. What is "rotary position embedding" (RoPE)? Derive its 2D rotation matrix formulation and explain its advantage over sinusoidal encodings.
7. How would you implement a retrieval-augmented generation pipeline that handles chunking, hybrid retrieval (BM25 + Dense), and hallucination verification?
8. Explain the "perceiver" architecture and its application in handling arbitrary modalities via cross-attention to a latent array.
9. How do you fine-tune a 70B parameter model on a single 80GB GPU? Discuss QLoRA, 4-bit NormalFloat, gradient checkpointing, and paged optimizers.
10. Compare speculative decoding, multi-query attention (MQA), grouped-query attention (GQA), and flash-decoding for LLM inference speedup.
11. Derive the denoising diffusion probabilistic model (DDPM) loss and explain the connection to score matching.
12. How does RLHF with PPO maintain a KL penalty to prevent reward hacking?
13. What are the challenges in evaluating open-ended generation, and how do methods like G-Eval or GPT-Score address them?
14. Describe the "Tree of Thoughts" prompt engineering technique and its search strategies (BFS vs DFS).
15. How does the "constitutional AI" method train safe models without human labels for harmfulness?
16. Explain the "ROUGE-L" metric and its formulation using longest common subsequence.
17. What is the "alignment tax" and how can it be minimized through techniques like PPO-ptx?
18. How would you implement a custom attention mask to enable cross-document attention in a RAG model?
19. Discuss the security risks of prompt injection and how to defend against them (input sanitization, privilege separation, guardrails).
20. What is the "NEFTune" technique and how does adding noise to embeddings improve fine-tuning?
21. Explain how "model soup" weight averaging improves generalization in LLMs.
22. How does the "LLaMA" series achieve state-of-the-art performance with architectural tweaks (SwiGLU, RoPE, RMSNorm)?
23. What is "GQA" (Grouped Query Attention) and how does it balance speed and quality compared to MHA and MQA?
24. Derive the contrastive loss used in CLIP and explain its cross-modal alignment across image and text encoders.
25. How would you build a multi-turn conversational agent with memory and tool-use capabilities using function calling?
26. Explain the "vision transformer" (ViT) and how it can be extended to video (TimeSformer, ViViT).
27. What is the "Prefix-Tuning" method and how does it differ from prompt tuning and LoRA?
28. How does Differential Privacy (DP) guarantee privacy in fine-tuning? Discuss DP-SGD.
29. What is "RLCD" (Reinforcement Learning with Contrastive Decoding) for safety?
30. Compare the "Flan-T5" and "OPT-IML" approaches for instruction tuning.
31. How does the "DALL-E" series use discrete VAE and transformer for image generation?
32. Explain the "denoising score matching" perspective of diffusion models.
33. What is the "classifier-free guidance" (CFG) technique in diffusion models?
34. How would you train a code generation model like Codex? Discuss fill-in-the-middle (FIM) objectives.
35. Describe the "StarCoder" architecture and data curation process.
36. How do you detect generated text? (watermarking, classifiers, statistical anomalies).
37. What is the "OpenCLIP" training pipeline and how to scale to billions of image-text pairs?
38. How does "Sliding Window Attention" (Mistral, Longformer) enable long document processing?
39. Explain the "ALiBi" position bias and its simplicity in scaling sequence length.
40. How does the `peft` library unify LoRA, prefix tuning, and adapters?
41. What is "RetNet" (Retentive Network) and its linear attention formulation?
42. Discuss the "Sparse Mixture of Experts" routing algorithm and expert capacity bottlenecks.
43. How to perform instruction-based image editing with a diffusion model (InstructPix2Pix)?
44. What is the "U-Net" backbone in diffusion and how does it incorporate time embeddings and cross-attention?
45. Explain "semantic entropy" to detect hallucinations in free-form generation.
46. How does "SAM" (Segment Anything Model) leverage a prompt encoder for zero-shot segmentation?
47. What is a distilled model like "DistilGPT-2" and how are soft targets used in distillation loss?
48. How to evaluate code generation models: pass@k and functional correctness.
49. What is the "Instruct-RLHF" pipeline that mixes instruction data with human preference datasets?
50. How does the "MosaicML" training stack achieve efficient LLM pre-training throughput?

---

## 🤹 Section 3: Top 50 Most Tricky Questions

1. Why can't you just use a larger context window without increasing computational cost quadratically? ($O(N^2)$ memory and matmul).
2. What is the difference between "temperature" and "top-p" sampling? When would you combine them?
3. If you train a model with masked language modeling, can it generate coherent text directly? Why not? (Lacks causal directional conditioning).
4. Why does a GPT-style model need causal masking during pre-training? (Prevents attending to future tokens).
5. What happens if you don't apply positional encoding to the transformer? (Input becomes set of tokens; permutation invariant).
6. Can you fine-tune a language model on unlabelled text? (Yes, self-supervised fine-tuning via CLM).
7. How do you incorporate external knowledge without fine-tuning? (RAG vs In-Context Prompting).
8. Why is beam search often worse than sampling for open-ended creative generation? (Tends towards repetitive, generic text).
9. If you double the number of attention heads, does the model necessarily improve? (Capacity vs head dimension $d_k$ split trade-off).
10. Why does "exposure bias" affect autoregressive models? How does scheduled sampling help?
11. When would you use a decoder-only model (GPT) vs encoder-decoder (T5)?
12. What is the role of the "stop token" (EOS) in generation? How is it learned?
13. Why does "gradient checkpointing" trade compute for memory? (Saves memory by recomputing activations during backward pass).
14. In LoRA, why does rank $r$ affect capacity? Is bigger always better? (Overfitting vs computational efficiency).
15. Why can't you simply average weights of two fine-tuned LLMs unless they share the same initialization?
16. What is the "Copy-attention" mechanism in text generation?
17. How do "continuous prompts" (soft prompts) differ from discrete text prompts?
18. Why might a diffusion model generate blurry images at low timesteps?
19. What is the difference between "latent diffusion" (Stable Diffusion) and pixel-space diffusion (Imagen)?
20. How does the "null prompt" in classifier-free guidance steer generation?
21. Why is the ELBO (Evidence Lower Bound) used in VAEs? What's the trade-off?
22. If you use a very high guidance scale in diffusion, what happens to image quality and diversity? (Over-saturated, low diversity).
23. Can you use a transformer trained on text to process images? (Yes, ViT patchifies images into visual tokens).
24. What is the problem with "exact match" metrics (BLEU/ROUGE) for open-ended generative tasks?
25. Why is perplexity on a held-out set not a perfect measure of generation quality?
26. How does the "repetition penalty" in generation work? How does it affect logits?
27. Why do LLMs sometimes output repetitive text even with high temperature?
28. How do you handle out-of-vocabulary words with subword tokenization?
29. What is the "attention sink" phenomenon, and how does StreamingLLM exploit it?
30. How does "knowledge distillation" from a large teacher to a smaller student differ from pruning?
31. Why is "router" training in MoE non-trivial? (Load balancing loss to prevent expert collapse).
32. Can you fine-tune a model on data with a very different distribution and still retain old knowledge? (Catastrophic forgetting).
33. How does "sequence packing" speed up training but affect loss computation?
34. What is the difference between "absolute positional encoding" and "relative positional bias"?
35. How does "token dropout" (masking) work in BERT and why?
36. Why does a diffusion model need many steps? What techniques reduce steps? (DDIM, Consistency Models).
37. What is the independence assumption in naive Bayes and how does autoregressive modeling avoid it?
38. How does "top-k sampling" truncate the token distribution?
39. Why might a prompt like "Let's think step by step" improve reasoning accuracy? (Triggers CoT intermediate computation steps).
40. Can you use the same tokenizer for multiple languages? What are the challenges?
41. What is the `sentencepiece` library's advantage over standard BPE? (Language-agnostic, handles whitespace directly).
42. How does weight tying between embedding layers and output projection save memory?
43. What is the scaling factor in FlashAttention and why is it divided by $\sqrt{d_k}$?
44. How do you prevent the "dead decoder" problem in VAEs?
45. What is the effect of "label smoothing" on cross-entropy loss in language models?
46. Why do we add a "beginning of sentence" (BOS) token?
47. How does "gradient accumulation" simulate larger batch sizes on small GPUs?
48. Why might a GAN discriminator overpower the generator? (Mode collapse).
49. What is "adaptive softmax" and when is it useful for large vocabularies?
50. How does "dynamic batching" (bucketing) improve training throughput for variable length sequences?

---

## 🌟 Section 4: Top 50 Must-Know Questions

1. Transformer block breakdown: Self-Attention, Multi-Head, LayerNorm, FFN, Residual Connections.
2. GPT-style autoregressive language modeling objective.
3. BERT-style masked language modeling (MLM) objective.
4. Subword tokenization: BPE, WordPiece, SentencePiece algorithms.
5. Training objectives: MLM, CLM, Sequence-to-Sequence denoising.
6. Positional encodings: Sinusoidal, Learned, RoPE, ALiBi.
7. Fine-tuning vs. Few-shot in-context learning vs. Zero-shot learning.
8. Prompt engineering strategies: Chain-of-Thought (CoT), Zero-Shot CoT, Few-Shot CoT.
9. Retrieval-Augmented Generation (RAG) complete architecture.
10. Vector databases and vector indices (FAISS, HNSW, IVF, Pinecone).
11. LoRA and QLoRA for parameter-efficient fine-tuning (PEFT).
12. RLHF alignment: Reward Model, PPO, DPO, KTO.
13. Hallucination: Root causes and mitigation strategies (RAG, Guardrails, Self-RAG).
14. Evaluation metrics: Perplexity, BLEU, ROUGE, METEOR, BERTScore, G-Eval, Human Eval.
15. Generation parameters: Temperature, Top-k, Top-p (Nucleus) sampling.
16. Decoding strategies: Greedy, Beam Search, Random Sampling, Speculative Decoding.
17. KV-Cache for fast autoregressive inference.
18. Attention variants: Multi-Head Attention (MHA), Multi-Query Attention (MQA), Grouped-Query Attention (GQA).
19. Diffusion models: Forward noise process, Reverse denoising process, U-Net, Classifier-Free Guidance.
20. GANs: Generator, Discriminator, Minimax objective, Mode collapse.
21. VAEs: Encoder, Decoder, Latent space regularization, Reparameterization trick.
22. Contrastive Learning (CLIP) for multimodal alignment.
23. Inference optimization: Quantization (INT8, INT4, AWQ, GPTQ), Pruning, Distillation.
24. FlashAttention and memory-efficient IO-aware attention.
25. Context length extension: RoPE scaling (YaRN, NTK-aware), ALiBi, Sliding Window.
26. Instruction tuning: Data curation (Self-Instruct, Alpaca), formatting templates.
27. Constitutional AI and automated harmlessness self-improvement.
28. Red-teaming and adversarial prompt injection testing.
29. Watermarking and AI-generated text detection (Kirchenbauer et al.).
30. Model serving engines: vLLM, TensorRT-LLM, TGI, Continuous Batching.
31. Pre-training data preparation: Deduplication, Quality filtering, Tokenization pipelines.
32. Scaling laws: Kaplan et al. vs Chinchilla compute-optimal training.
33. Mixture of Experts (MoE): Top-k routing, Gating networks, Load balancing.
34. Knowledge distillation in LLMs: Soft target cross-entropy.
35. Chain-of-Thought reasoning mechanics in transformers.
36. Tree-of-Thought and Graph-of-Thought advanced reasoning search algorithms.
37. Tool use and API function calling via formatted schemas.
38. Multimodal models: Image captioning, Visual Question Answering (LLaVA, Gemini).
39. Code generation models: Codex, StarCoder, Fill-in-the-Middle (FIM).
40. Open-weights LLM families: Llama 3, Mistral, Gemma, Falcon.
41. Data privacy in training: Differential Privacy (DP-SGD).
42. Bias, fairness, and safety alignment in generative models.
43. Training stability: Gradient clipping, Warmup schedules, LayerNorm placement (Pre-LN).
44. Model parallelism: Tensor Parallelism (Megatron-LM), Pipeline Parallelism, Data Parallelism (DDP/FSDP).
45. Embedding model fine-tuning for domain-specific RAG.
46. RAG Evaluation metrics: Ragas framework (Faithfulness, Answer Relevance, Context Recall).
47. Security vulnerabilities: Indirect prompt injection, System prompt extraction.
48. Synthetic data generation with LLMs (Evol-Instruct).
49. Multilingual LLMs and cross-lingual transfer learning.
50. Practical MLOps for LLMs: Experiment tracking (W&B), CI/CD for prompt testing.

---

## 🚫 Section 5: Top 50 Frequently Rejected Pitfalls

1. **"What is RLHF?"** – *Bad Answer*: "Training with human feedback." -> *Expected Answer*: Explain SFT baseline, Reward Model training on preference pairs $(y_w \succ y_l)$, and PPO policy optimization with a KL divergence penalty to prevent reward hacking.
2. **"How to reduce hallucination?"** – *Bad Answer*: "Use RAG." -> *Expected Answer*: Detail semantic chunking, hybrid retrieval (BM25 + dense), Cross-Encoder re-ranking, and output verification via Self-RAG.
3. **"Explain LoRA"** – *Bad Answer*: "It reduces parameters." -> *Expected Answer*: Formulate $W = W_0 + \frac{\alpha}{r} (B A)$, explain low-rank decomposition $r \ll d$, zero initialization of $B$, and parameter savings.
4. **"What is attention?"** – *Bad Answer*: "It looks at words." -> *Expected Answer*: Derive $\text{softmax}(Q K^T / \sqrt{d_k}) V$, explaining Queries, Keys, Values, and scaling factors.
5. **"What is a diffusion model?"** – *Bad Answer*: "Generates images from noise." -> *Expected Answer*: Explain forward Gaussian noise process $q(x_t|x_0)$, reverse denoising U-Net $\epsilon_\theta(x_t, t)$, MSE loss, and Classifier-Free Guidance (CFG).
6. **"How to evaluate an LLM?"** – *Bad Answer*: "Use accuracy." -> *Expected Answer*: Discuss Perplexity on test corpus, ROUGE/BLEU for overlap, BERTScore for semantic similarity, pass@k for code, and LLM-as-a-Judge (G-Eval).
7. **"Why use temperature?"** – *Bad Answer*: "To make it creative." -> *Expected Answer*: Explain dividing logits $z_i / T$ before softmax to adjust probability distribution entropy.
8. **"What is a tokenizer?"** – *Bad Answer*: "Splits words." -> *Expected Answer*: Detail subword algorithms (BPE, WordPiece, SentencePiece), vocabulary management, and out-of-vocabulary handling.
9. **"What is a vector DB?"** – *Bad Answer*: "Stores embeddings." -> *Expected Answer*: Explain high-dimensional index structures (HNSW, IVF), Approximate Nearest Neighbor (ANN) search, and distance metrics (Cosine, L2, Inner Product).
10. **"Why does GPT need a causal mask?"** – *Bad Answer*: "So it doesn't cheat." -> *Expected Answer*: Explain lower-triangular masking in attention matrix preventing token $t$ from attending to future tokens $t+1 \dots N$ during autoregressive training.
11. **"Difference between GPT and BERT?"** – *Bad Answer*: "GPT generates, BERT understands." -> *Expected Answer*: Contrast Decoder-only causal attention vs Encoder-only bidirectional attention and pre-training tasks (CLM vs MLM).
12. **"How to fine-tune a 13B model?"** – *Bad Answer*: "Just load and train." -> *Expected Answer*: Detail VRAM calculation for FP16 training vs PEFT/QLoRA with 4-bit quantization, gradient checkpointing, and optimizer offloading.
13. **"What is KV-cache?"** – *Bad Answer*: "Caches keys and values." -> *Expected Answer*: Explain storing previous token $K$ and $V$ activations in GPU RAM to eliminate $O(N^2)$ recomputation per autoregressive generation step.
14. **"How does beam search work?"** – *Bad Answer*: "Picks best beams." -> *Expected Answer*: Explain keeping top-$K$ candidate sequence hypotheses at each token step based on log probability summation with length normalization.
15. **"What is top-p sampling?"** – *Bad Answer*: "Picks top p." -> *Expected Answer*: Explain dynamic truncation of probability distribution to smallest set of tokens whose cumulative sum exceeds $p$.
16. **"Why is perplexity used?"** – *Bad Answer*: "It's a metric." -> *Expected Answer*: Derive $\text{PPL} = \exp(-\frac{1}{N} \sum \log P(x_i))$, representing the exponentiated cross-entropy loss.
17. **"Role of positional encoding?"** – *Bad Answer*: "Gives order." -> *Expected Answer*: Explain that self-attention is permutation-invariant without explicit positional representations added or applied via rotation (RoPE).
18. **"How to handle long documents?"** – *Bad Answer*: "Truncate." -> *Expected Answer*: Discuss chunking with overlap, sliding window attention, RoPE scaling (YaRN), or retrieval augmentation.
19. **"What is reparameterization trick?"** – *Bad Answer*: "To sample." -> *Expected Answer*: Formulate $z = \mu + \sigma \odot \epsilon$ where $\epsilon \sim \mathcal{N}(0, I)$, making stochastic latent sampling differentiable for backpropagation in VAEs.
20. **"What is GAN?"** – *Bad Answer*: "Generator and discriminator." -> *Expected Answer*: Formulate minimax game $\min_G \max_D V(D, G) = \mathbb{E}[\log D(x)] + \mathbb{E}[\log(1 - D(G(z)))]$.
21. **"Why layer norm instead of batch norm?"** – *Bad Answer*: "To normalize." -> *Expected Answer*: Batch size in NLP varies and sequence lengths differ; LayerNorm normalizes across feature dimensions per token independently of batch size.
22. **"How to detect generated text?"** – *Bad Answer*: "Use a classifier." -> *Expected Answer*: Discuss watermarking schemes (green/red token rules), statistical perplexity anomalies, and classifier limitations on fine-tuned models.
23. **"What is a reward model?"** – *Bad Answer*: "Gives reward." -> *Expected Answer*: Scalar network trained with binary cross-entropy on pairwise preferences $\mathcal{L} = -\mathbb{E}[\log \sigma(r_\phi(x, y_w) - r_\phi(x, y_l))]$.
24. **"LoRA vs QLoRA?"** – *Bad Answer*: "QLoRA is quantized." -> *Expected Answer*: Detail QLoRA innovations: 4-bit NormalFloat (NF4), Double Quantization (DQ), and Paged Optimizers.
25. **"How do you serve an LLM?"** – *Bad Answer*: "Put it in an API." -> *Expected Answer*: Detail serving engine optimizations: vLLM PagedAttention, Continuous Batching, Tensor Parallelism, and Speculative Decoding.
26. **"Purpose of softmax?"** – *Bad Answer*: "Converts to probabilities." -> *Expected Answer*: Exponentiates logits to ensure positive values summing to 1; combined with cross-entropy for non-vanishing gradient flow.
27. **"Why BOS token?"** – *Bad Answer*: "Start of sentence." -> *Expected Answer*: Provides an initial conditioning context vector for generating the very first token in autoregressive decoding.
28. **"Zero-shot vs Few-shot?"** – *Bad Answer*: "Number of examples." -> *Expected Answer*: Zero-shot relies purely on pre-trained parametric knowledge; Few-shot provides $k$ input-output demonstration pairs in context without weight updates.
29. **"How to improve output diversity?"** – *Bad Answer*: "Increase temperature." -> *Expected Answer*: Increase temperature combined with top-$p$ nucleus sampling and repetition/frequency penalties to penalize repeated logits.
30. **"What is stop token?"** – *Bad Answer*: "It stops." -> *Expected Answer*: Special token (EOS) emitted by model to signal end of completion sequence, breaking the generation loop.
31. **"Why larger model doesn't always win?"** – *Bad Answer*: "Overfitting." -> *Expected Answer*: Compute-optimal data scaling (Chinchilla law) shows under-trained large models perform worse than smaller models trained on significantly more high-quality tokens.
32. **"What is decoder-only?"** – *Bad Answer*: "No encoder." -> *Expected Answer*: Model utilizing causal attention masks where every token attends only to prior positions, optimized for autoregressive text generation.
33. **"How does RAG work?"** – *Bad Answer*: "Retrieve then generate." -> *Expected Answer*: Detail chunking, dense embedding generation, vector ANN search, re-ranking, prompt context assembly, and grounded generation.
34. **"What is Chinchilla law?"** – *Bad Answer*: "More data is better." -> *Expected Answer*: Demonstrates that for compute-optimal training, model size $N$ and training dataset size $D$ should scale in equal proportion ($D \approx 20N$).
35. **"What is FlashAttention?"** – *Bad Answer*: "It's fast." -> *Expected Answer*: IO-aware exact attention algorithm that tiles $Q, K, V$ into GPU SRAM blocks, eliminating $O(N^2)$ reads/writes to HBM.
36. **"Explain prompt injection"** – *Bad Answer*: "Hacking prompt." -> *Expected Answer*: Malicious input overriding developer system prompt instructions; mitigated via dual-LLM privilege separation, input sanitization, and output guardrails.
37. **"What is a soft prompt?"** – *Bad Answer*: "Trainable embeddings." -> *Expected Answer*: Prepended virtual embedding vectors optimized via gradient descent while keeping LLM weights frozen (Prompt Tuning).
38. **"Prevent catastrophic forgetting?"** – *Bad Answer*: "Small learning rate." -> *Expected Answer*: Apply PEFT (LoRA/adapters), mix general pre-training data into fine-tuning dataset, or use Elastic Weight Consolidation (EWC).
39. **"GELU vs ReLU?"** – *Bad Answer*: "GELU is smoother." -> *Expected Answer*: $\text{GELU}(x) = x \Phi(x) = x P(X \le x)$, providing smooth probabilistic gating without zero-gradient dead zones for negative inputs.
40. **"Purpose of pad token?"** – *Bad Answer*: "To pad." -> *Expected Answer*: Equalizes sequence lengths in a batch, paired with attention masks so padded positions are ignored during attention weight computation.
41. **"Why attention mask important?"** – *Bad Answer*: "Ignores padding." -> *Expected Answer*: Sets attention scores at padded or future positions to $-\infty$ prior to softmax, ensuring zero attention probability weight.
42. **"What is an embedding?"** – *Bad Answer*: "A vector." -> *Expected Answer*: Dense vector mapping discrete tokens into a continuous semantic vector space where geometric proximity reflects semantic similarity.
43. **"How does knowledge distillation work?"** – *Bad Answer*: "Student learns from teacher." -> *Expected Answer*: Student model minimizes KL divergence between its output logit distribution and teacher model soft probabilities computed at temperature $T$.
44. **"What is a latent variable in VAE?"** – *Bad Answer*: "Hidden variable." -> *Expected Answer*: Continuous random variable $z \sim q_\phi(z|x)$ capturing bottleneck generative factors of data in regularized Gaussian latent space.
45. **"Purpose of generator in GAN?"** – *Bad Answer*: "To generate." -> *Expected Answer*: Learns mapping $G: Z \to X$ from noise distribution to data distribution, attempting to minimize discriminator accuracy in minimax game.
46. **"How does diffusion denoise?"** – *Bad Answer*: "Removes noise." -> *Expected Answer*: U-Net predicts noise tensor $\epsilon_\theta(x_t, t)$ present at timestep $t$, subtracting predicted noise to compute step $x_{t-1}$.
47. **"Why scale attention output?"** – *Bad Answer*: "Avoid large values." -> *Expected Answer*: Scaling by $1/\sqrt{d_k}$ maintains variance at $1$, preventing dot product magnitude growth from driving softmax into zero-gradient saturation.
48. **"What is causal LM?"** – *Bad Answer*: "Predicts next token." -> *Expected Answer*: Autoregressive language model conditioned strictly on preceding context tokens $P(x) = \prod_{i=1}^N P(x_i | x_1 \dots x_{i-1})$.
49. **"How to handle OOV words?"** – *Bad Answer*: "Use UNK." -> *Expected Answer*: Subword tokenization (BPE/SentencePiece) breaks unknown words into subword pieces or raw bytes, eliminating out-of-vocabulary tokens.
50. **"Pre-norm vs Post-norm?"** – *Bad Answer*: "Location of LayerNorm." -> *Expected Answer*: Pre-LN ($\text{LN}(x) \to \text{SubLayer} \to \text{Add}$) stabilizes gradient flow allowing deep transformer training without warmups; Post-LN requires careful LR warmup schedules.

---

## 🏆 Section 6: Top 50 Differentiating Questions

1. Compare architectural decisions behind Llama 3, Mistral-7B, and Gemma. How do SwiGLU, GQA, RMSNorm, and sliding window attention impact memory and FLOP throughput?
2. Derive the training loss of a diffusion model from variational inference and explain the role of the noise schedule $\beta_t$.
3. How would you implement retrieval-augmented generation with a cross-encoder reranker and hybrid search (BM25 + Dense vector HNSW)?
4. What are the failure modes of RLHF and how does DPO address them? Derive the DPO loss step-by-step from the Bradley-Terry preference model.
5. How does continuous batching (vLLM) achieve higher throughput than static batching? Explain PagedAttention virtual block tables.
6. Design a system for fine-tuning a language model on private medical data with Differential Privacy (DP-SGD) guarantees.
7. How would you evaluate an LLM's factuality and detection of its own hallucinations? Discuss self-consistency and semantic entropy.
8. Compare Speculative Decoding, Medusa (multi-head draft), and Lookahead decoding for LLM inference acceleration.
9. Explain how to fine-tune a model to follow instructions when you have only a few hundred seed examples (Self-Instruct, Evol-Instruct).
10. Discuss training stability issues in MoE models and how to mitigate them (auxiliary router loss, $Z$-loss, expert capacity factor).
11. How would you create a multi-modal assistant that can process images, audio, and text in a unified conversation?
12. What is the "reversal curse" in LLMs and why does autoregressive pre-training suffer from it?
13. Explain the concept of "watermarking" in LLM text generation and a robust scheme (Kirchenbauer green/red list token bias).
14. How do you implement "Chain-of-Verification" (CoVe) to systematically check and edit hallucinated statements?
15. Design a retrieval system that can update its vector index in real-time with sub-second ingestion latency.
16. Compare Prefix-Tuning, P-Tuning v2, and LoRA in terms of parameter insertion locations and gradient backpropagation paths.
17. How does the "Unlimiformer" architecture handle unlimited context lengths via off-loaded retrieval-augmented attention?
18. What is the "scaling law for reward model over-optimization" and its implications for iterative RLHF alignment?
19. How would you integrate an LLM with an external execution sandbox (e.g., Python REPL) for self-correcting code generation?
20. Explain the "Tree of Thoughts" (ToT) framework and how it integrates BFS/DFS search algorithms with LLM evaluation prompts.
21. Discuss the trade-offs between Encoder-Decoder (T5) and Decoder-Only (GPT) architectures for multi-task unified models.
22. How does Grouped-Query Attention (GQA) reduce KV-cache memory requirements? Show the mathematical breakdown for 64 query heads and 8 KV heads.
23. Explain "ALiBi" (Attention with Linear Biases) position encoding and why it enables zero-shot sequence length extrapolation.
24. How would you build a tool-use model that can parse formatted OpenAPI JSON schemas and execute function calls?
25. Design a pipeline to detect and mitigate demographic bias in generative models producing hiring recommendations.
26. Compare "Contrastive Decoding" and "DoLa" (Decoding by Contrasting Layers) for improving factual accuracy during sampling.
27. How does "QLoRA" achieve full-precision fine-tuning performance with 4-bit NormalFloat (NF4), Double Quantization, and Paged Optimizers?
28. What are the security vulnerabilities of deploying an LLM agent with web browsing capabilities, and how do you sandbox execution?
29. Explain the relationship between Word2Vec skip-gram objectives and modern transformer embedding layers.
30. How does Vector-Quantized VAE (VQ-VAE-2) discretize continuous latent spaces using codebook quantization?
31. Design a production evaluation suite for a customer support RAG system measuring Faithfulness, Context Precision, and Context Recall.
32. What is Perceiver IO and how does it read and produce arbitrary structured outputs using latent cross-attention arrays?
33. Explain the MoE top-$k$ gating function with noisy top-$k$ softmax routing and load balancing auxiliary loss terms.
34. How do you implement Teacher Forcing during autoregressive Transformer training and why does it accelerate convergence?
35. Describe the Block-Recurrent Transformer architecture that introduces recurrent state memories into transformer blocks.
36. How would you curate and compress a 1 Trillion token pre-training corpus? (MinHash deduplication, quality classifier filtering, domain blending).
37. What is the "NEFTune" method and why does adding uniform noise to embedding vectors during SFT improve instruction-following?
38. How does DPO implicitly learn an optimal reward function without requiring a separate neural network reward model?
39. Explain how Latent Consistency Models (LCM) reduce diffusion sampling to 1-4 steps via consistency distillation.
40. How would you build a natural language Text-to-SQL system over enterprise databases with schema linking and execution feedback?
41. Discuss the FastSpeech 2 non-autoregressive architecture for text-to-speech generation.
42. How does OpenAI's Whisper model utilize an Encoder-Decoder transformer for multilingual speech recognition and translation?
43. What is retrieval-augmented diffusion for text-to-image synthesis (e.g., Retrieval-Augmented Diffusion Models)?
44. Explain non-autoregressive parallel decoding (Mask-Predict) in BERT-style generation.
45. How would you fine-tune a multilingual model to prevent catastrophic forgetting of low-resource languages?
46. What is the "Fill-in-the-Middle" (FIM) training objective in StarCoder/CodeLlama and how does it enable inline code completion?
47. How does Mistral-7B outperform Llama-1 13B despite having fewer parameters? Analyze its architectural choices (GQA, SWA, Byte-fallback BPE).
48. What is Constitutional AI (CAI) and how does it automate Critique and Revision loops to eliminate human harmfulness labeling?
49. How would you use Reinforcement Learning from Execution Feedback (RLEF) to optimize code generation models using compiler test suite returns?
50. Explain Noisy Student self-training for semi-supervised generative modeling.
