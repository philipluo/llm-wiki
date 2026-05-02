# 如何构建我的第一个 Playwright 自动化测试项目

本文档记录了从零开始构建 Playwright 自动化测试项目的完整过程，包括理论步骤和实际操作。

## 一、项目背景

**目标：** 学习并使用 Playwright 技术完成自动化测试

**测试目标应用：**
- 应用地址：http://localhost:3000/
- 应用类型：Vue 3 + FastAPI 示例应用
- 主要功能：搜索、查询、数据展示

## 二、理论步骤

### 步骤 1：初始化项目

创建一个空目录作为测试项目，初始化 npm 项目：

```bash
mkdir playwright-auto-testing
cd playwright-auto-testing
npm init -y
```

### 步骤 2：安装依赖

Playwright 测试项目需要以下核心依赖：

```bash
npm install -D @playwright/test typescript @types/node
```

| 包名 | 作用 |
|------|------|
| `@playwright/test` | Playwright 测试框架 |
| `typescript` | TypeScript 编译器 |
| `@types/node` | Node.js 类型定义 |

### 步骤 3：创建配置文件

**playwright.config.ts** - Playwright 测试配置：

```typescript
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',           // 测试文件目录
  fullyParallel: true,          // 并行执行测试
  reporter: 'html',             // HTML 测试报告
  use: {
    baseURL: 'http://localhost:3000',  // 测试目标地址
    trace: 'on-first-retry',           // 失败时记录 trace
    screenshot: 'only-on-failure',     // 失败时截图
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit', use: { ...devices['Desktop Safari'] } },
  ],
});
```

**tsconfig.json** - TypeScript 配置：

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "moduleResolution": "bundler",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["tests/**/*", "playwright.config.ts"]
}
```

### 步骤 4：安装浏览器

Playwright 需要下载浏览器引擎：

```bash
npx playwright install chromium firefox webkit
```

### 步骤 5：创建测试文件

测试文件放在 `tests/` 目录，文件名格式为 `*.spec.ts`。

基本结构：

```typescript
import { test, expect } from '@playwright/test';

test.describe('功能分组名称', () => {
  test.beforeEach(async ({ page }) => {
    // 每个测试前的准备工作
    await page.goto('/');
  });

  test('测试描述', async ({ page }) => {
    // 测试步骤
    await expect(page).toHaveTitle(/关键词/);
  });
});
```

### 步骤 6：配置 npm 脚本

在 `package.json` 中添加便捷命令：

```json
{
  "scripts": {
    "test": "playwright test",
    "test:ui": "playwright test --ui",
    "test:headed": "playwright test --headed",
    "test:debug": "playwright test --debug",
    "report": "playwright show-report",
    "codegen": "playwright codegen http://localhost:3000"
  }
}
```

### 步骤 7：运行测试

```bash
npm test                    # 运行所有测试
npm run test:headed         # 显示浏览器运行
npm run test:ui             # UI 模式
npm run report              # 查看测试报告
```

## 三、实际操作实例

### 实例 1：使用 Codegen 自动生成测试代码

Playwright 提供代码生成器，可以通过操作浏览器自动生成测试代码。这是学习 Playwright 的最佳方式。

**操作步骤：**

1. 启动代码生成器：
```bash
npm run codegen
```

2. 浏览器窗口和 Playwright Inspector 窗口同时打开

3. 在浏览器中进行操作（示例）：
   - 点击输入框
   - 输入搜索关键词
   - 点击搜索按钮
   - 点击搜索结果

4. Inspector 自动生成代码：
```typescript
import { test, expect } from '@playwright/test';

test('搜索并查看详情', async ({ page }) => {
  await page.goto('http://localhost:3000/');
  await page.getByPlaceholder('输入关键词').click();
  await page.getByPlaceholder('输入关键词').fill('测试关键词');
  await page.getByRole('button', { name: '搜索' }).click();
  await page.getByRole('link', { name: '测试关键词' }).click();
});
```

5. 复制生成的代码到 `tests/search.spec.ts`

### 实例 2：手动编写测试

**测试首页加载：**

```typescript
import { test, expect } from '@playwright/test';

