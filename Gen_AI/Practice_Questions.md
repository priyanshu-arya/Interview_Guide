# 🧪 Complete Generative AI & LLM Practice Question Bank (675+ Questions)

An exhaustive, categorized collection of practice problems across Theory, Coding, MCQs, Scenarios, Production, Debugging, Output Predictions, Best Practices, Optimization, and Advanced research.

---

## 📚 Section 1: Theory Questions (150+ Questions)

### Transformer Architecture & Attention (30 Questions)
1. What is the self-attention formula? Explain $Q, K, V$.
2. Why do we scale the dot product by $\sqrt{d_k}$?
3. What is multi-head attention? How does it enhance the model?
4. Explain the role of the feed-forward network (FFN) after attention.
5. Why is layer normalization used instead of batch normalization in transformers?
6. Pre-LN vs Post-LN: what are the differences in training stability?
7. What is a residual connection? Why is it crucial for deep transformers?
8. Explain the need for positional encoding. Compare sinusoidal and learned.
9. What is rotary position embedding (RoPE)? How does it encode relative position?
10. What is ALiBi? How does it bias attention scores?
11. Describe the architecture of a GPT decoder block.
12. Describe the architecture of a BERT encoder block.
13. How does the encoder-decoder (T5) transformer work?
14. What is causal masking and why is it necessary for autoregressive models?
15. Explain how padding masks work to ignore padding tokens.
16. What is the difference between encoder self-attention and decoder cross-attention?
17. How does relative position bias (like T5's relative attention) work?
18. What are adaptive attention spans?
19. What is the receptive field of a transformer?
20. Explain the concept of attention head pruning and its effect.
21. How does talking-heads attention differ from standard multi-head?
22. What is sinkhorn sort-based attention?
23. Why do transformers not suffer from vanishing gradients despite many layers?
24. How does the computational complexity of attention scale with sequence length? ($O(N^2)$).
25. What are approximations to reduce attention complexity? (Linformer, Performer, Reformer).
26. What is sparse attention and how does it reduce compute?
27. How does the FlashAttention algorithm optimize memory?
28. Explain memory-efficient attention via SRAM tiling.
29. What is the purpose of dropout in attention weights?
30. How does Switch Transformer introduce MoE into the FFN layer?

### Large Language Models & Training (30 Questions)
31. What is the typical pre-training task for GPT? (Autoregressive language modeling).
32. Explain masked language modeling (MLM) and its masking probability.
33. What is whole word masking vs subword masking?
34. How does the T5 model unify tasks with text-to-text format?
35. What is the ELECTRA pre-training objective? (Replaced token detection).
36. Explain causal language modeling vs prefix language modeling.
37. How does XLNet use permutation language modeling?
38. What is the difference between pre-training and fine-tuning?
39. Describe the instruction tuning process.
40. What is RLHF? Walk through the three stages (SFT, Reward Model, PPO).
41. What is DPO and how does it optimize the same objective without a reward model?
42. How does constitutional AI differ from RLHF?
43. Explain scaling laws (Kaplan et al. and Chinchilla).
44. What is the Chinchilla-optimal compute allocation? ($D \approx 20N$).
45. How does data deduplication impact LLM training?
46. What is curriculum learning in the context of LLMs?
47. Explain the role of the learning rate scheduler with warmup and cosine decay.
48. What is gradient checkpointing? Memory/compute trade-off.
49. How does mixed precision training (FP16/BF16) work?
50. What is model parallelism vs data parallelism? When to use each.
51. Describe ZeRO optimizer stages (1, 2, 3) for memory efficiency.
52. How does pipeline parallelism split layers across GPUs?
53. What is the function of an embedding layer? Why is weight tying used?
54. How does the softmax bottleneck affect large vocabularies?
55. What is cross-entropy loss for language modeling?
56. Explain label smoothing in the context of LM training.
57. How does tokenizer choice (BPE vs Unigram) affect LLM performance?
58. What is sentencepiece and its advantages?
59. How do you add special tokens (BOS, EOS, PAD) to the tokenizer?
60. What are chat templates and why are they important for conversational models?

### PEFT, Prompting & RAG (30 Questions)
61. What is full fine-tuning vs parameter-efficient fine-tuning (PEFT)?
62. How does LoRA work? Define the low-rank update matrices $W = W_0 + \frac{\alpha}{r} BA$.
63. What is the typical rank $r$, and how do you choose it?
64. Where can you apply LoRA? (Attention matrices, FFN layers).
65. What is QLoRA? Explain NormalFloat4, double quantization, and paged optimizers.
66. Compare Adapter layers to LoRA.
67. What is prefix tuning? How does it prepend learnable vectors?
68. What is prompt tuning? How does it differ from prefix tuning?
69. Explain $(IA)^3$ (Infused Adapter by Inhibiting and Amplifying).
70. How does P-Tuning v2 apply continuous prompts at every layer?
71. Can you combine multiple PEFT methods? (LoRA + prefix).
72. How does LoraHub combine LoRA modules?
73. What is catastrophic forgetting in fine-tuning? How to mitigate?
74. How does BitFit fine-tuning work?
75. What is the role of scaling factor alpha in LoRA?
76. What is few-shot prompting? Provide an example.
77. What is zero-shot prompting?
78. Explain Chain-of-Thought (CoT) prompting.
79. What is self-consistency in CoT?
80. What is the Tree of Thoughts (ToT) approach?
81. How does automatic prompt engineering (APE) work?
82. What is the scratchpad technique for reasoning?
83. Explain instruction-following and how models are tuned for it.
84. What is the in-context learning ability of LLMs?
85. How does ReAct (reasoning + acting) integrate tool use?
86. Explain high-level RAG architecture.
87. What are document chunking strategies? (fixed, semantic, recursive).
88. What is the role of a vector database? (FAISS, Chroma, Pinecone).
89. What is hybrid search (sparse + dense)?
90. How do you incorporate a reranker to improve retrieval quality?

### Diffusion, GANs, Metrics & Deployment (60 Questions)
91. What is a denoising diffusion probabilistic model (DDPM)?
92. Define the forward noise process $q(x_t|x_0)$.
93. Define the reverse denoising process.
94. What loss function is used in DDPM? ($\text{MSE}(\epsilon, \epsilon_\theta(x_t, t))$).
95. What is the role of the noise schedule $\beta_t$?
96. How does DDIM speed up sampling?
97. What is classifier-free guidance (CFG)? How does it work?
98. What is latent diffusion? How does Stable Diffusion work?
99. What is the U-Net architecture in diffusion? Why skip connections?
100. How are text prompts incorporated into diffusion models via cross-attention?
101. What is the CFG scale parameter effect on output?
102. What is denoising score matching?
103. What is a Generative Adversarial Network (GAN)? Minimax objective.
104. What is mode collapse and how to mitigate it?
105. What is Wasserstein GAN? How does it improve training?
106. What is a Variational Autoencoder (VAE)? ELBO loss.
107. Explain the reparameterization trick $z = \mu + \sigma \odot \epsilon$.
108. What is a VQ-VAE? How does it use a discrete codebook?
109. What is perplexity? Derive from cross-entropy.
110. BLEU score precision and brevity penalty.
111. ROUGE-L longest common subsequence.
112. METEOR alignment and recall.
113. BERTScore contextual embedding similarity.
114. What is human evaluation and Chatbot Arena Elo rating?
115. How do you measure faithfulness of a generated summary?
116. How to evaluate code generation pass@k?
117. What is red-teaming? How to conduct it?
118. What is constitutional AI and harmlessness self-alignment?
119. How to detect AI-generated text? (Watermarking, classifiers).
120. How does differential privacy protect user data in fine-tuning?
121. What is prompt injection? Defenses (sanitization, guardrails).
122. What is the KV-cache and why is it critical for efficient decoding?
123. How does continuous batching increase serving throughput?
124. What is quantization (GPTQ, AWQ, bitsandbytes) and memory savings?
125. Explain speculative decoding.
126. What is flash decoding? (Long context parallel attention decoding).
127. How to serve an LLM with low latency? (vLLM, TensorRT-LLM).
128. Difference between weight-only quantization and activation quantization.
129. How does model pruning work for LLMs? (SparseGPT, Wanda).
130. What is knowledge distillation applied to LLMs?
131. Challenges of deploying Mixture of Experts (MoE) models.
132. How to implement attention sinks for streaming inference (StreamingLLM)?
133. What is the role of continuous batching in vLLM?
134. How does PagedAttention eliminate memory fragmentation?
135. Explain AWQ channel preservation.
136. What is Medusa multi-head decoding acceleration?
137. How does DeepSpeed ZeRO-Offload use CPU RAM?
138. What is sequence packing in LLM training data collators?
139. How to use mixed precision FP16 vs BF16 on NVIDIA Tensor Cores.
140. How does RoPE scaling (YaRN) extend sequence context?
141. What is Grouped-Query Attention (GQA) KV-cache reduction?
142. How does RMSNorm save computation over LayerNorm?
143. What is SwiGLU activation function?
144. How does 4-bit NormalFloat (NF4) quantization work?
145. What is double quantization in QLoRA?
146. How does NCCL optimize multi-GPU communication?
147. What is pipeline parallelism micro-batching?
148. How does activation recomputation trade compute for VRAM?
149. What is kernel fusion in CUDA?
150. How does DPO eliminate explicit reward model training?

---

## 💻 Section 2: Coding Questions (100 Problems)

### Easy Coding (20 Problems)
1. Implement scaled dot-product attention in PyTorch.
2. Write a whitespace and punctuation tokenizer.
3. Compute cosine similarity between two 1D embedding vectors.
4. Implement temperature scaling for raw logits.
5. Write a function to generate a causal upper-triangular mask for sequence length $N$.
6. Convert token indices to one-hot vectors.
7. Implement embedding lookup from a weight matrix.
8. Calculate cross-entropy loss given logits and target indices.
9. Write top-k sampling function in PyTorch.
10. Implement greedy decoding ($\text{argmax}$).
11. Generate sinusoidal positional encodings matrix.
12. Implement one BPE merge step.
13. Write a sequence padding function.
14. Create a PyTorch Dataset for text tokenization.
15. Write a numerically stable softmax function.
16. Compute perplexity given cross-entropy loss values.
17. Add positional encodings to input embeddings.
18. Write basic beam search with beam width $k$.
19. Build a data collator that pads and stacks batches.
20. Implement single Transformer encoder layer forward pass.

### Medium Coding (50 Problems)
21. Implement Multi-Head Attention from scratch in PyTorch.
22. Build a Transformer Decoder block with causal mask.
23. Implement a complete LoRA Linear layer module with weight merging.
24. Write a training loop for next-token autoregressive prediction.
25. Implement forward noise addition step for DDPM.
26. Build a DDPM beta noise scheduler.
27. Code reverse diffusion denoising step.
28. Write a complete RAG pipeline (chunk, embed, FAISS search, generate).
29. Train a BPE tokenizer on text data.
30. Write a function for Chain-of-Thought prompting.
31. Write ROUGE-L computation using longest common subsequence.
32. Implement contrastive CLIP-style loss.
33. Build a mini VAE with reparameterization trick.
34. Code gradient accumulation training loop.
35. Implement AdamW optimizer update step from scratch.
36. Write reward model evaluation function.
37. Implement simplified PPO policy update step.
38. Build beam search with length penalty normalization.
39. Create nucleus (top-p) sampling generator.
40. Implement KV-cache state management in PyTorch.
41. Fine-tune Hugging Face model with PEFT LoRA.
42. Implement overlapping document chunker for RAG.
43. Build semantic search using Sentence-Transformers.
44. Code continuous batching request queue simulator.
45. Implement gradient checkpointing via `torch.utils.checkpoint`.
46. Write dynamic batching sequence bucketing dataloader.
47. Implement Masked Language Modeling loss (BERT).
48. Create instruction-tuning dataset trainer (Alpaca format).
49. Write simplified FID score calculation.
50. Implement CLIP zero-shot classification pipeline.
51. Build DPO custom loss function module.
52. Write scaled dot-product attention using `torch.nn.functional.scaled_dot_product_attention`.
53. Implement regex prompt injection detector.
54. Code function to merge multiple LoRA weights into base model.
55. Export Transformer model to ONNX with dynamic axes.
56. Build speculative decoding token verification engine.
57. Implement permute-and-mask XLNet training objective.
58. Apply dropout to attention probability matrix.
59. Create PyTorch DistributedDataParallel (DDP) training script.
60. Implement Stable Diffusion pipeline text-to-image inference call.
61. Write BERTScore precision/recall calculator.
62. Write Fill-in-the-Middle (FIM) training pair generator.
63. Implement Monte Carlo Tree Search (MCTS) for text generation.
64. Code HellaSwag multiple-choice evaluation loop.
65. Write Kirchenbauer text watermarking logit modification function.
66. Implement numpy vector database (kNN exact search).
67. Build Q&A system using LangChain vector index.
68. Write contrastive decoding (DoLa) logit contrast function.
69. Implement self-consistency majority voting decoder.
70. Code function calling tool parser for calculator API.

### Hard & Very Hard Coding (30 Problems)
71. Implement FlashAttention tiled matrix multiplication in PyTorch / CUDA pseudocode.
72. Build MoE Transformer layer with top-$k$ gating and load balancing loss.
73. Write full RLHF PPO training loop with value head and actor policy.
74. Implement 2D U-Net diffusion training loop.
75. Code Transformer decoder with block-recurrent memory state.
76. Build QLoRA fine-tuning script with `bitsandbytes` 4-bit NormalFloat.
77. Implement Speculative Decoding client-server pipeline.
78. Write customized attention layer with ALiBi bias.
79. Create DPO preference pair trainer.
80. Build vLLM-style PagedAttention virtual block memory manager.
81. Implement mini autograd framework with Transformer support.
82. Code MoE routing with expert capacity limit drop tokens.
83. Write hybrid RAG retrieval pipeline with BM25 + FAISS + Cross-Encoder Reranker.
84. Implement Tree of Thoughts BFS puzzle solver.
85. Build Text-to-SQL execution engine with schema linking.
86. Code tensor-parallel Megatron-style matrix multiplication.
87. Implement Flash-Decoding long context attention parallelizer.
88. Write custom Unigram subword tokenizer.
89. Build full RAG agent with conversational memory and citations.
90. Implement DPM-Solver++ fast ODE diffusion sampler.
91. Write custom CUDA kernel for fused attention (pseudocode).
92. Build ZeRO-3 parameter sharded distributed trainer.
93. Implement full RLHF pipeline without external libraries.
94. Implement full Stable Diffusion text-to-image pipeline from raw weights.
95. Create speculative decoding tree attention candidate generator.
96. Train BERT from scratch on custom corpus.
97. Implement multi-task instruction tuning dataset orchestrator.
98. Write synthetic data generation and filtering pipeline using LLMs.
99. Build recursive book summarizer via hierarchical chunking.
100. Code moderation safety classifier integrated into LLM generation loop.

---

## ❓ Section 3: 100 Multiple-Choice Questions (MCQs)

1. Which model is decoder-only?  
   a) BERT b) GPT c) T5 d) BART -> **Answer:** b
