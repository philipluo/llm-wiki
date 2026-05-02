---
tags: [summary]
created: 2026-04-13
updated: 2026-04-13
sources:
  - "[[karpathy-llm-wiki-2026-04-02]]"
related:
  - "[[andrej-karpathy]]"
  - "[[llm-wiki-pattern]]"
  - "[[rag-vs-wiki]]"
---

# Karpathy LLM Wiki — 文章摘要

## 一句话总结

Karpathy 提出用 LLM Agent 持续维护个人 Markdown 知识库，取代传统 RAG 的"每次从零检索"模式，实现知识的复利增长。

## 核心要点

1. **问题**：传统 RAG 没有积累，每次提问 LLM 都从原始文档重新检索和拼凑
2. **方案**：让 LLM 增量式构建和维护一个 Wiki（结构化 Markdown 文件集合）
3. **关键区别**：Wiki 是持久资产，知识编译一次，交叉引用已建好，矛盾已标注
4. **三层架构**：Raw Sources（只读）→ Wiki（LLM 维护）→ Schema（规则文件）
5. **三个操作**：Ingest（录入）、Query（提问）、Lint（体检）
6. **为什么有效**：LLM 不会厌倦簿记工作，维护成本接近零

## 实践建议

- 一次 Ingest 一条，边录边审，引导 LLM 重点关注什么
- 好答案要回存到 Wiki，这是复利增长的关键
- 定期 Lint，保持 Wiki 健康
- 中等规模（~100 来源、几百页）直接用 index.md 就够，不需要向量数据库
- Obsidian 是 IDE，LLM 是程序员，Wiki 是代码库

## 社区实现

- **sage-wiki**（Go）：一个二进制文件就能跑，支持增量编译、搜索、问答，可当 MCP Server
- **Claude Code Skill**：一行命令安装
- **Thinking-Space**：专为思维工作流设计的 IDE

## 引申思考

- 精神传承于 Vannevar Bush 的 Memex (1945)——Bush 没解决谁来维护，LLM 解决了
- "Intentionally abstract"——理念文档，不是具体实现，需要和 Agent 一起定制
- 适用于个人成长、深度研究、读书笔记、团队知识库等多种场景

## References

- Source: [[karpathy-llm-wiki-2026-04-02]]
- Related entities: [[andrej-karpathy]]
- Related concepts: [[llm-wiki-pattern]], [[rag-vs-wiki]]
