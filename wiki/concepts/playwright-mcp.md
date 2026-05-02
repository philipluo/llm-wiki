---
tags: [concept]
created: 2026-04-15
updated: 2026-04-25
sources:
  - "[[playwright-mcp-cli-skills-deep-dive-shanhaih-2026-04-12]]"
  - "https://www.npmjs.com/package/@playwright/mcp"
  - "https://github.com/microsoft/playwright-mcp"
  - "https://playwright.dev/docs/mcp"
related:
  - "[[playwright]]"
  - "[[playwright-cli-tool]]"
---

# Playwright MCP Server

微软 Playwright 团队推出的 MCP 协议浏览器自动化服务器，让 LLM 通过无障碍树感知和操控网页。

## 仓库

`microsoft/playwright-mcp` — 29.9k+ stars

## 核心设计：Accessibility Snapshots

不使用截图，而使用结构化的无障碍树（Accessibility Tree）：

| 传统截图方案 | Playwright MCP |
|-------------|---------------|
| ~100KB+ 图片 | ~几KB 文本 |
| 需要视觉模型 | 纯文本可解析 |
| 定位模糊 | 精确 ref 元素引用 |
| Token 爆炸 | Token 友好 |

## 配置示例

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

支持 Claude Desktop、VS Code Copilot、Cursor、Claude Code 等。

## MCP Tools

| 工具 | 描述 |
|------|------|
| browser_navigate | 导航到 URL |
| browser_click | 点击元素 |
| browser_fill_form | 批量填表 |
| browser_snapshot | 获取无障碍快照 |
| browser_screenshot | 截图 |
| browser_evaluate | 执行 JS |
| browser_file_upload | 上传文件 |
| browser_pdf | 生成 PDF |

## 运行模式

1. **持久化**（默认）：登录状态跨会话保留
2. **隔离模式**（`--isolated`）：每次会话完全隔离
3. **浏览器扩展**：连接已有 Chrome 标签页，复用登录态

## 适用场景

- 探索性自动化（爬取、竞品分析）
- 自修复测试
- 需要 AI 持续观察页面变化的场景
- IDE 内 AI 助手（Cursor / Claude Desktop）

## 局限

- Token 消耗较高（每次调用含完整 Schema + 快照）
- 单浏览器实例
- 无可视化监控面板

## Claude Code 配置经验

### 正确的 NPM 包名

**⚠️ 常见错误**：网上很多教程使用错误的包名 `@anthropic-ai/mcp-server-playwright`，这个包**不存在**，会导致 MCP server 启动失败。

**正确包名**：`@playwright/mcp`（Microsoft 官方）

### .mcp.json 配置文件位置

**⚠️ MCP 配置是项目级别的，不是全局的。**

每个项目目录需要有自己的 `.mcp.json` 文件，Claude Code 只会读取当前项目目录下的配置：

```
your-project/.mcp.json      ← 正确位置（会被读取）
~/.claude/.mcp.json         ← 不会被自动读取
~/.mcp.json                 ← 不会被自动读取
```

这意味着如果你有多个项目，需要在**每个项目目录**下分别创建 `.mcp.json` 和 `.claude/settings.json`。

批量配置示例（假设项目都在 `/Users/xxx/projects` 下）：

```bash
for dir in /Users/xxx/projects/*/; do
  cat > "$dir/.mcp.json" << 'EOF'
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    }
  }
}
EOF

  mkdir -p "$dir/.claude"
  cat > "$dir/.claude/settings.json" << 'EOF'
{
  "enableAllProjectMcpServers": true
}
EOF
done
```

### Claude Code 配置示例

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    }
  }
}
```

同时需要在 `settings.json` 中启用：

```json
{
  "enableAllProjectMcpServers": true
}
```

### 启动失败排查

如果 `/mcp` 显示 server 状态为 "failed"：

1. **检查包名**：手动测试 `npx -y @playwright/mcp --help`
2. **检查 npm 404**：如果返回 404，说明包名错误
3. **检查配置文件位置**：确保 `.mcp.json` 在项目目录下
4. **重启 Claude Code**：修改配置后需要重启

### 其他可选包

如果官方包不适合，还有社区包：

| 包名 | 版本 | 特点 |
|------|------|------|
| `@executeautomation/playwright-mcp-server` | 1.0.12 | 基础功能 |
| `playwright-mcp-tabbed` | 1.1.2 | 支持并行 agent |
| `playwright-mcp-server` | 1.0.0 | 生成测试用例 |
