# 📚 Generative AI & LLMs Curated Resources & Reference Material

Curated collection of essential documentation, research papers, books, repositories, courses, and engineering blogs for mastering Generative AI and LLM technical interviews.

---

## 📄 Must-Read Foundation Papers

| Paper Title | Authors / Organization | Key Innovation / Why It Matters |
| :--- | :--- | :--- |
| **Attention Is All You Need** | Vaswani et al. (Google, 2017) | Original Transformer architecture, self-attention equations, positional encodings. |
| **LoRA: Low-Rank Adaptation of Large Language Models** | Hu et al. (Microsoft, 2021) | Parameter-efficient fine-tuning via low-rank matrix decomposition ($W_0 + BA$). |
| **QLoRA: Efficient Fine-tuning of Quantized LLMs** | Dettmers et al. (UW, 2023) | NF4 quantization, Double Quantization, and Paged Optimizers. |
| **Direct Preference Optimization (DPO)** | Rafailov et al. (Stanford, 2023) | Closed-form preference optimization replacing PPO and explicit Reward Models. |
| **FlashAttention: Fast and Memory-Efficient Exact Attention** | Dao et al. (Stanford, 2022) | SRAM tiling algorithm reducing HBM reads/writes from $O(N^2)$ to $O(N)$. |
| **vLLM: Efficient Memory Management for LLM Serving** | Kwon et al. (UC Berkeley, 2023) | PagedAttention virtual memory allocation for KV-cache. |
| **Denoising Diffusion Probabilistic Models (DDPM)** | Ho et al. (UC Berkeley, 2020) | Mathematical foundation of modern generative diffusion models. |

---

## 🛠️ Official Framework Documentation

- **[Hugging Face `transformers` Docs](https://huggingface.co/docs/transformers/)**: The standard Python library for loading, fine-tuning, and running LLMs and vision transformers.
- **[Hugging Face `peft` Docs](https://huggingface.co/docs/peft/)**: Official guide to LoRA, QLoRA, Prefix Tuning, and Prompt Tuning implementations.
- **[vLLM Documentation](https://docs.vllm.ai/)**: High-throughput LLM serving engine featuring PagedAttention and continuous batching.
- **[PyTorch Distributed & FSDP](https://pytorch.org/docs/stable/fsdp.html)**: Fully Sharded Data Parallelism for multi-GPU pre-training and fine-tuning.

---

## 📖 Recommended Books & Reading

1. **"Build a Large Language Model (From Scratch)"** by *Sebastian Raschka*
   - *Why It's Worth Reading*: Line-by-line PyTorch implementation of tokenizers, attention mechanisms, GPT models, and instruction fine-tuning loops.
2. **"Designing Machine Learning Systems"** by *Chip Huyen*
   - *Why It's Worth Reading*: Industry-standard guide on ML system architecture, data pipelines, monitoring, and production deployment.
3. **"Deep Learning"** by *Ian Goodfellow, Yoshua Bengio, Aaron Courville*
   - *Why It's Worth Reading*: Core mathematical foundations of backpropagation, probability, and neural networks.

---

## 🎬 YouTube Channels & Educational Content

- **[Andrej Karpathy YouTube](https://www.youtube.com/@AndrejKarpathy)**: "Neural Networks: Zero to Hero" series and "Let's build GPT: from scratch, in code". Absolutely essential viewing.
- **[Jay Alammar - The Illustrated Transformer](https://jalammar.github.io/)**: Visual explanations of Transformers, Attention, BERT, GPT-3, and Stable Diffusion.
- **[Umar Jamil YouTube](https://www.youtube.com/@umarjamilai)**: Mathematical paper walkthroughs and PyTorch implementations from scratch (Attention, Llama 2, RoPE, LoRA).

---

## 🌐 Blogs & Technical Resources

- **[Lil'Log by Lilian Weng (OpenAI)](https://lilianweng.github.io/)**: Deep-dive technical summaries on LLM Centric Agents, RLHF, Prompt Engineering, Diffusion Models, and RAG.
- **[Anyscale / Ray Blog](https://www.anyscale.com/blog)**: Advanced LLM serving benchmarks, multi-GPU orchestration, and latency tuning.
- **[Hugging Face Blog](https://huggingface.co/blog)**: SOTA open-weights announcements, DPO tutorials, and Quantization guides.

---

## 💻 Essential GitHub Repositories

- **[vllm-project/vllm](https://github.com/vllm-project/vllm)**: SOTA serving engine with PagedAttention.
- **[Dao-AILab/flash-attention](https://github.com/Dao-AILab/flash-attention)**: Fast CUDA implementation of exact attention.
- **[huggingface/trl](https://github.com/huggingface/trl)**: Transformer Reinforcement Learning library for SFT, PPO, DPO, and KTO.
- **[Lightning-AI/litgpt](https://github.com/Lightning-AI/litgpt)**: Clean, hackable PyTorch implementations of open LLMs.
