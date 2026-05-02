---
tags: [concept]
created: 2026-04-15
updated: 2026-04-15
sources:
  - "[[playwright-cli-skills-ai-automation-techshrimp-2026-04-13]]"
  - "[[playwright-mcp-cli-skills-deep-dive-shanhaih-2026-04-12]]"
related:
  - "[[playwright]]"
  - "[[playwright-mcp]]"
  - "[[playwright-codegen]]"
---

# Playwright CLI Tool

微软 Playwright 团队推出的命令行浏览器自动化工具，专为 Coding Agent（Claude Code、Copilot CLI）优化，通过 Skills 机制实现极低 Token 消耗。

## 仓库

`microsoft/playwright-cli` — 6.5k+ stars

## 安装

```bash
npm install -g @playwright/cli@latest
playwright-cli install --skills   # 安装 Skills 到项目
```

## Skills 机制

Skills 是让 Coding Agent 理解 CLI 命令的知识投递机制。安装后，Claude Code 等工具会自动读取 Skills 定义文件，无需手动教学。

即使不安装 Skills，Agent 也可以通过 `playwright-cli --help` 自行学习命令用法。

## Token 效率

| MCP 方案 | CLI 方案 |
|---------|---------|
| Tool Schema (~2K tokens) | 纯文本命令 |
| Accessibility Tree (~5-50K/snap) | 几十字节命令 |
| 每轮数千~数万 tokens | 每轮几十~几百 tokens |

**结论：CLI 比 MCP 约减少 4 倍 Token 消耗。**

## 核心命令

```bash
# 浏览器操作
playwright-cli open [url]           # 打开
playwright-cli goto <url>           # 导航
playwright-cli click <ref>          # 点击
playwright-cli fill <ref> <text>    # 填充
playwright-cli type <text>          # 输入
playwright-cli press <key>          # 按键
playwright-cli snapshot             # 页面快照
playwright-cli screenshot           # 截图
playwright-cli close                # 关闭

# 会话管理
playwright-cli -s=name open ...     # 命名会话
playwright-cli list                 # 列出会话

# 存储管理
playwright-cli state-save [file]    # 保存状态
playwright-cli state-load <file>    # 加载状态
playwright-cli cookie-list          # 查看Cookies

# 网络拦截
playwright-cli route <pattern>      # 模拟请求

# 监控
playwright-cli show                 # 可视化监控面板
playwright-cli console              # 控制台消息
playwright-cli network              # 网络请求
```

## 可视化监控面板

`playwright-cli show` 开启实时仪表盘，包含：
- **会话网格**：所有活跃会话的实时屏幕预览
- **会话详情**：选中会话的远程控制视图

Agent 在后台跑自动化时，可以观察进度甚至手动接管。

## 适用场景

- Coding Agent 深度用户（Claude Code / Copilot CLI）
- 写代码 → 跑测试 → 浏览器验证 → 修 bug 循环
- CI/CD 中的自动化验证
- 0 Token 自动化（不需要 AI 参与的工作流）

## 与 QClaw/OpenClaw 的关系

QClaw 内置 browser 工具和 browser-cdp skill 可以直接操控浏览器。Playwright CLI 提供了另一种底层方案，适合需要在 Coding Agent 内部精细控制浏览器的场景。
