# 多智能体（Multi-Agent）

单个 Agent 能力有限，让多个 Agent 分工、对话、互相验证，才能解决真正复杂的任务。

---

## 19. CAMEL

**论文：** CAMEL: Communicative Agents for "Mind" Exploration of Large Language Model Society
**作者：** Li et al., 2023
**链接：** https://arxiv.org/abs/2303.17760

### 解决了什么问题
多 Agent 协作能否完全自主进行，不需要人工介入。

### 核心方法
两个 Agent，一个扮演 AI Assistant，一个扮演 AI User，通过角色扮演对话来完成任务。不需要人工介入，两个 Agent 自己把任务谈完。

**核心挑战：** 防止对话失控——模型容易偏离角色、陷入死循环或角色翻转。

**整体框架（三个Agent，一个任务）：**
- **Task Specifier**：接收人类粗粒度想法，扩展成具体可执行的详细任务描述
- **AI User（需求方）**：持续给出指令，推动任务进展
- **AI Assistant（执行者）**：负责执行和回应

**Inception Prompting（核心设计）：**
- AI User prompt：明确被告知"你是需求方，不要自己动手解决问题"
- AI Assistant prompt：明确被告知"Never flip roles! Never instruct me!"，维持角色边界
- 终止机制：AI User 判断任务完成时，输出特殊 token `<CAMEL_TASK_DONE>`，对话结束

### 为什么可行
通过精心设计的角色提示和终止机制，让两个 Agent 能够在明确的边界内自主协作，不会陷入死循环。

### AIPM可以学什么
**角色分工是协作的基础。** CAMEL 证明：只要给清楚每个人的角色和边界，多 Agent 可以自主运行。产品经理在做系统设计时，核心就是定义好"谁做什么、什么时候结束"。

---

## 20. AutoGen

**论文：** AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation
**作者：** Wu et al., 2023
**链接：** https://arxiv.org/abs/2308.08155

### 解决了什么问题
CAMEL 证明了多 Agent 协作的可行性，但都是定制方案，换个场景就得重新设计。需要一个通用框架。

### 核心方法
**核心抽象：ConversableAgent**
- **AssistantAgent**：默认用 LLM 驱动，不需要人工干预
- **UserProxyAgent**：代理人类参与对话，关键能力是可以直接执行代码，并把执行结果（包括错误信息）反馈给 AssistantAgent

**两种对话模式：**
- **两 Agent 对话**：UserProxyAgent 执行 AssistantAgent 写的代码，把错误信息回传，AssistantAgent 修改代码，直到成功或达到终止条件
- **群聊（GroupChat）**：多个 Agent 加入 GroupChatManager 管理的群组，循环执行：动态选择下一个发言的 Agent → 让该 Agent 生成回应 → 广播给所有其他 Agent

### 为什么可行
AutoGen 把多 Agent 系统从"论文里的概念"变成了"真的能写代码跑起来的东西"。代码自动执行、错误反馈、重试逻辑都内置了。

### AIPM可以学什么
**Agent产品的最低可靠标准不是一次正确，是能自己发现错误并自我修复。** AutoGen 的 UserProxyAgent 执行代码→捕获错误→回传反馈→Assistant修正的循环，是产品级的容错设计。做 Agent 产品时，"发现并纠正错误"的能力比"一次正确"更重要。

---

## 21. MetaGPT

**论文：** MetaGPT: Meta Programming for a Multi-Agent Collaborative Framework
**作者：** Hong et al., 2023
**链接：** https://arxiv.org/abs/2308.00352

### 解决了什么问题
多 Agent 系统的"级联幻觉"——一个 Agent 输出了错误内容，下游 Agent 把它当成真实信息继续处理，错误逐级放大。

### 核心方法
把人类成熟的工作流（SOP）引入多 Agent 系统，通过结构化中间产物让每个环节都有明确的验证点。

**角色体系（模拟一家软件公司）：**
- **Product Manager**：接收用户需求，输出 PRD
- **Architect**：读取 PRD，输出系统设计文档
- **Project Manager**：读取系统设计，拆解任务，分配给工程师
- **Engineer**：按任务写代码
- **QA Engineer**：写测试用例，执行测试，反馈 bug

**关键设计：**
- **SOP**：每个 Agent 的职责、输入/输出格式事先定义好
- **结构化输出**：Agent 产出的是标准交付物（PRD、设计图、代码文件），不是自由文本
- **共享消息池**：所有 Agent 可以订阅自己关心的消息，减少无效通信

### 📊 效果
在 HumanEval 和 MBPP 代码生成基准上表现很好。

### AIPM可以学什么
**流程规范化是抑制多Agent混乱的最有效手段。** 定义好每个角色的输入/输出格式，本质上就是在做产品架构设计。但要注意：现实任务往往比预定义的 SOP 复杂得多，这是 MetaGPT 的局限。
