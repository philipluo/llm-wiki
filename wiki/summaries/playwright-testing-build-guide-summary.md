---
tags: [summary]
created: 2026-04-14
updated: 2026-04-14
sources:
  - "[[playwright-testing-build-guide-2026-04-14]]"
related:
  - "[[playwright]]"
  - "[[playwright-codegen]]"
  - "[[playwright-selector-strategy]]"
---

# Playwright 自动化测试项目构建指南摘要

## 核心内容

Playwright 入门实操指南，从零构建自动化测试项目（Vue 3 + FastAPI 示例应用）。

## Playwright 项目 7 步初始化流程

1. `mkdir` + `npm init -y` 初始化项目
2. `npm install -D @playwright/test typescript @types/node` 安装依赖
3. 创建 `playwright.config.ts`（baseURL、reporter、projects）和 `tsconfig.json`
4. `npx playwright install chromium firefox webkit` 安装浏览器
5. 创建 `tests/` 目录，编写 `*.spec.ts` 测试文件
6. 在 `package.json` 添加 npm 脚本
7. `npm test` 运行测试

## 项目结构模板

```
playwright-auto-testing/
├── CLAUDE.md              # 给 AI 的上下文（关键！）
├── playwright.config.ts   # 测试配置
├── tsconfig.json
├── package.json
├── tests/
│   └── *.spec.ts          # 测试文件
└── .gitignore
```

## 选择器优先级（强烈推荐）

| 优先级 | 选择器 | 理由 |
|--------|--------|------|
| ⭐1 | `getByRole` | 语义化，最稳定 |
| 2 | `getByText` | 文本内容 |
| 3 | `getByLabel` | 表单标签 |
| 4 | `getByPlaceholder` | 输入框提示 |
| 5 | `getByTestId` | 需要手动加 data-testid |
| ❌避免 | CSS 选择器 | 样式变则失效 |

## Codegen 工作流

```
npm run codegen
→ 浏览器 + Inspector 同时打开
→ 手动操作生成代码
→ 复制到 tests/*.spec.ts
→ 补充 expect 断言
```

## 常用 npm 脚本

| 命令 | 作用 |
|------|------|
| `npm test` | 运行所有测试 |
| `npm run test:headed` | 显示浏览器窗口运行 |
| `npm run test:ui` | Playwright UI 模式 |
| `npm run test:debug` | 打开 Inspector 调试 |
| `npm run codegen` | 代码生成器 |
| `npm run report` | 查看测试报告 |

## 常用断言

```typescript
expect(locator).toBeVisible()
expect(locator).toBeHidden()
expect(page).toHaveTitle(/关键词/)
expect(locator).toHaveValue('值')
expect(locator).toContainText('文本')
expect(locator).toHaveCount(n)
```

## 学习路径建议

1. **Codegen 入门** — 用代码生成器熟悉 Playwright API，边操作边看生成代码
2. **补充断言** — 在生成的代码里加 expect，理解断言的作用
3. **手动编写** — 理解结构后脱离 Codegen 自己写
4. **分组组织** — 用 `test.describe` 分组相关测试
5. **调试模式** — 失败用 `--debug` 逐步排查