test.describe('首页 Dashboard', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/');
  });

  test('页面应该正确加载', async ({ page }) => {
    await expect(page).toHaveTitle(/应用/);
  });

  test('应该显示搜索组件', async ({ page }) => {
    const searchInput = page.getByPlaceholder(/搜索/);
    await expect(searchInput).toBeVisible();
  });
});
```

**测试搜索功能：**

```typescript
import { test, expect } from '@playwright/test';

test.describe('搜索功能', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/');
  });

  test('搜索应该显示结果', async ({ page }) => {
    const searchInput = page.getByPlaceholder(/搜索/);
    await searchInput.fill('测试关键词');

    const searchButton = page.getByRole('button', { name: /搜索/ });
    if (await searchButton.isVisible()) {
      await searchButton.click();
    }

    await expect(page.getByText('测试关键词')).toBeVisible();
  });
});
```

## 四、Playwright 选择器优先级

Playwright 提供多种选择器，推荐按以下优先级使用：

| 选择器 | 示例 | 适用场景 |
|--------|------|----------|
| `getByRole` | `page.getByRole('button', { name: '搜索' })` | 最推荐，语义化 |
| `getByText` | `page.getByText('关键词')` | 文本内容选择 |
| `getByLabel` | `page.getByLabel('用户名')` | 表单标签 |
| `getByPlaceholder` | `page.getByPlaceholder('输入关键词')` | 输入框提示 |
| `getByTestId` | `page.getByTestId('search-btn')` | 需要添加 data-testid 属性 |

**避免使用：**
- CSS 选择器如 `page.locator('.search-btn')` - 不稳定，样式可能改变

## 五、常用断言

```typescript
// 元素可见性
await expect(page.getByText('关键词')).toBeVisible();

// 元素不可见
await expect(page.getByText('加载中')).toBeHidden();

// 页面标题
await expect(page).toHaveTitle(/应用/);

// 输入框值
await expect(page.getByPlaceholder('搜索')).toHaveValue('测试关键词');

// 元素包含文本
await expect(page.getByRole('alert')).toContainText('成功');

// 元素数量
await expect(page.getByRole('listitem')).toHaveCount(5);
```

## 六、项目结构总结

```
playwright-auto-testing/
├── CLAUDE.md              # 项目指南（给 AI 的上下文）
├── playwright.config.ts   # Playwright 配置
├── tsconfig.json          # TypeScript 配置
├── package.json           # npm 配置和脚本
├── tests/
│   ├── dashboard.spec.ts      # 首页测试
│   ├── search.spec.ts         # 搜索功能测试
│   └── example.spec.ts        # codegen 生成的测试
└── .gitignore
```

## 七、学习建议

1. **从 Codegen 开始** - 先用代码生成器熟悉 Playwright API
2. **逐步理解选择器** - 学习语义化选择器的优先级
3. **添加断言** - 在生成的代码中补充 `expect` 断言
4. **组织测试** - 使用 `test.describe` 分组相关测试
5. **调试技巧** - 使用 `--debug` 模式逐步执行，查看每一步状态

## 八、常见问题

**Q: 测试运行前需要做什么准备？**

确保目标应用已启动。本项目需要：
- 前端：http://localhost:3000
- 后端：http://localhost:8000

**Q: 测试失败如何调试？**

```bash
npm run test:debug tests/search.spec.ts
```
会打开 Playwright Inspector，可以逐步执行并查看每一步的状态。

**Q: 如何只运行单个测试文件？**

```bash
npm test tests/search.spec.ts
```

**Q: 测试报告在哪里？**

测试运行后，报告保存在 `playwright-report/` 目录，运行 `npm run report` 打开。