# LLM Wiki

一个用 LLM Agent (Claude Code) 持续维护的个人知识库，遵循 [Karpathy 提出的 "LLM Wiki" 模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)。

## 什么是 LLM Wiki？

传统 RAG 在查询时从原始文档检索，每次都从零开始。LLM Wiki 则让 Agent **增量式地构建**一个持久的、互相链接的 Markdown 知识库——你读 Wiki，Agent 写 Wiki。

核心特点：
- **Agent 维护**：Claude Code 按照 `CLAUDE.md` 中的 Schema 自动入库、更新、清理
- **双层结构**：`raw/` 存原始来源，`wiki/` 存 Agent 生成的知识页面
- **Obsidian 兼容**：使用 `[[wiki-links]]` 语法，支持图谱视图和反向链接
- **复利增长**：每次入库都建立链接，知识网络越用越密

## 目录结构

```
LLM-Wiki/
├── CLAUDE.md          # Agent 操作手册（Schema）
├── index.md           # Wiki 索引（Agent 的入口）
├── log.md             # 操作日志（append-only）
├── raw/               # 原始来源（只读）
│   ├── articles/      # 网页文章
│   └── videos/        # 视频内容摘要
├── wiki/              # Agent 生成的知识库
│   ├── concepts/      # 概念页面
│   ├── entities/      # 实体页面（人、组织、项目）
│   ├── summaries/     # 来源摘要
│   ├── insights/      # 个人感悟
│   └── comparisons/   # 对比分析
└── templates/         # 页面模板
```

## 如何使用

### 方式一：Obsidian 浏览（推荐）

1. **安装 Obsidian** — [官网下载](https://obsidian.md)
2. **打开仓库** — 选择 `LLM-Wiki/` 目录作为 Vault
3. **浏览入口** — 打开 `index.md`，从这里开始探索
4. **图谱视图** — 点击右上角图谱图标，查看知识网络
5. **反向链接** — 每个页面右侧面板显示所有引用

Obsidian 的优势：
- 双向链接可视化（图谱 + 反向链接面板）
- 本地 Markdown，完全掌控
- 插件生态（Dataview、Calendar 等）

### 方式二：AI Agent 维护

如果你想让 AI Agent 帮你维护这个 Wiki，可以使用以下工具：

**Claude Code** (Anthropic 官方)：
1. **安装 Claude Code CLI** — [anthropic.com/claude-code](https://www.anthropic.com/claude-code)
2. **在仓库目录启动** — `cd LLM-Wiki && claude`
3. **告诉 Agent 要做什么**：
   - 「录入这篇文章：[URL]」 — Agent 自动入库
   - 「Wiki 里关于 XX 有什么？」 — Agent 从 Wiki 查找回答
   - 「帮我 Lint 一下 Wiki」 — Agent 健康检查

**OpenCode** (开源替代)：
1. **安装 OpenCode** — `npm install -g opencode` 或 [GitHub](https://github.com/sst/opencode)
2. **在仓库目录启动** — `cd LLM-Wiki && opencode`
3. **使用方式与 Claude Code 相同** — OpenCode 兼容 `CLAUDE.md` Schema

Agent 会自动：
- 抓取来源保存到 `raw/`
- 生成摘要、实体、概念页面到 `wiki/`
- 建立 `[[wiki-links]]` 跨引用
- 更新 `index.md` 和 `log.md`

### 方式三：纯 Markdown 阅读

直接在 GitHub 或任何 Markdown 编辑器中浏览。入口是 `index.md`。

## 内容范围

当前 Wiki 覆盖主题：

- **LLM Wiki 模式** — Karpathy 的理念、RAG vs Wiki 对比
- **Claude Code** — 最佳实践、工作流、配置技巧
- **Playwright** — 测试框架、MCP vs CLI 方案、选择器策略
- **飞书 Bot** — agents-to-im 搭建指南

## 贡献

这是一个个人知识库，不接受外部 PR。但你可以：
- Fork 后作为自己的 LLM Wiki 模板
- 参考 `CLAUDE.md` 写自己的 Agent Schema
- 用 Obsidian 浏览和本地修改

## 致谢

- [Andrej Karpathy](https://gist.github.com/karpathy) — LLM Wiki 模式提出者
- [Claude Code](https://www.anthropic.com/claude-code) — 维护本 Wiki 的 Agent
- [Obsidian](https://obsidian.md) — 双向链接可视化

## License

MIT — 自取自用，不作担保。