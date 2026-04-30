---
type: decision
tags: [taxonomy, political-leaning, sources]
updated: 2026-04-30
---

ADR-0002: Political-leaning labels are sourced from named third-party non-partisan raters and cited per-channel; the project does not assert its own ratings.

## Status

Proposed — 2026-04-30. Open question: which raters to treat as authoritative.

## Context

The taxonomy includes a "political leaning" axis. This is the most contested classification in the project and the one most likely to draw criticism if mishandled. The owner has stated a preference for a non-partisan source that strives for accuracy, and does not have a pre-existing trusted source.

## Decision (Tentative)

Adopt a **multi-rater, citation-required** approach:

- A channel's leaning entry is a list of `{rater, label, retrieved_on}` records, not a single project-asserted value.
- A channel without rater coverage is recorded as `unrated`, not guessed.
- Disagreements between raters are preserved as-is, not averaged into a synthetic consensus.

Candidate raters (to be evaluated, not yet committed):

- **AllSides** — US-centric, methodology blends editorial review, blind surveys, and community feedback. Strongest for US outlets.
- **Ad Fontes Media** — two-axis (reliability × bias) chart, methodology published, paid API for bulk access.
- **Media Bias/Fact Check (MBFC)** — broader international coverage but methodology has drawn criticism for opacity. Use with caveats if at all.
- **Reuters Institute Digital News Report** — academic, country-level rather than per-outlet, useful for regional context.

Non-candidates (excluded):

- Any rater explicitly aligned with a partisan organization.
- Crowdsourced-only ratings without editorial review.

## Rationale

- **Defensibility.** "Per AllSides as of 2026-04-30" is defensible; "we say it's center-left" is not.
- **Auditability.** Citations let a reader verify or challenge a label without reverse-engineering our judgment.
- **Drift handling.** Raters update their ratings; storing `retrieved_on` makes staleness visible.
- **Coverage gaps.** A multi-rater scheme means international channels (where US-focused raters are silent) can carry coverage from a regionally appropriate source instead of being forced into a US frame.

## Open Questions

- Which raters does the owner want to commit to? Recommend starting with AllSides + Ad Fontes for US/English coverage; add region-specific raters as discovered.
- Acquisition path for each rater's data — public web pages (scrape, with attribution) vs paid API. Free-tier preference per project constraints argues for scrape + manual entry initially.
- Re-rating cadence — annual? On-demand?

## Consequences

- Taxonomy schema must support multiple leaning records per channel — see [Taxonomy](../data/taxonomy.md).
- Each rater needs a [`sources/`](../sources/) page documenting methodology, coverage, and acquisition method.
- Channels without coverage from any selected rater stay `unrated` and surface in lint reports, not in leaning-based playlists.

## Sources

- User input, 2026-04-30.
- Source pages: [AllSides](../sources/allsides.md), [Ad Fontes](../sources/ad-fontes.md), [MBFC](../sources/mbfc.md).