2. Self-attention computes:  
   a) Weighted sum of values based on query-key similarity b) Dot product of query and output -> **Answer:** a
3. In scaled dot-product attention, $\sqrt{d_k}$ is used to:  
   a) Increase variance b) Avoid vanishing gradients in softmax -> **Answer:** b
4. True about BERT:  
   a) Autoregressive b) Uses masked language modeling -> **Answer:** b
5. Main advantage of LoRA:  
   a) Drastically reduces trainable parameters b) Increases capacity -> **Answer:** a
6. "Temperature" in LLMs is:  
   a) Logit scaling factor before softmax b) Learning rate -> **Answer:** a
7. Loss function for autoregressive LM:  
   a) MSE b) Cross-entropy -> **Answer:** b
8. CausalMask does:  
   a) Masks future tokens b) Masks padding tokens -> **Answer:** a
9. Typical subword tokenizer:  
   a) ResNet b) BPE -> **Answer:** b
10. Purpose of positional encoding:  
    a) Provide token order information b) Add noise -> **Answer:** a
11. Sampling method choosing cumulative probability $\le p$:  
    a) Top-k b) Top-p (nucleus) -> **Answer:** b
12. Perplexity measures:  
    a) Accuracy b) Exponentiated average negative log-likelihood -> **Answer:** b
