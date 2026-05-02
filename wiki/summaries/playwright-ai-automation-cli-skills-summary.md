---
tags: [summary]
created: 2026-04-15
updated: 2026-04-15
sources:
  - "[[playwright-cli-skills-ai-automation-techshrimp-2026-04-13]]"
  - "[[playwright-mcp-cli-skills-deep-dive-shanhaih-2026-04-12]]"
related:
  - "[[playwright]]"
  - "[[playwright-mcp]]"
  - "[[playwright-cli-tool]]"
---

# Playwright AI 浏览器自动化：CLI + Skill 方案摘要

## 来源

- 视频：技术爬爬虾「告别一切重复枯燥任务，CLI+Skill搭建AI浏览器自动化框架」(YouTube + B站)
- 图文：CSDN 同主题深度解析文章（内容一致，互补）

## 核心论点

Playwright 团队推出两种 AI 集成方案，面向不同场景：

| 方案 | 面向对象 | Token 消耗 | 适合场景 |
|------|---------|-----------|---------|
| Playwright MCP Server | 通用 AI Agent | 较高（Schema+快照） | 探索性工作、自修复测试 |
| Playwright CLI + Skills | Coding Agent | 极低（4x更省） | 写代码+测试+验证循环 |

## CLI + Skills 方案核心

1. **Token 效率碾压**：避免了将 Tool Schema 和无障碍树加载到模型上下文，纯文本命令+简洁响应
2. **0 Token 可能**：很多工作流全程不需要 AI 参与（如抓取电商评论）
3. **Skills 机制**：通过安装 SKILL.md 让 Agent 自动理解 CLI 命令，无需手动教学
4. **多会话并行**：`playwright-cli -s=app1 open ...` + `playwright-cli -s=app2 open ...`
5. **可视化监控**：`playwright-cli show` 实时观察 Agent 操作浏览器的进度

## CLI 常用命令

```bash
# 安装
npm install -g @playwright/cli@latest
playwright-cli install --skills

# 核心操作
playwright-cli open [url]           # 打开浏览器
playwright-cli goto <url>           # 导航
playwright-cli click <ref>          # 点击元素（用快照中的ref）
playwright-cli fill <ref> <text>    # 填充表单
playwright-cli snapshot             # 获取页面快照
playwright-cli screenshot           # 截图

# 会话管理
playwright-cli list                 # 列出所有会话
playwright-cli -s=name open ...     # 命名会话
```

## MCP vs CLI 选型决策树

```
你的场景是什么？
├─ Coding Agent（写代码+测试+验证）→ CLI + Skills
│   └─ 上下文窗口有限？→ CLI + Skills（Token效率碾压）
├─ 探索性自动化（爬取、数据收集）→ MCP
├─ 自修复测试/自主工作流 → MCP
└─ CI/CD 自动化验证 → CLI + Skills
```

## 关键洞察

- **Accessibility Snapshots 而非截图**：MCP 选择可访问性树而非像素级输入，~几KB文本 vs ~100KB截图
- **ref 元素引用**：CLI 和 MCP 都使用快照中的 ref（如 e15）定位元素，精确无歧义
- **与 OpenClaw/QClaw 的关联**：QClaw 本身就支持 browser 工具和 browser-cdp skill，Playwright MCP/CLI 是更底层的选择
