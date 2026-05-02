---
tags: [concept]
created: 2026-04-13
updated: 2026-04-13
sources: "[[karpathy-llm-wiki-2026-04-02]]"
related:
  - "[[llm-wiki-pattern]]"
  - "[[andrej-karpathy]]"
---

# RAG vs Wiki

## Definition

两种用 LLM 管理知识的根本性差异：RAG 是"每次查询时检索"，Wiki 是"持续编译和维护"。

## Comparison

| 维度 | RAG（检索增强生成） | Wiki（LLM 维护知识库） |
|------|-------------------|---------------------|
| 核心逻辑 | 查询时从原始文档检索片段 | 知识预先编译进结构化 Wiki |
| 积累性 | 无积累，每次从零开始 | 持久资产，复利增长 |
| 交叉引用 | 不存在，每次重新发现 | 已建好，随新资料自动更新 |
| 矛盾处理 | 每次遇到都要重新判断 | 已标注，持续追踪 |
| 综合分析 | 跨文档需要重复拼凑 | 已反映所有读过的内容 |
| 回答沉淀 | 用完即弃 | 好答案回存为新页面 |
| 维护成本 | 低（无维护） | 低（LLM 做） |
| 代表产品 | NotebookLM, ChatGPT 文件上传 | Karpathy LLM Wiki 模式 |

## Key Insight

RAG 的致命问题不是检索不准，而是**没有积累**。问一个需要综合五篇文档的复杂问题，LLM 每次都得从头来。问完了，答案就没了。

Wiki 模式解决了这个问题：知识编译一次，然后持续保持最新。每一次探索、每一次提问都在让知识库变得更好。

## When to Use Which

- **RAG 适合**：偶尔查一次、不需要积累、快速问答
- **Wiki 适合**：长期研究、需要跨文档综合、知识要沉淀复用

## Connections

- 相关概念：[[llm-wiki-pattern]]
- 提出者：[[andrej-karpathy]]

## References

- Sources: [[karpathy-llm-wiki-2026-04-02]]