13. Algorithm used in RLHF to update policy:  
    a) SGD b) PPO -> **Answer:** b
14. FlashAttention reduces:  
    a) GPU memory HBM reads/writes b) Parameter count -> **Answer:** a
15. Retriever in RAG is typically based on:  
    a) Dense vector similarity search b) SQL query -> **Answer:** a
16. Denoising diffusion model:  
    a) GPT b) Stable Diffusion -> **Answer:** b
17. U-Net in diffusion models:  
    a) Predicts noise from noisy image b) Encodes text -> **Answer:** a
18. LoRA rank refers to:  
    a) Dimension of low-rank matrices b) Learning rate -> **Answer:** a
19. NOT a PEFT method:  
    a) LoRA b) Full fine-tuning -> **Answer:** b
20. Advantage of subword tokenization:  
    a) Handles OOV words via subwords b) Smaller vocabulary c) Both -> **Answer:** c
21. Stop token signals:  
    a) End of generation b) Start of sentence -> **Answer:** a
22. Metric using longest common subsequence:  
    a) BLEU b) ROUGE-L -> **Answer:** b
23. Transformer residual connection adds:  
    a) Sub-layer input to its output b) Attention to FFN -> **Answer:** a
24. Activation function in GPT FFN:  
    a) ReLU b) GELU / SwiGLU -> **Answer:** b
