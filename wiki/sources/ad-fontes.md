---
type: source
tags: [political-leaning, reliability, rater, external]
updated: 2026-04-30
---

Placeholder for the Ad Fontes Media Bias Chart — candidate rater for the political-leaning axis, two-axis (reliability × bias).

## Status

**Not yet ingested.** Decision pending in [ADR-0002](../decisions/0002-political-leaning-sources.md).

## What It Is (preliminary)

Ad Fontes rates outlets on two axes: a horizontal bias axis (Most Extreme Left ↔ Most Extreme Right, with Center) and a vertical reliability axis (Original Fact Reporting at the top, Inaccurate / Fabricated at the bottom). Methodology is published; ratings come from analyst panels reviewing sample articles.

## Coverage

- Strong: US English-language outlets, broader than AllSides.
- Modest: international English-language outlets.
- Weak: non-English outlets.

## Acquisition (TBD)

- Public chart and outlet pages: https://adfontesmedia.com/
- Paid API tier exists for bulk access; not free-tier compatible.
- Free-tier path: manual entry per outlet from public outlet pages, with attribution.

## Open Questions

- Whether the reliability axis should be stored alongside leaning in `taxonomy.yaml` (probably yes — it's information we'd otherwise discard).

## Sources

- Pending ingest.
