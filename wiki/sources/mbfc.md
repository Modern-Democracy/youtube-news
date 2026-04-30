---
type: source
tags: [political-leaning, rater, external]
updated: 2026-04-30
---

Placeholder for Media Bias/Fact Check (MBFC) — candidate rater with broader international coverage, weaker methodology transparency.

## Status

**Not yet ingested.** Use with caveats; see [ADR-0002](../decisions/0002-political-leaning-sources.md).

## What It Is (preliminary)

MBFC publishes per-outlet pages with a bias label (Left, Left-Center, Least Biased, Right-Center, Right, plus Pro-Science, Conspiracy-Pseudoscience, Questionable Source) and a factual reporting score. Methodology has drawn criticism from researchers for opacity and single-rater subjectivity.

## Coverage

- Notable strength: international outlets that AllSides and Ad Fontes do not cover.
- Notable weakness: methodology transparency.

## Recommendation (preliminary)

Treat MBFC as a tertiary source — useful for international coverage where primary raters are silent, but never the sole rater on a politically sensitive entry. Always store alongside any other rater records, not as a replacement.

## Acquisition (TBD)

- Public per-outlet pages: https://mediabiasfactcheck.com/
- No free API; manual entry or scrape (with attribution).

## Sources

- Pending ingest.
