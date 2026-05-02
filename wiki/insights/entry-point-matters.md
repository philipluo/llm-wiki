---
type: insight
date: 2026-04-13
tags:
  - knowledge-management
  - ai-agent
  - architecture
related: "[[llm-wiki-pattern]], [[rag-vs-wiki]]"
originated: Building and discussing the Knowledge-Wiki with Ai Agent
---

# 入口比链接更重要

## 感悟

建 Wiki 的时候，我本能地觉得双向链接是核心——Obsidian 的图谱视图太酷了，知识点互相连接，仿佛只要链接够多，知识就能自动流动。

但问了一个简单的问题后，这个假设碎了：**双向链接对 AI Agent 有什么用？**

答案是：几乎没用。

Agent 查 Wiki 的路径是 `index.md → 定位 2-3 个页面 → 读完回答`，不是沿链接跳来跳去。`[[wiki-link]]` 对 Agent 来说只是文本，不会触发任何自动行为。Obsidian 那个漂亮的图谱和反向链接面板，Agent 完全看不到。

真正让 Agent 高效的是**入口**——index.md 这个目录。它比任何链接都重要。

## 更深的理解

这揭示了一个不对称：

- **人**靠链接探索——点一个、跳一个，像散步，有意外发现
- **Agent**靠索引定位——读目录、挑候选、精准读取，像查字典

两种方式都有效，但它们需要不同的基础设施。为 Agent 优化 Wiki 结构时，该关注的是「Agent 能多快找到该读的页面」，而不是「页面之间有多少链接」。

## 反直觉的地方

双向链接仍然是好的设计，只是它的价值是**为人服务**，不是为 Agent 服务。这不是缺陷，是分工：

- index.md → Agent 的入口
- 图谱视图 → 人的入口
- `[[wiki-links]]` → 连接两者，让人看到 Agent 构建的网络

## 这个感悟如何影响后续决策

Wiki 的演进路线（分片索引 → 搜索索引 → 语义检索）本质上都是在优化**入口效率**，而不是优化链接密度。方向对了。

> Originated from: 构建 Knowledge-Wiki 时讨论双向链接的实际作用
