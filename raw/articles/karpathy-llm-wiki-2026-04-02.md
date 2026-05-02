# LLM Wiki — A Pattern for Building Personal Knowledge Bases Using LLMs

**Author:** Andrej Karpathy
**Date:** 2026-04-02
**URL:** https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
**Type:** Gist / Idea document

---

Most people's experience with LLMs and documents looks like RAG: you upload a collection of files, the LLM retrieves relevant chunks at query time, and generates an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. There's no accumulation. Ask a subtle question that requires synthesizing five documents, and the LLM has to find and piece together the relevant fragments every time. Nothing is built up. NotebookLM, ChatGPT file uploads, and most RAG systems work this way.

The idea here is different. Instead of just retrieving from raw documents at query time, the LLM **incrementally builds and maintains a persistent wiki** — a structured, interlinked collection of markdown files that sits between you and the raw sources. When you add a new source, the LLM doesn't just index it for later retrieval. It reads it, extracts the key information, and integrates it into the existing wiki — updating entity pages, revising topic summaries, noting where new data contradicts old claims, strengthening or challenging the evolving synthesis. The knowledge is compiled once and then *kept current*, not re-derived on every query.

This is the key difference: **the wiki is a persistent, compounding artifact.** The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read. The wiki keeps getting richer with every source you add and every question you ask.

You never (or rarely) write the wiki yourself — the LLM writes and maintains all of it. You're in charge of sourcing, exploration, and asking the right questions. The LLM does all the grunt work — the summarizing, cross-referencing, filing, and bookkeeping that makes a knowledge base actually useful over time. In practice, I have the LLM agent open on one side and Obsidian open on the other. The LLM makes edits based on our conversation, and I browse the results in real time — following links, checking the graph view, reading the updated pages. Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase.

## Use Cases

- **Personal**: tracking goals, health, psychology, self-improvement — filing journal entries, articles, podcast notes, building a structured picture of yourself over time.
- **Research**: going deep on a topic over weeks or months — reading papers, articles, reports, incrementally building a comprehensive wiki with an evolving thesis.
- **Reading a book**: filing each chapter, building pages for characters, themes, plot threads. Like fan wikis (e.g. Tolkien Gateway) but built personally as you read.
- **Business/team**: internal wiki maintained by LLMs, fed by Slack threads, meeting transcripts, project documents, customer calls.
- **Competitive analysis, due diligence, trip planning, course notes, hobby deep-dives** — anything where you accumulate knowledge over time.

## Architecture

Three layers:

1. **Raw sources** — curated collection of source documents. Immutable. LLM reads but never modifies.
2. **The wiki** — directory of LLM-generated markdown files. Summaries, entity pages, concept pages, comparisons, overview, synthesis. LLM owns this layer.
3. **The schema** — a document (CLAUDE.md / AGENTS.md) that tells the LLM how the wiki is structured, conventions, and workflows. You and the LLM co-evolve this over time.

## Operations

### Ingest
Drop a new source, tell the LLM to process it. Flow: read → discuss → write summary → update index → update entity/concept pages → append to log. A single source may touch 10-15 wiki pages. Prefer one at a time, stay involved.

### Query
Ask questions against the wiki. LLM searches relevant pages, synthesizes answer with citations. **Good answers can be filed back into the wiki as new pages.** Explorations compound.

### Lint
Periodically health-check: contradictions, stale claims, orphans, missing pages, missing cross-refs, data gaps. LLM suggests new questions and sources.

## Indexing and Logging

- **index.md** — content-oriented catalog. Each page: link + one-line summary + optional metadata. Organized by category. LLM reads index first to find relevant pages. Works well at moderate scale (~100 sources, ~hundreds of pages).
- **log.md** — chronological, append-only. Consistent prefix format (e.g. `## [2026-04-02] ingest | Title`) for parseability with unix tools.

## Optional: CLI Tools

- **qmd** (github.com/tobi/qmd) — local search engine for markdown with hybrid BM25/vector search + LLM re-ranking. CLI + MCP server.
- Build simpler tools as needed.

## Tips

- **Obsidian Web Clipper** — browser extension to convert web articles to markdown.
- **Download images locally** — Obsidian → Settings → Files and links → Attachment folder path → `raw/assets/`. Bind "Download attachments" to hotkey.
- **Obsidian graph view** — best way to see wiki shape: hubs, orphans, clusters.
- **Marp** — markdown-based slide deck format. Obsidian plugin available.
- **Dataview** — Obsidian plugin for querying page frontmatter.
- The wiki is a **git repo** — version history, branching, collaboration for free.

## Why This Works

The tedious part of maintaining a knowledge base is the bookkeeping — updating cross-refs, keeping summaries current, noting contradictions, maintaining consistency across dozens of pages. Humans abandon wikis because maintenance burden grows faster than value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass. The wiki stays maintained because maintenance cost is near zero.

Related in spirit to Vannevar Bush's Memex (1945) — personal, curated knowledge store with associative trails. The part Bush couldn't solve was who does the maintenance. The LLM handles that.

## Note

This document is intentionally abstract. It describes the idea, not a specific implementation. Everything mentioned is optional and modular — pick what's useful, ignore what isn't. The right way to use this is to share it with your LLM agent and work together to instantiate a version that fits your needs.