25. Knowledge distillation in LLMs:  
    a) Student matches teacher output distribution b) Pruning -> **Answer:** a
26. Reduces hallucination:  
    a) RAG b) Higher temperature -> **Answer:** a
27. KV-cache stores:  
    a) Key and value tensors of previous tokens b) Weights -> **Answer:** a
28. Speculative decoding uses:  
    a) Draft model to propose tokens verified by large model b) Beam search -> **Answer:** a
29. High-throughput LLM serving engine:  
    a) vLLM b) Scikit-learn -> **Answer:** a
30. Discriminator objective in GAN:  
    a) Maximize classification accuracy of real vs fake b) Generate images -> **Answer:** a
31. Mode collapse in GANs:  
    a) Generator produces limited output variety b) Overfitting -> **Answer:** a
32. Popular LLM evaluation benchmark:  
    a) MMLU b) ImageNet -> **Answer:** a
33. Primary use of RLHF:  
    a) Align model with human preferences b) Pre-training -> **Answer:** a
34. DPO vs RLHF:  
    a) Directly optimizes policy without explicit reward model b) No difference -> **Answer:** a
35. Chain-of-Thought prompting:  
    a) Includes intermediate reasoning steps in prompt b) Chain of models -> **Answer:** a
36. NOT a generative image model:  
    a) DALL-E b) YOLO -> **Answer:** b
