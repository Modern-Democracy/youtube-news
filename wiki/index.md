# Wiki Index

Central catalog of all wiki pages. Every new page must be linked here.

## Overview

- [Project Overview](./overview.md) — goals, scope, and high-level architecture of the YouTube news playlist maintainer.

## Decisions

- [ADR-0001: Allowlist Safety Model](./decisions/0001-allowlist-safety-model.md) — managed playlists are explicitly opted in; automation never touches anything else.
- [ADR-0002: Political-Leaning Rating Sources](./decisions/0002-political-leaning-sources.md) — which third-party raters we treat as authoritative (open).

## Data

- [Taxonomy Schema](./data/taxonomy.md) — category model (region, leaning, funding, live format) and the YAML file shape that backs it.

## Ingestion

- [YouTube Data API v3 — Quota Budget](./ingestion/quota-budget.md) — staying inside the 10,000 unit/day free tier; per-operation costs and budget allocations.

## Sources

- [AllSides Media Bias Ratings](./sources/allsides.md) — placeholder; non-partisan US-centric leaning ratings.
- [Ad Fontes Media Bias Chart](./sources/ad-fontes.md) — placeholder; reliability + leaning two-axis chart.
- [Media Bias/Fact Check](./sources/mbfc.md) — placeholder; broader international coverage, methodology mixed.
- [YouTube Data API v3 Reference](./sources/youtube-data-api-v3.md) — placeholder; the API surface we depend on.
- [Wikipedia: List of Public Broadcasters](./sources/wikipedia-public-broadcasters.md) — placeholder; seed list for the publicly-funded category.

## Log

- [log.md](./log.md) — append-only record of ingests, queries, and lint passes.
