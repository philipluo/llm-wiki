---
tags: [entity, project]
created: 2026-04-13
updated: 2026-04-13
sources: "[[claude-code-best-practice-2026-04-09]]"
related: "[[shanraisshan]]"
---

# claude-code-best-practice (GitHub Repo)

## Overview

GitHub 上 3.2 万 Star 的 Claude Code 最佳实践仓库，系统化的 Claude Code 使用指南。

## Key Facts

| 属性              | 值               |
| --------------- | --------------- |
| Star 数          | 3.2 万           |
| GitHub Trending | 日榜第一            |
| 作者              | shanraisshan    |
| 收录技巧            | 86+ 条           |
| 口号              | 「实践造就完美 Claude」 |

## 核心内容

### 1. 模块化讲解
- Agents、Commands、Skills、Hooks、MCP Server、Subagents
- 每个模块都有可直接用的模板

### 2. Hot Features 追踪
实时追踪 Claude Code 最新 Beta 功能：
- **Ultraplan**：云端规划，浏览器审查执行计划
- **Claude Code Web**：云端基础设施，跑长时间任务
- **Auto Mode**：后台安全分类器，替代手动确认
- **Computer Use**：macOS 屏幕控制
- **Code Review**：多 Agent PR 审查
- **无闪烁模式**：`CLAUDE_CODE_NO_FLICKER=1`

### 3. 工作流对比
对比 10+ 套主流 Claude Code 开发工作流：

| 工作流 | Star | 特点 |
|--------|------|------|
| Everything Claude Code | 13.7 万 | 大而全，38 命令、75 Agent、156 Skill |
| Superpowers | 13.5 万 | TDD 优先，Iron Laws 约束代码质量 |
| Spec Kit | 8.5 万 | spec 文档驱动，集成 22+ 工具 |
| gstack | 6.4 万 | 角色扮演，并行冲刺 |
| Get Shit Done | 4.8 万 | 全新 200K 上下文，wave 分批执行 |
| BMAD-METHOD | 4.4 万 | 完整软件生命周期，支持 22+ 平台 |

### 4. 使用方法
```bash
git clone https://github.com/shanraisshan/claude-code-best-practice.git
```
- 从 README 开始理解核心概念
- 查看 `.claude/` 目录下的配置模板
- 按需复制 agents、skills、hooks
- 根据工作流对比表选择适合的开发流程

## 核心洞察

> 这个仓库的价值不在于教你用 Claude Code 的某个功能，而是帮你建立一套**系统化的 Claude Code 使用方法论**。

觉得 Claude Code 用起来没啥感觉？可能不是工具不行，是打开方式不对。

## Connections

- 作者：[[shanraisshan]]
- 被引用：Boris Cherny (Claude Code 创造者) 在 X 上多次引用

## References

- Sources: [[claude-code-best-practice-2026-04-09]]
- GitHub: https://github.com/shanraisshan/claude-code-best-practice
