# Wiki Schema & Workflows

This document defines the schema, directory layout, and maintenance workflows for the project wiki — a folder-based markdown knowledge base intended for both LLM ingestion and human reference.

## Layering

The project's knowledge lives in three tiers. Keep them distinct; do not mix.

1. **Raw Sources** — immutable project files (specs, GDDs, datasheets, transcripts, code, third-party docs). Never edited by the wiki workflow.
2. **Wiki** — the LLM-maintained synthesis under `wiki/`. Distills, cross-links, and contextualizes the raw sources. Always cites back.
3. **Schema** — this document (`wiki/CLAUDE.md`). Governs how the wiki is structured and maintained.

Rule of thumb: if a fact lives only in someone's head or in a chat transcript, it belongs in the wiki. If it lives in a source file, the wiki *summarizes and links* — it does not duplicate.

## Directory Layout

```
wiki/
├── CLAUDE.md           # this schema document
├── index.md            # central catalog of all pages, grouped by category
├── log.md              # append-only dated record of ingests, queries, lint passes
├── sources/            # one page per ingested raw source, summarizing it
├── ingestion/          # YouTube API, scraping, rate limits, auth, quotas
├── data/               # schemas, storage formats, dataset shape, retention
├── pipeline/           # ETL stages, scheduling, orchestration, failure modes
├── analysis/           # NLP, topic modeling, entity extraction, metrics
├── platform/           # runtime, infra, deployment, OS quirks
├── implementation/     # lessons learned, gotchas, perf findings, workarounds
└── decisions/          # ADR-style records: option A vs B, scope changes, pivots
```

Add new top-level categories sparingly. Prefer extending an existing category over creating a sibling.

## Page Conventions

Every wiki page must follow these rules.

### Frontmatter

```yaml
---
type: source | reference | lesson | decision | overview
tags: [ingestion, youtube-api, quotas]
updated: 2026-04-30
---
```

- `type` — one of the listed values; drives lint behavior.
- `tags` — lowercase, hyphenated, plural where natural. Reuse existing tags before inventing new ones.
- `updated` — ISO date of last substantive edit (not whitespace/typo fixes).

### Body

- **Purpose line** — the first line after frontmatter is a single sentence stating what this page is for. No preamble, no heading.
- **Headers** — start at `##` (the page title is implied by the filename / index). Keep nesting shallow; if you need `####`, consider splitting.
- **Length cap** — soft limit ~300 lines. When a page exceeds this, split along a natural seam and link the children from the parent (and from `index.md`).
- **Links** — relative markdown only: `[Quotas](./ingestion/quotas.md)`. No Obsidian-style `[[wikilinks]]`. No absolute paths.
- **Sources** — every page ends with a `## Sources` section listing the raw files, URLs, or wiki source-pages the content draws from. A page with no sources is suspect.

### Naming

- Files: `kebab-case.md`.
- One concept per page. If a page's title needs "and", it probably wants to be two pages.

## Core Workflows

### Ingest (new raw source → wiki)

1. **Read** the source end-to-end before writing anything.
2. **Summarize** it as `wiki/sources/<source-name>.md` — a faithful synopsis with section pointers, not a rewrite.
3. **Discuss takeaways** with the user (or yourself, in scratch) to identify which existing pages this source confirms, contradicts, or extends.
4. **Propagate** to 5–15 related pages: update facts, add citations, flag contradictions inline with `> ⚠️ Contradicts ./sources/foo.md — needs reconciliation`.
5. **Link** the new source page from `index.md` under `## Sources`.
6. **Log** the ingest in `log.md`.

### Query (answering a question from the wiki)

Use a **Map-to-Mine** strategy:

1. **Map** — open `index.md`, scan for the relevant category and pages.
2. **Mine** — drill into the candidate pages; follow citations to raw sources only when the wiki summary is insufficient.
3. **Synthesize** — answer the question.
4. **File back** — if the synthesis produced a new fact, lesson, or cross-link not yet in the wiki, write it back to the appropriate page before moving on. The wiki should improve with each query.

### Lint (periodic health check)

Run when pages feel stale or before a milestone. Look for:

- **Contradictions** — pages disagreeing on the same fact. Reconcile or mark explicitly.
- **Orphans** — pages not linked from `index.md` or any other page. Either link or delete.
- **Stale data** — `updated` field older than the related raw source's modification date.
- **Unsourced claims** — assertions without a `## Sources` backing.
- **Oversized pages** — anything well over 300 lines that hasn't been split.
- **Duplication** — two pages covering the same concept; merge into the canonical one.

Record each lint pass as a dated line in `log.md` with a one-line summary of what was fixed.

## Index & Log

- `index.md` is the catalog: grouped by category, one bullet per page with its purpose line as the description. It is the entry point for every query.
- `log.md` is append-only. Format: `- 2026-04-30 — ingest: youtube-data-api-v3 reference (8 pages updated)`. Never delete entries; corrections go in a new entry.

## High-Value Ingest Targets

Track here the raw sources known to exist but not yet fully reflected in the wiki. Promote to `sources/` as ingested; remove from this list when done.

- _(populate as the project grows — e.g., YouTube Data API v3 reference, transcript extraction libraries, classifier model cards, dataset README, deployment runbooks)_

## Maintenance Principles

- **Prefer updating over creating.** Before adding a page, search the wiki for the concept.
- **Capture rationale, not just decisions.** Especially for "Option A vs B" resolutions — the *why* outlives the *what*.
- **Cite always.** A wiki claim without a source is a rumor.
- **Small, frequent passes.** Five minutes of propagation after each ingest beats a quarterly cleanup marathon.

## Sources

- This schema document — authored as the governance contract for `wiki/`.
