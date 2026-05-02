---
title: "浏览器自动化Playwright：MCP Server 与 CLI + SKILLS 深度解析"
author: shanhaih (CSDN)
published: 2026-04-12
source_url: https://blog.csdn.net/shanhaih/article/details/159615487
type: article
note: 与技术爬爬虾视频内容高度一致，作为视频的图文补充
---

摘要：当 Playwright 遇上 AI Agent，浏览器自动化的范式正在被彻底重写。本文深入剖析 Playwright 团队推出的两大 AI 集成方案——Playwright MCP Server 与 Playwright CLI with SKILLS。

# Playwright MCP Server
- 仓库: microsoft/playwright-mcp (29.9k+ stars)
- 设计选择: 使用无障碍快照(Accessibility Snapshots)而非截图
- 优势: Token友好、纯文本可解析、精确元素引用
- 支持: Claude Desktop、VS Code、Cursor、Windsurf等所有MCP客户端

# Playwright CLI with SKILLS
- 仓库: microsoft/playwright-cli (6.5k+ stars)
- 设计选择: CLI命令行方式，专为Coding Agent优化
- 核心优势: 比MCP方案减少约4倍Token消耗
- Skills机制: 让Agent通过SKILL.md理解CLI命令
- 支持多会话并行、可视化监控面板(playwright-cli show)

# MCP vs CLI 选型
- Coding Agent(写代码+测试+验证): CLI + SKILLS (Token效率碾压)
- 探索性自动化(爬取、分析): MCP (持久状态+丰富内省)
- 自修复测试/自主工作流: MCP
- CI/CD自动化验证: CLI + SKILLS (轻量确定性)

# 详细内容见原始URL
