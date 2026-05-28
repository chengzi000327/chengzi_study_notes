# 技能（Skills）

Agent 如何积累经验、把解决过的问题变成可复用的能力，这是让 Agent 真正"成长"的关键。

---

## 22. Agent Skills for LLMs（综述，2026）

**论文：** Agent Skills for Large Language Models: Architecture, Acquisition, Security, and the Path Forward

### 解决了什么问题
LLM 的能力扩展从提示词工程 → 工具调用 → 技能工程演进，这篇是**第一篇系统梳理 Agent Skills 这个新范式**的综述。

### 核心方法
论文按四条主线组织：架构、技能获取、部署、安全。

**① 架构：Skills 到底是什么**
Skills 不是工具（Tool）也不是提示词模板，而是一个**自包含的目录包**，里面有 SKILL.md（YAML frontmatter + 操作流程说明）以及可选的脚本、参考文档、资源文件。

**关键设计——渐进式披露（Progressive Disclosure）：**
Agent 启动时只把每个 Skill 的元数据（几十个 token）预加载进 system prompt，触发时才把完整正文加载进来。这样即使有几百个 Skill，也不会撑爆上下文。

**② 技能获取：六种方式**
1. **人工编写**——目前最有效的方式（Atlassian、Canva、Stripe 等公司已有生产级 Skill）
2. **RL + 技能库**——成功轨迹被提炼成可复用 Skill（类似 Voyager 的思路）
3. **自主发现**——Agent 在环境里自主探索，从失败经验里提炼 Skill
4. **结构化技能库**——带检索机制的 Skill 库，按任务相似度匹配调用
5. **组合式合成**——多个原子 Skill 拼装成复合 Skill
6. **多 Agent 压缩成单 Agent**——多智能体协作经验编译成单个 Agent 的 Skill

**③ 部署：Computer-Use Agent 是核心应用场景**
CUA（Computer-Use Agent）四层架构：
- 操作系统环境 → 感知-定位-行动流水线 → Skill Library + 路由器 → MCP 连接层

**④ 安全：被严重低估的风险 ⚠️**
三项并行研究（2025.10~2026.2）的实证数据：
- 社区贡献的 Skills 中 **26.1% 存在安全漏洞**
- 攻击方式：在 SKILL.md 的长文本里嵌入恶意指令，Agent 隐式信任直接执行
- 更危险的是"Don't ask again"授权穿透

**四层治理框架（Skill Trust and Lifecycle Governance Framework）：**
按 Skill 来源（自写 → 组织认证 → 社区贡献 → 匿名）分级授权，来源越可信，允许的操作权限越高。

### AIPM可以学什么
**Skills 是 Agent 产品的核心差异化能力。** 如果一个 Agent 产品能让用户积累和复用技能，它就具备了"用越多越强"的网络效应。同时 26.1% 的安全漏洞也提醒我们：Agent Skills 市场是一个需要审核机制的平台——跟你之前选方案 B 时说的"Agent 审核机制"完全一致。
