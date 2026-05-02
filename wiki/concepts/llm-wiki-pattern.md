---
tags: [concept]
created: 2026-04-13
updated: 2026-04-13
sources: "[[karpathy-llm-wiki-2026-04-02]]"
related:
  - "[[andrej-karpathy]]"
  - "[[rag-vs-wiki]]"
---

# LLM Wiki Pattern

## Definition

一种用 LLM Agent 持续构建和维护个人知识库的模式。与传统的 RAG（检索增强生成）不同，LLM Wiki 不是在查询时从原始文档检索，而是让 LLM 增量式地构建一个持久的、互相链接的 Markdown 知识库。

## Core Insight

**知识应该编译一次，持续保持最新，而不是每次查询都从原始文档重新推导。**

传统 RAG 的问题：
- 每次提问都从零开始检索和拼凑
- 没有积累，答案用完即弃
- 跨文档综合需要重复工作

LLM Wiki 的解法：
- 知识编译进 Wiki 后持久存在
- 交叉引用已建好，矛盾已标注
- 每加一个来源、每问一个好问题，Wiki 都变得更丰富 → **复利增长**

## Three-Layer Architecture

| Layer | Description | Who Owns |
|-------|-------------|----------|
| Raw Sources | 原始资料，不可变 | Human (收集) |
| The Wiki | LLM 生成的结构化 Markdown | LLM (维护) |
| The Schema | CLAUDE.md，定义 Wiki 规则 | Human + LLM (共同演进) |

## Three Core Operations

### Ingest（录入）
1. 丢入新资料到 raw/
2. LLM 读取 → 讨论要点 → 写摘要页 → 更新实体/概念页 → 更新 index.md → 追加 log.md
3. 一个来源可能牵动 10-15 个 Wiki 页面
4. 推荐：一次录一条，边录边审

### Query（提问）
1. 对着 Wiki 提问
2. LLM 搜相关页面 → 综合回答
3. **好答案回存到 Wiki** → 知识复利增长

### Lint（体检）
1. 定期健康检查
2. 找矛盾、过时信息、孤儿页面、缺失交叉引用
3. 建议新研究问题和资料来源

## Why It Works

- 维护知识库最烦人的是簿记工作，不是阅读和思考
- LLM 不会厌倦，不会忘记更新交叉引用，可以一次改 15 个文件
- 维护成本接近零 → Wiki 不会荒废
- 精神传承：Vannevar Bush 的 Memex (1945)

## Key Quote

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."
> — Andrej Karpathy

## Trade-offs

| Pros | Cons |
|------|------|
| 知识复利增长，不用重复推导 | 需要主动 Ingest，不会自动收集 |
| 维护成本接近零 | 小规模（<10 篇）可能不值得 |
| 好答案沉淀为持久资产 | 大规模（500+ 页）需要搜索工具辅助 |
| 交叉引用和矛盾自动处理 | 不能替代人的分析和判断 |

## Connections

- 提出者：[[andrej-karpathy]]
- 对比：[[rag-vs-wiki]]
- 工具：Obsidian, qmd, Marp, Dataview
- 精神传承：Vannevar Bush Memex

## References

- Sources: [[karpathy-llm-wiki-2026-04-02]]