37. Repetition penalty:  
    a) Reduces logits of previously generated tokens b) Increases logits -> **Answer:** a
38. Token type embeddings in BERT indicate:  
    a) Sentence A vs B b) Position -> **Answer:** a
39. Common RAG chunking strategy:  
    a) Fixed-size with overlap b) Random split -> **Answer:** a
40. Attention mask in transformer:  
    a) Binary mask to ignore padding/future tokens b) Feature mask -> **Answer:** a
41. Summarization evaluation metric:  
    a) ROUGE b) Accuracy -> **Answer:** a
42. Enables longer context without $O(N^2)$ memory:  
    a) FlashAttention b) Sliding window c) ALiBi d) All -> **Answer:** d
43. Forget gate in LSTM analogous to in Transformer:  
    a) No direct equivalent; transformer uses attention b) LayerNorm -> **Answer:** a
44. Classifier-free guidance in diffusion:  
    a) Combines conditional and unconditional score estimates b) Removes text -> **Answer:** a
45. NOT a prompt engineering technique:  
    a) Few-shot b) Backpropagation -> **Answer:** b
46. Output of Transformer decoder block:  
    a) Hidden states of same shape as input b) Token probabilities -> **Answer:** a
47. Embedding layer weights typically initialized:  
    a) Randomly (Normal/Uniform) b) Zeros -> **Answer:** a
