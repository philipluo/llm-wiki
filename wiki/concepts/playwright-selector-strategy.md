---
tags: [concept]
created: 2026-04-14
updated: 2026-04-14
sources: "[[playwright-testing-build-guide-2026-04-14]]"
related: "[[playwright]]"
---

# Playwright 选择器策略

Playwright 测试中最重要的一环：选择器决定测试的稳定性和可维护性。

## 优先级（从高到低）

| 优先级 | 选择器 | 适用场景 | 示例 |
|--------|--------|---------|------|
| ⭐1 | `getByRole` | 按钮、链接、输入框等语义元素 | `page.getByRole('button', { name: '搜索' })` |
| 2 | `getByText` | 任意包含文本的元素 | `page.getByText('提交')` |
| 3 | `getByLabel` | 关联了 `<label>` 的表单元素 | `page.getByLabel('用户名')` |
| 4 | `getByPlaceholder` | 带 placeholder 属性的输入框 | `page.getByPlaceholder('输入关键词')` |
| 5 | `getByTestId` | 显式标记的测试专用属性 | `page.getByTestId('search-btn')` |
| ❌ | CSS 选择器 | **避免使用** | `page.locator('.btn-primary')` |

## 核心原则

**语义优先，结构次之。** 好的选择器告诉你在做什么，坏的选择器告诉你在哪里。

## 为什么避免 CSS 选择器

- 样式改（class 名、层级），选择器失效
- ID 和 class 名不稳定（minify 后更不可读）
- 不传达语义：`.btn-primary` 不知道是提交还是取消

## 测试隔离下的选择器

Playwright 每个测试在**独立进程**运行（`fullyParallel: true`），不共享浏览器状态。这意味着：
- 可以并行运行，加快反馈速度
- 每个测试都需要完整的 setup（`beforeEach`）
- 选择器不需要刻意唯一化（因为不共享页面）

## Playwright 测试隔离 vs 其他框架

| 框架 | 隔离方式 | 速度影响 |
|------|---------|---------|
| Playwright | 独立进程 | ✅ 快（并行） |
| Cypress | 共享进程 + 事务回滚 | ⚠️ 较慢 |
| Selenium | 独立进程 | ✅ 快（但 API 笨重）|
