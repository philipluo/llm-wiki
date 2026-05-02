---
tags: [concept]
created: 2026-04-14
updated: 2026-04-14
sources: "[[playwright-testing-build-guide-2026-04-14]]"
related: "[[playwright]]"
---

# Playwright Codegen

Playwright 内置的代码生成器，浏览器操作自动生成测试代码。**最佳入门方式。**

## 工作流

```bash
npm run codegen  # 或: npx playwright codegen [url]
```

1. 执行命令，浏览器窗口 + Inspector 窗口同时打开
2. 在浏览器中进行实际操作（点击、输入、导航）
3. Inspector 实时生成对应 Playwright 代码
4. 复制生成的代码到测试文件
5. 在代码中补充 `expect` 断言

## 生成示例

```typescript
// 生成的代码（需要自己补充断言）
test('搜索并查看详情', async ({ page }) => {
  await page.goto('http://localhost:3000/');
  await page.getByPlaceholder('输入关键词').click();
  await page.getByPlaceholder('输入关键词').fill('测试关键词');
  await page.getByRole('button', { name: '搜索' }).click();
  await page.getByRole('link', { name: '测试关键词' }).click();
});

// 补充断言后
test('搜索并查看详情', async ({ page }) => {
  await page.goto('http://localhost:3000/');
  await page.getByPlaceholder('输入关键词').fill('测试关键词');
  await page.getByRole('button', { name: '搜索' }).click();
  await expect(page.getByRole('link', { name: '测试关键词' })).toBeVisible();
});
```

## 学习价值

- **熟悉 API** — 边操作边看生成代码，建立直觉
- **学选择器** — 观察 Codegen 优先用什么选择器（`getByRole` > `getByPlaceholder`）
- **学断言** — 理解什么值得 assert，什么不用

## 局限

- 生成的代码不一定最优（可能用 `click()` 而不是 `fill()`）
- 不会自动加 `expect`，需要自己补充
- 复杂交互流程生成代码较长，需要人工精简
