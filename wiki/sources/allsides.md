---
type: source
tags: [political-leaning, rater, external]
updated: 2026-04-30
---

Placeholder for AllSides Media Bias Ratings — candidate rater for the political-leaning axis, US-centric.

## Status

**Not yet ingested.** Decision pending in [ADR-0002](../decisions/0002-political-leaning-sources.md).

## What It Is (preliminary)

AllSides assigns each US news outlet one of five labels: Left, Lean Left, Center, Lean Right, Right. Methodology blends editorial review, blind bias surveys, third-party research, and community feedback. Ratings are revisited periodically.

## Coverage

- Strong: US English-language outlets.
- Weak: international outlets, non-English outlets.

## Acquisition (TBD)

Public ratings page is at https://www.allsides.com/media-bias/ratings. No public free API. For free-tier compliance: manual entry or scrape (with attribution), refreshed on a low cadence (quarterly?).

## Open Questions

- Terms of use for scraping the ratings page.
- Whether per-outlet pages carry stable URLs we can cite from `taxonomy.yaml`.

## Sources

- Pending ingest.
