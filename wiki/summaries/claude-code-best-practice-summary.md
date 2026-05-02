---
tags: [summary]
created: 2026-04-13
updated: 2026-04-13
sources:
  - "[[claude-code-best-practice-2026-04-09]]"
related:
  - "[[shanraisshan]]"
  - "[[claude-code-best-practice-repo]]"
---

# Claude Code 最佳实践开源项目 — 摘要

## 一句话总结

GitHub 3.2 万 Star 的 Claude Code 系统化使用指南，收录 86+ 实战技巧，对比 10+ 主流工作流，被 Claude Code 创造者 Boris Cherny 多次引用。

## 核心要点

1. **项目定位**：不是翻译文档，而是真实踩坑经验总结
2. **模块化讲解**：Agents、Commands、Skills、Hooks、MCP Server、Subagents 全覆盖
3. **Hot Features 追踪**：Ultraplan、Claude Code Web、Auto Mode、Computer Use 等 Beta 功能实时更新
4. **工作流选购指南**：对比 6 大主流工作流，按质量/效率/全家桶/流程驱动分类推荐
5. **拿来即用**：`.claude/` 目录下有现成配置模板，Clone 后按需取用

## 工作流选择速查

| 你的需求 | 推荐工作流 |
|---------|-----------|
| 代码质量第一 | Superpowers (TDD + Iron Laws) |
| 追求开发速度 | Get Shit Done (200K 上下文) |
| 想要完整方案 | Everything Claude Code |
| 流程规范团队 | Spec Kit (spec 驱动) |
| 多任务并行 | gstack (角色扮演 + 并行) |
| 大型项目全流程 | BMAD-METHOD |

## 关键洞察

> 「觉得 Claude Code 用起来没啥感觉？可能不是工具不行，是打开方式不对。」

这个仓库的价值在于建立**系统化的 Claude Code 使用方法论**，从概念理解到配置模板，从工作流选择到最新功能追踪，覆盖使用过程中的绝大多数问题。

## 使用方法

```bash
git clone https://github.com/shanraisshan/claude-code-best-practice.git
```

1. 从 README 理解核心概念区别
2. 查看 `.claude/` 目录的配置模板
3. 按需复制 agents、skills、hooks
4. 根据工作流对比表选择开发流程

## References

- Source: [[claude-code-best-practice-2026-04-09]]
- GitHub: https://github.com/shanraisshan/claude-code-best-practice
- Related: [[shanraisshan]], [[claude-code-best-practice-repo]]
