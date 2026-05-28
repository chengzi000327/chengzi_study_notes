# Transformer架构演进 — 核心论文清单

> 从Encoder-Decoder到Decoder-Only的演变路线

---

## 一、起源：Transformer原始架构

### 1. Attention Is All You Need
- **作者：** Vaswani et al., Google
- **年份：** 2017
- **链接：** https://arxiv.org/abs/1706.03762
- **核心贡献：** 提出完全基于注意力机制的Transformer架构，包含Encoder（双向）和Decoder（单向+Cross-Attention）
- **学习状态：** 📌 待读

---

## 二、Encoder-Only 路线

### 2. BERT: Pre-training of Deep Bidirectional Transformers
- **作者：** Devlin et al., Google
- **年份：** 2019
- **链接：** https://arxiv.org/abs/1810.04805
- **核心贡献：** 纯Encoder架构，双向注意力，Masked Language Model预训练
- **学习状态：** 📌 待读

---

## 三、Encoder-Decoder 路线

### 3. BART: Denoising Sequence-to-Sequence Pre-training
- **作者：** Lewis et al., Facebook
- **年份：** 2019
- **链接：** https://arxiv.org/abs/1910.13461
- **核心贡献：** Encoder-Decoder的去噪自编码器，结合BERT双向编码和GPT单向生成的优点
- **学习状态：** 📌 待读

### 4. Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer (T5)
- **作者：** Raffel et al., Google
- **年份：** 2020
- **链接：** https://arxiv.org/abs/1910.10683
- **核心贡献：** 将所有NLP任务统一为Text-to-Text格式，Encoder-Decoder架构的集大成者
- **学习状态：** 📌 待读

---

## 四、Decoder-Only 路线

### 5. Language Models are Few-Shot Learners (GPT-3)
- **作者：** Brown et al., OpenAI
- **年份：** 2020
- **链接：** https://arxiv.org/abs/2005.14165
- **核心贡献：** 证明了Decoder-Only模型在175B规模下涌现In-Context Learning能力，改变行业方向
- **学习状态：** 📌 待读

### 6. LLaMA: Open and Efficient Foundation Language Models
- **作者：** Touvron et al., Meta
- **年份：** 2023
- **链接：** https://arxiv.org/abs/2302.13971
- **核心贡献：** 开源Decoder-Only模型，证明更小的模型+更多数据可以媲美更大模型
- **学习状态：** 📌 待读

---

## 五、关键理论支撑：为什么Decoder-Only赢了

### 7. Scaling Laws for Neural Language Models
- **作者：** Kaplan et al., OpenAI
- **年份：** 2020
- **链接：** https://arxiv.org/abs/2001.08361
- **核心贡献：** 发现模型性能与参数量、数据量、计算量之间存在幂律关系，Decoder-Only的参数利用率更高
- **学习状态：** 📌 待读

---

## 六、正在读的

### 8. Agent Harness Engineering: A Survey
- **作者：** Li et al., Amazon/CMU/Yale/etc.
- **年份：** 2026
- **链接：** https://openreview.net/pdf?id=eONq7FdiHa
- **核心贡献：** 提出ETCLOVG七层框架，论证Harness > Model
- **学习状态：** 📖 进行中（第1课完成）

### 9. Continuous Latent Diffusion Language Model (Cola DLM)
- **作者：** Guo et al., ByteDance Seed
- **年份：** 2026
- **链接：** https://arxiv.org/abs/2605.06548
- **核心贡献：** 在潜空间做扩散生成的文本模型，非自回归第三条路
- **学习状态：** 📌 待读
