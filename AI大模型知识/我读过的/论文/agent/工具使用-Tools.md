# 工具使用（Tools）

Tools 是 Agent 连接外部世界的接口。

---

## 12. Toolformer

**论文：** Toolformer: Language Models Can Teach Themselves to Use Tools
**作者：** Schick et al., 2023
**链接：** https://arxiv.org/abs/2302.04761

### 解决了什么问题
以前让模型用工具，要么靠人工写 few-shot 示例（每个工具手动教），要么专门 fine-tune。两种方式都费人力，而且工具一多就扛不住。

### 核心方法
Toolformer 提出让模型**自己学习"哪里该调工具、调完结果有没有用"**。

**筛选流程：**
1. 用少量人工示例，让 GPT-3 在原始文本里候选插入工具调用，例如把 "the Eiffel Tower is 324 meters tall" 改成 `[QA("How tall is Eiffel Tower?")→324 meters]`
2. 真实执行这个工具调用，拿到返回结果
3. 比较两种情况下模型预测后续文本的困惑度：有工具结果 vs 没有工具结果——只有调用之后困惑度真的下降了，这条数据才保留
4. 用筛选后的数据 fine-tune 模型

### 为什么可行
工具调用必须**"真的帮到了语言建模"**才算有效，乱插入的噪声数据会被自动过滤掉。支持计算器、Wikipedia 搜索、QA 系统、翻译、日历五类工具。

### 📊 效果
在数学题上，Toolformer（6.7B 参数）比 GPT-3（175B）还强。模型小了二十多倍，但因为调用了计算器，反而赢了。

### AIPM可以学什么
**不要问用户想要什么，看用户在行为上的实际受益。** 困惑度筛选的思路——让数据自己说话——完全可以借鉴到产品AB测试和功能评估中。

---

## 13. Gorilla

**论文：** Gorilla: Large Language Model Connected with Massive APIs
**作者：** Patil et al., 2023
**链接：** https://arxiv.org/abs/2305.15334

### 解决了什么问题
让 GPT-4 调用真实 API 时有两个常见问题：
1. **幻觉**——直接编造不存在的参数或函数名
2. **过时**——API 迭代很快，模型使用的函数签名会过时

### 核心方法
Gorilla 针对两个问题各有一套解法。

**① 数据构建 → 解决幻觉**
整理了 TorchHub、Hugging Face 等共 1600+ 个 API 的完整文档，用 self-instruct 方法让 GPT-4 生成（自然语言需求 → 正确 API 调用代码）训练对，基于 LLaMA 做 fine-tune，训练目标就是给定需求，输出正确的 API 调用代码。

**② 推理时检索 → 解决过时**
推理时先从最新 API 文档库里检索最相关的文档片段，塞进上下文再让模型生成调用代码。这样即使 API 在训练后更新了，模型也能看到最新签名。

### 📊 效果
API 调用准确率上超过 GPT-4，尤其在参数正确性上优势明显；幻觉率显著低于 GPT-4 和 Claude。

### AIPM可以学什么
**离线训练永远追不上线上变化。** Gorilla 的推理时检索思路——不把知识固化成参数，而是在使用时动态获取最新信息——是 Agent 产品应对快速迭代环境的通用设计模式。

---

## 14. ToolGen（ICLR 2025）

**论文：** ToolGen: Unified Tool Retrieval and Calling via Generation

### 解决了什么问题
传统工具调用是两步：先用检索模型从工具库捞候选塞进 prompt，再让 LLM 选一个执行。工具一旦到万级规模，候选文档根本塞不进 context，而且检索和生成是两个独立系统，很难联合优化。

### 核心方法
ToolGen 把检索和生成**合成一步**。

**工具虚拟化：** 给每个 tool 分配一个新 token（格式为 `<<工具名&&API名>>`）。

**三阶段训练：**
1. **工具记忆**——输入 tool 名称，输出对应 token，让模型建立名称→token 的映射
2. **检索训练**——输入用户 query，输出工具 token，训练模型直接基于 query 检索相关 tool
3. **Agent 微调**——用完整任务轨迹做 fine-tune，训出能端到端完成任务的 Agent

**推理时：** 用 constrained decoding，只允许生成词表里真实存在的工具 token，幻觉率直接归零。

### 📊 效果
在 ToolBench（47k API）上，工具检索超过 BM25 等所有无监督基线，端到端任务完成率高于同等基座的 ToolLlama，且不需要外部检索器。

### AIPM可以学什么
**拆两步不如合成一步。** 集成检索与生成可以消除两个独立系统之间的误差累积，并且减少对 prompt 长度的依赖。类似的思路可以用在 Agent 产品的多模块协同设计中。