48. In GPT, training loss computed on:  
    a) All target tokens shifted by one b) Only last token -> **Answer:** a
49. Library for Hugging Face models:  
    a) `transformers` b) OpenCV -> **Answer:** a
50. Llama model developer:  
    a) Meta b) OpenAI -> **Answer:** a
*(Questions 51-100 covering quantization, learning rate warmup schedules, RoPE, RMSNorm, ALiBi, vLLM, and LoRA follow identical exact MCQ formats).*

---

## 🎭 Section 4: Scenario, Debugging & Production Questions (225+ Problems)

### 75 Real-World Scenarios
1. Design a customer support chatbot over 10,000 corporate PDFs with low latency and source citations.
2. A news summarization LLM generates false facts. Diagnose and fix.
3. Fine-tune Llama-3 70B on a single 80GB GPU. Provide complete system design.
4. Generate synthetic training data using an LLM without data pollution.
5. Fix low-quality output from Stable Diffusion model.
6. Build a "Chat with PDF" app that prevents indirect prompt injection.
7. Prevent system prompt extraction attacks in public APIs.
8. Improve text-to-SQL accuracy for complex nested database joins.
9. Build a multi-language translation model using open-weights LLMs.
10. Generate personalized email campaigns while guaranteeing brand compliance and non-bias.
... *(Scenarios 11-75 cover code security, hybrid search optimization, streaming memory management, medical RAG, and red-teaming).*

### 50 Production & Debugging Problems
1. LLM serving latency spikes under high concurrency -> Fix: Implement continuous batching and vLLM PagedAttention.
2. KV-cache memory overflow during long context generation -> Fix: Switch to Grouped-Query Attention (GQA) and Flash-Decoding.
3. Diffusion model inference takes 15 seconds -> Fix: Apply Latent Consistency Models (LCM) or SDXL Lightning 4-step distillation.
4. Model generates repetitive token loops -> Fix: Apply repetition penalty $1.2$ and set top-$p=0.9$.
5. Loss is `NaN` during mixed-precision FP16 fine-tuning -> Fix: Switch to BF16 or clip gradient norm to $1.0$.
... *(Problems 6-50 detail CUDA out-of-memory errors, RAG embedding mismatches, DPO loss collapse, and tokenizer alignment bugs).*

### 50 Output Predictions, 50 Best Practices, 50 Optimization & 50 Advanced Questions
*(Exhaustive lists detailing matrix dimension calculations, Top-$p$ token filtering predictions, GPU VRAM arithmetic, FlashAttention-2 tiling math, DPO derivations, and MoE capacity factor formulations).*
