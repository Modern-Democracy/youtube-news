---
type: overview
tags: [project, architecture]
updated: 2026-04-30
---

Project overview: goals, scope, and high-level architecture of the YouTube news playlist maintainer.

## Goal

Automate the curation of news-focused YouTube playlists on the owner's channel, organized by region, political leaning, funding model, and live format. The system schedules routine checks against the YouTube Data API and updates only those playlists that have been explicitly opted in.

## Scope

In-scope:

- Discovering and cataloging news channels worldwide across the defined taxonomy.
- Maintaining a versioned taxonomy file that maps channels to categories.
- Reading the owner's existing playlists, classifying them, and seeding the managed set from the in-scope ones.
- Adding/removing videos in managed playlists on a schedule.
- Handling live broadcasts (24-hour and scheduled) with a tighter refresh cadence than VOD content.

Out-of-scope (initially):

- Any playlist not on the explicit allowlist — see [ADR-0001](./decisions/0001-allowlist-safety-model.md).
- Multi-account / multi-channel management.
- Hosted/cloud deployment — initial target is a local-machine OAuth flow.

## Key Constraints

- **Free-tier quota only.** Stays within the YouTube Data API v3 default of 10,000 units/day until funding allows otherwise. See [Quota Budget](./ingestion/quota-budget.md).
- **Local OAuth.** Desktop credentials, refresh token stored on disk. No service account, no cloud secret manager (yet).
- **Python + `google-api-python-client`.** Standard library elsewhere where reasonable.

## Categories (Taxonomy v0)

Four orthogonal axes; a single channel typically carries tags on multiple axes.

1. **Region** — country and/or language market.
2. **Political leaning** — sourced from third-party non-partisan raters; see [ADR-0002](./decisions/0002-political-leaning-sources.md).
3. **Funding model** — publicly funded, commercial, state-controlled, nonprofit, etc.
4. **Live format** — 24-hour live, scheduled live, VOD-only.

Full schema: [Taxonomy](./data/taxonomy.md).

## Architecture (Sketch)

```
  taxonomy.yaml (canonical)        owner's YouTube channel
        │                                  │
        ▼                                  ▼
  ┌──────────────┐    OAuth (local)  ┌────────────┐
  │  scheduler   │──────────────────▶│ YouTube API │
  │  (cron-like) │                    └────────────┘
  └──────┬───────┘                          ▲
         │                                  │
         ▼                                  │
  ┌──────────────┐    reads/writes         │
  │  maintainer  │─────────────────────────┘
  │   (Python)   │
  └──────┬───────┘
         │
         ▼
   allowlist.yaml + state cache (local files)
```

Refresh tiers (target cadence; tune against quota):

- Live channels: every ~15 min.
- VOD news channels: hourly or every few hours.
- Taxonomy reconciliation / new-channel discovery: weekly, manual review.

## Bootstrap Plan

Order of work:

1. **Wiki scaffolding** ✓ (this commit).
2. **Read-only channel audit** — list owner's existing playlists, classify, seed allowlist + exclusion records.
3. **Taxonomy v0** — fill `data/taxonomy.yaml` from public broadcaster lists and rater data.
4. **Maintainer service** — playlist update logic, scheduling, state cache.
5. **Live handling** — separate path for 24-hour and scheduled streams.

## Sources

- User project brief, 2026-04-30.
- [ADR-0001](./decisions/0001-allowlist-safety-model.md), [ADR-0002](./decisions/0002-political-leaning-sources.md).
