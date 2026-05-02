---
type: insight
date: 2026-04-18
tags: [knowledge-management, enterprise-ai, ontology, graph-database, neo4j, agent-architecture]
related: "[[entry-point-matters]], [[rag-vs-wiki]], [[llm-wiki-pattern]]"
originated: "和豆包讨论 Obsidian + Neo4j + LangChain 企业知识方案，结合 Wiki Agent 的评估"
---

# 企业知识本体：从幻想到落地

## 起因

和豆包讨论用 Obsidian + Neo4j + LangChain 搭建企业知识库。豆包描绘了一个美好的图景：Obsidian 笔记同步到 Neo4j，标签当实体类型，双链自动生成关系，LangChain 的 GraphCypherQAChain 让 Agent 自动生成 Cypher 查询。

听起来很完美。但仔细拆解，有几个关键问题被跳过了。

## 核心认知修正

### Obsidian 双链 ≠ 本体

Obsidian 的标签和双链是**非正式的、个人化的、扁平的**。

- `#python`、`#claude-code`、`#today-I-learned` 都是标签，但不是本体层级
- `[[wiki-link]]` 映射成 Neo4j 关系，技术上可以，但映射出来的图**语义稀疏**
- 你知道 A 和 B 有关联，但不知道是「依赖」、「包含」、「相似」还是「随便写的链接」

真正的本体需要：
- **严格的类型定义**：`ProgrammingLanguage` 是 `Technology` 的子类
- **关系语义**：`uses`、`depends_on`、`extends` 是不同的关系类型
- **约束规则**：`Contract` 必须关联一个 `Customer`

机器需要的不是「有关系」，而是：这是什么类型？是什么关系？有什么约束？不能出现什么脏数据？

### 本体「自动生成工具调度」——目前根本没有全自动方案

没有任何一个开源工具能做到「写完本体 → 自动调度 SQL / ES / Neo4j」。

LangChain、LlamaIndex、CrewAI 全都做不到。它们能做的只是给组件、给模板 Prompt、给连接驱动。但什么时候查 SQL？什么时候查 ES？什么时候走图数据库多跳？怎么组合顺序？**全部要自己设计、自己写调度逻辑。**

所谓「本体自动调度工具」，目前只存在于大厂内部闭源系统、学术论文、产品宣传稿。不在开源世界里。

## 本体在架构中的三个真实作用

不是魔法自动调度，而是三件非常落地的事：

### 1. 统一业务词汇表

MySQL 里叫 `customer_id`，ES 里叫 `client_id`，业务里叫「客户」，模型理解成 `Customer`——本体把这些全部对齐。模型不会因为名字不一样就当成两个东西。

### 2. 给 LLM 一个严格的 Schema

没有本体，LLM 生成 Cypher / SQL 是狂野的。有本体，可以强制：只能查哪些实体、只能用哪些关系、不能出现哪些字段、必须带哪些过滤条件。**这才是真正抗幻觉的地方。**

### 3. 告诉 Agent 哪些数据源能回答什么问题

- 客户基本信息 → MySQL
- 合同全文检索 → ES
- 复杂关系多跳 → Neo4j

本体不是自动调度，而是给调度提供规则。调度还是要自己写。只是有了本体，写出来的东西稳定、可维护。

## 落地路径

```
Step 1: 停止把 Obsidian 当本体 → 从笔记里提炼极简本体（3 类实体 + 4 种关系）
Step 2: 把本体映射到各数据源（MySQL / ES / Neo4j）→ 手写，没有自动
Step 3: 用本体写工具选择规则 → 含「查找」走 ES，含「统计」走 SQL，含「关系」走 Neo4j
Step 4: 让 LLM 只在本体范围内生成查询 → 超出本体的直接拒绝
Step 5: 调度、组合、返回答案 → LangChain 能帮忙，但流程要自己设计
```

## 对之前洞察的修正

之前的结论：

> 双向链接对 Agent 没用，index.md 才是入口

现在的认知：

> 双向链接单独没用，但如果给它加上关系语义（本体约束），它就能变成 Agent 的可查询资产

这不是推翻，是升维——从「链接本身有没有用」到「链接需要什么条件才有用」。

## 一句话

本体的作用不是自动做掉一切，而是让 LLM 不乱做、不幻觉、不胡说。工具链、调度、查询生成，依然要自己设计。只是有了本体，设计出来的东西稳定、可维护、企业可用。

> Originated from: 与豆包讨论 Obsidian + Neo4j + LangChain 企业知识方案，经 Wiki Agent 评估后形成
