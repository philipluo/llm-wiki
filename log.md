# Knowledge Wiki Log

_(append-only chronological record of wiki operations)_

## [2026-04-13] ingest | Karpathy LLM Wiki Gist
- Added: [[karpathy-llm-wiki-summary]], [[andrej-karpathy]], [[llm-wiki-pattern]], [[rag-vs-wiki]]
- Updated: index.md (initial), raw/_index.md (initial)
- Source: [[karpathy-llm-wiki-2026-04-02]]
- Notes: First ingest. Created 4 wiki pages from 1 source article.

## [2026-04-13] ingest | Claude Code 最佳实践项目
- Added: [[claude-code-best-practice-summary]], [[shanraisshan]], [[claude-code-best-practice-repo]]
- Updated: index.md, raw/_index.md
- Source: [[claude-code-best-practice-2026-04-09]]
- Notes: WeChat article about the 32K Star GitHub repo. Created 3 wiki pages from 1 source.

## [2026-04-13] reflect | 入口比链接更重要
- Added: [[entry-point-matters]]
- Updated: CLAUDE.md (added Reflect operation + insights/ directory), index.md
- Originated: 讨论 Wiki 中双向链接的实际作用时产生
- Notes: 第 0 篇感悟。核心洞察：Agent 靠索引定位，人靠链接探索，两者需要不同的基础设施。

## [2026-04-14] ingest | Playwright 测试项目构建（本地文件 x2）
- Added: [[playwright-testing-build-guide-summary]], [[playwright]], [[playwright-codegen]], [[playwright-selector-strategy]]
- Updated: index.md
- Notes: 来源为本地文件，不是网页。教程内容扎实，概念清晰；工作日志作为原始记录保留，不单独建Wiki页面。

## [2026-04-15] ingest | Playwright AI 浏览器自动化（视频 + 文章）
- Added: [[playwright-ai-automation-cli-skills-summary]], [[playwright-mcp]], [[playwright-cli-tool]], [[techshrimp]]
- Updated: index.md
- Source 1: [[playwright-cli-skills-ai-automation-techshrimp-2026-04-13]]（技术爬爬虾视频，YouTube+B站）
- Source 2: [[playwright-mcp-cli-skills-deep-dive-shanhaih-2026-04-12]]（CSDN图文，与视频内容一致）
- Notes: 视频和文章内容高度一致，合并入库。核心价值是 MCP vs CLI + Skills 的选型决策树，CLI 方案 Token 消耗比 MCP 低约 4 倍。

## [2026-04-18] reflect | 企业知识本体落地路径
- Added: [[enterprise-ontology-pragmatic-path]]
- Updated: index.md (Insights section)
- Originated: 与豆包讨论 Obsidian + Neo4j + LangChain 企业知识方案，经 Wiki Agent 评估后形成
- Notes: 第 2 篇感悟。核心：Obsidian 双链 ≠ 本体；本体三个真实作用（词汇对齐、Schema 约束、数据源路由）；五步落地路径（极简本体 → 数据源映射 → 工具选择规则 → 限制查询范围 → 调度组合）。修正了 entry-point-matters 的结论：双链不是没用，是需要本体约束才有用。

## [2026-04-21] ingest | agents-to-im 飞书 Bot 搭建指南
- Added: [[agents-to-im-feishu-bot]] — 四阶段搭建流程（飞书应用创建 → 本地配置 → 事件订阅 → 测试）
- Updated: index.md (Concepts section)
- Notes: 重命名规范文件名（原文件名含中文和空格），添加 YAML frontmatter，简化内容保留核心流程表和命令参考

## [2026-04-24] lint | Obsidian Frontmatter 格式修复
- Found: 所有 summary/concept 文件的 sources/related 字段用逗号分隔多个 wiki-links，Obsidian 不认
- Actions: 批量修复为 YAML 数组格式
  - 修复 8 个 summary 文件（手动）
  - 修复 22 个 concept 文件（Python 脚本）
- Notes: 正确格式：`sources:\n  - "[[a]]"\n  - "[[b]]"`，错误格式：`sources: "[[a]], [[b]]"`

