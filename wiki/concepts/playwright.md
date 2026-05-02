---
tags: [concept]
created: 2026-04-14
updated: 2026-04-14
sources: "[[playwright-testing-build-guide-2026-04-14]]"
related:
  - "[[playwright-codegen]]"
  - "[[playwright-selector-strategy]]"
---
	
# Playwright

微软出品的端到端自动化测试框架，支持 Chromium、Firefox、WebKit 三大浏览器引擎。

## 核心组成

| 包 | 作用 |
|----|------|
| `@playwright/test` | 测试框架（test/expect） |
| `playwright` | 底层浏览器控制 API |
| `@playwright/browser` | 独立浏览器包 |

## Playwright 项目结构

```typescript
// playwright.config.ts
export default defineConfig({
  testDir: './tests',
  fullyParallel: true,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit', use: { ...devices['Desktop Safari'] } },
  ],
});
```

```typescript
// tests/*.spec.ts
import { test, expect } from '@playwright/test';

test.describe('功能分组', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/');
  });

  test('测试用例', async ({ page }) => {
    await expect(page).toHaveTitle(/关键词/);
  });
});
```

## Playwright vs 其他框架

| 特性 | Playwright | Cypress | Puppeteer |
|------|-----------|---------|-----------|
| 浏览器支持 | 3大引擎 | 仅 Chromium | 仅 Chromium |
| API 风格 | async/await | Promise链式 | async/await |
| 测试隔离 | 每个测试独立进程 | 共享进程 | 不含测试框架 |
| Codegen | ✅ 内置 | ❌ 需插件 | ❌ 无 |

## 相关

- [[playwright-codegen]] — 代码生成器，最佳入门方式
- [[playwright-selector-strategy]] — 选择器最佳实践
