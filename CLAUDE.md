# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

# Knowledge-Wiki Schema

This document defines how the Ai Agent operates on this Knowledge Wiki.

## Quick Reference

Common user commands (see `index.md` for details):
- **「录入这篇文章：[链接]」** → Trigger Ingest operation
- **「Wiki 里关于 XX 有什么？」** → Trigger Query operation
- **「帮我 Lint 一下 Wiki」** → Trigger Lint operation
- **「感悟」或「记一笔」** → Trigger Reflect operation

Current wiki page count: ~27 pages (Stage 0, pre-sharding)

## Overview

This is a personal knowledge base following the "LLM Wiki" pattern (Karpathy, 2026).
The Agent maintains a structured, interlinked collection of Markdown files.
You read the wiki; the Agent writes and maintains it.

**Viewing**: Use Obsidian to browse this wiki. The `[[wiki-links]]` syntax and `.obsidian/` config are designed for Obsidian's graph view and backlinks panel. The Agent writes for both Obsidian (human exploration) and index.md (Agent navigation).

## Architecture

Three layers:

1. **raw/** — Source documents (articles, papers, images). Immutable. Agent reads only.
2. **wiki/** — Agent-generated markdown: summaries, entities, concepts, comparisons. Agent owns this layer.
3. **CLAUDE.md** (this file) — The Schema. Rules and conventions for wiki operations.

## Directory Structure

```
Knowledge-Wiki/
├── CLAUDE.md          ← This file (The Schema)
├── index.md           ← Content catalog (Agent maintains)
├── log.md             ← Chronological log (Agent appends)
├── raw/               ← Layer 1: Sources (read-only)
│   ├── _index.md      ← Sources list
│   ├── articles/      ← Web articles as markdown
│   ├── papers/        ← Papers, PDFs, documents
│   └── assets/        ← Downloaded images
├── wiki/              ← Layer 2: Wiki (Agent writes)
│   ├── entities/      ← Entity pages (people, orgs, products, projects)
│   ├── concepts/      ← Concept pages (methods, technologies, theories)
│   ├── comparisons/   ← Comparison analysis pages
│   ├── summaries/     ← Summary/overview pages
│   ├── insights/      ← Polished personal reflections (Reflect operation)
│   └── questions/     ← Good Q&A that deserves its own page
└── templates/         ← Page templates
```

## Operations

### Ingest

When the user provides a new source (URL, file, or text):

1. **Fetch** the source content (use CDP for dynamic pages, web_fetch for static)
2. **Save** the raw content to `raw/articles/` or `raw/papers/` with filename: `{author}-{topic}-{YYYY-MM-DD}.md`
3. **Read** and discuss key takeaways with the user
4. **Write** a summary page in `wiki/summaries/`
5. **Create/update** relevant entity pages in `wiki/entities/`
6. **Create/update** relevant concept pages in `wiki/concepts/`
7. **Update** `index.md` — add the new pages with links and one-line summaries
8. **Append** to `log.md` with format: `## [YYYY-MM-DD] ingest | {Title}`

A single source may touch 10-15 wiki pages. Prefer ingesting one source at a time and staying involved with the user.

### Query

When the user asks a question:

1. **Check Wiki first** — read `index.md` and relevant wiki pages
2. **If Wiki has relevant content** — synthesize answer from Wiki, cite pages with [[wiki-links]]
3. **If Wiki is empty on the topic** — acknowledge the gap, seek external info if needed
4. **If the answer is valuable** — offer to file it back as a new page in `wiki/questions/`

Good answers compound. A comparison, an analysis, a discovered connection — these should not disappear into chat history.

### Lint

Periodically health-check the wiki. Look for:
- Contradictions between pages
- Stale claims superseded by newer sources
- Orphan pages with no inbound links
- Important concepts mentioned but lacking their own page
- Missing cross-references
- Data gaps that could be filled with a web search
- Suggest new questions to investigate and new sources to look for

Append results to `log.md` with format: `## [YYYY-MM-DD] lint | Wiki Health Check`

### Reflect

Turn fuzzy intuitions into clear insights. The act of writing a polished piece is itself the deepest form of learning.

When to Reflect:
- After a meaningful Ingest (not every one — only when genuine insight emerged)
- When a pattern becomes visible across multiple sources
- When a belief is challenged or refined
- When the user says "感悟" or "记一笔"

How to Reflect:
1. **Identify** the core insight — what shifted in understanding?
2. **Write** a self-contained piece in `wiki/insights/` — it must make sense on its own
3. **Link** to the wiki pages and raw sources that led to this insight
4. **Update** `index.md` — add to Insights section
5. **Append** to `log.md` with format: `## [YYYY-MM-DD] reflect | {Insight Title}`

Insight vs Summary:
- **Summary** = someone else's content, distilled by you
- **Insight** = your own understanding, crystallized by writing

Insight page format:
```markdown
---
type: insight
date: YYYY-MM-DD
tags: [tag1, tag2]
related: "[[wiki-page-1]], [[wiki-page-2]]"
originated: "[[raw-source]] or conversation"
---
# Insight Title

Polished, self-contained reflection...

> Originated from: what triggered this insight
```

## Conventions

### File Naming
- Use lowercase, hyphens for spaces: `karpathy-llm-wiki.md`
- Include date for raw sources: `{author}-{topic}-{YYYY-MM-DD}.md`
- Entity pages: `{entity-name}.md` (e.g., `andrej-karpathy.md`)
- Concept pages: `{concept-name}.md` (e.g., `rag-retrieval-augmented-generation.md`)

### Page Format
Every wiki page should have YAML frontmatter and use [[wiki-links]] for cross-references.

**Templates**: Use `templates/entity.md` for entity pages, `templates/concept.md` for concept pages. Templates are placeholders—replace `{...}` placeholders with actual values. Summaries and insights don't have fixed templates; follow the formats documented below.

```markdown
---
tags: [entity/concept/comparison/summary/question]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: "[[source-file-name]]"
related: "[[other-wiki-page]]"
---

# Page Title

Content here with [[wiki-links]] to other pages.
```

### Cross-References
- Use Obsidian `[[wiki-links]]` for all internal references
- Every mention of an entity or concept that has its own page should be linked
- When creating a new page, check existing pages that should link to it and update them

### index.md Format
Organized by category, each entry with link + one-line summary:

```markdown
# Wiki Index

## Entities
- [[entity-name]] — One-line summary

## Concepts
- [[concept-name]] — One-line summary

## Summaries
- [[summary-name]] — One-line summary (N sources)

## Insights
- [[insight-name]] — One-line summary (date)

## Comparisons
- [[comparison-name]] — One-line summary

## Questions
- [[question-name]] — One-line summary
```

### log.md Format
Consistent prefix for parseability (`grep "^## \[" log.md | tail -5`):

```markdown
## [YYYY-MM-DD] ingest | Article Title
- Added: [[summary-page]], [[entity-page]], [[concept-page]]
- Updated: [[existing-page-1]], [[existing-page-2]]
- Source: [[raw-source-file]]

## [YYYY-MM-DD] query | Question Topic
- Created: [[answer-page]] (from query)
- Key insight: ...

## [YYYY-MM-DD] lint | Wiki Health Check
- Found: N orphans, M contradictions, K missing cross-refs
- Actions: ...
```

## Evolution Plan

The Wiki structure evolves as it grows. **Do not over-engineer early.** Follow this plan when scale milestones are reached.

### Stage 1: 50-100 pages → Shard index

**Trigger**: index.md exceeds ~200 lines or becomes slow to read in one pass.

**Changes**:
- Break `index.md` into a top-level catalog + sub-indexes by domain
- Top-level `index.md` only lists sub-index links + counts
- Sub-index files: `index-{domain}.md` (e.g., `index-ai.md`, `index-dev.md`)

**Directory structure becomes**:
```
wiki/
├── index.md          ← Top-level catalog (links to sub-indexes only)
├── index-ai.md       ← AI/LLM related pages
├── index-dev.md      ← Development practices
├── index-product.md  ← Product related
└── ...
```

**Agent behavior**: Read top-level index → identify relevant domain → read sub-index → locate pages.

### Stage 2: 100-200 pages → Lightweight search index

**Trigger**: Even sub-indexes become large; agent can't efficiently narrow down candidates.

**Changes**:
- Add `wiki/_search.md` — a compact metadata file for agent-only use
- Each entry: `title | tags | one-line summary | source link`
- Organized by tag for fast filtering
- Agent reads `_search.md` instead of full index for query operations

**Format**:
```markdown
# Search Index (Agent only)
## Tag: claude-code
- claude-code-safety-patterns | safety, hooks | 4 rules from real incidents | [[source]]
- claude-code-workflows | workflow, comparison | 6 workflows compared | [[source]]
## Tag: knowledge-management
- llm-wiki-pattern | wiki, pattern | LLM-maintained knowledge base | [[source]]
```

**Agent behavior**: Read `_search.md` → filter by tag/keyword → read 2-3 candidate pages.

### Stage 3: 200+ pages → Semantic retrieval

**Trigger**: Tag-based filtering no longer sufficient; agent misses relevant pages due to vocabulary mismatch.

**Changes**:
- Generate embeddings for all wiki pages
- Use vector search to find top-k relevant pages before reading
- Wiki still provides quality content; embedding only improves retrieval efficiency
- This is complementary, not contradictory: Wiki = content quality, RAG = retrieval efficiency

**Key principle**: Wiki solves RAG's quality problem; RAG solves Wiki's scale problem. They are complementary.

### Invariant across all stages

1. **Ingest is always intentional** — never auto-crawl everything
2. **Curation over quantity** — the value comes from selecting what to ingest
3. **Agent reads Wiki first, external second** — Rule 9 always applies
4. **index.md (or its descendants) is always the agent's entry point**

### How to track milestones

Check page count periodically during Lint operations:
```bash
find wiki/ -name "*.md" ! -name "_*" | wc -l
```
When count crosses a threshold, propose the next evolution step to the user.

## Important Rules

1. **Never modify files in raw/** — sources are immutable
2. **Always update index.md** after creating or renaming pages
3. **Always append to log.md** after every operation
4. **Use [[wiki-links]]** for all internal references
5. **One source at a time** — stay involved with the user during ingest
6. **File good answers** — valuable query results should be saved back to the wiki
7. **Keep frontmatter consistent** — tags, dates, sources, related links
8. **Don't over-engineer** — this schema evolves with use. Keep it simple, iterate.
9. **Prioritize local wiki over external knowledge** — when answering questions, always check the Wiki first. If the Wiki has relevant content, use it as primary source. Only seek external information when Wiki is empty on the topic.
