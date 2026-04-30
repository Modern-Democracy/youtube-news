---
type: reference
tags: [taxonomy, schema, data]
updated: 2026-04-30
---

The category model that classifies news channels along four orthogonal axes (region, political leaning, funding, live format) and the YAML schema that backs it.

## Axes

A channel carries tags on each axis it has data for. Axes are independent — a channel is not "either regional or by-leaning"; it is both, simultaneously.

### 1. Region

- `country` — ISO 3166-1 alpha-2 code (e.g., `US`, `GB`, `JP`). Multi-country broadcasters list multiple.
- `language` — primary broadcast language(s), ISO 639-1 (e.g., `en`, `es`, `ar`).
- `region_group` — optional human grouping (e.g., `north-america`, `europe-west`, `mena`). Drives playlist naming.

### 2. Political Leaning

Per [ADR-0002](../decisions/0002-political-leaning-sources.md): a list of rater records, never a single project-asserted value.

Each record:

- `rater` — short ID (`allsides`, `ad-fontes`, `mbfc`).
- `label` — rater's own label string, verbatim (`Lean Left`, `Center`, etc.). Do not normalize across raters.
- `retrieved_on` — ISO date.
- `url` — citation link.

### 3. Funding Model

One or more of:

- `public` — publicly funded (license fee, government grant with editorial independence). E.g., BBC, NHK, NPR, DW, ABC AU.
- `state` — state-controlled or state-aligned editorial. E.g., RT, CCTV, PressTV.
- `commercial` — for-profit, ad/subscription supported.
- `nonprofit` — non-state, non-commercial nonprofit. E.g., ProPublica, The Intercept (mixed).
- `unknown`.

`public` and `state` are distinct on purpose; conflating them is a known anti-pattern.

### 4. Live Format

- `live_24h` — continuous 24-hour live news stream.
- `live_scheduled` — recurring scheduled live programming (e.g., evening news broadcast).
- `vod` — primarily video-on-demand uploads.

A channel may carry multiple (e.g., a 24-hour live stream and a separate VOD upload feed).

## YAML Schema

Canonical file: `config/taxonomy.yaml` (path tentative).

```yaml
channels:
  - id: UCXXXXXXXXXXXXXXXXXXXXXX        # YouTube channel ID, not handle
    handle: "@bbcnews"                   # for human reference
    name: "BBC News"
    region:
      countries: [GB]
      languages: [en]
      region_group: europe-west
    leaning:
      - rater: allsides
        label: "Center"
        retrieved_on: 2026-04-30
        url: https://www.allsides.com/news-source/bbc-news
    funding: [public]
    live_format: [live_24h, vod]
    notes: "Separate channel from BBC World News; verify which feeds the 24h stream."
    added_on: 2026-04-30
```

### Required vs Optional

Required: `id`, `name`, `funding`, `live_format`, `added_on`.
Optional but strongly preferred: `region.countries`, `region.languages`, `leaning` (when rater coverage exists).
Always allowed: `notes`.

### IDs, Not Handles

`id` is the canonical YouTube channel ID (`UC...`, 24 chars). Handles (`@bbcnews`) and custom URLs change; channel IDs do not. The handle is stored only for human readability.

## Playlist Mapping

The maintainer derives playlists from the taxonomy by query, not by static assignment. Example queries:

- "All channels where `funding` includes `public`" → "Publicly Funded News" playlist.
- "All channels where `region.countries` includes `JP`" → "News — Japan" playlist.
- "All channels where `live_format` includes `live_24h`" → "Live 24/7 News" playlist.

Each query is paired with a playlist ID in [the allowlist](../decisions/0001-allowlist-safety-model.md). Adding a channel to the taxonomy automatically affects every playlist whose query it now matches; no per-playlist editing needed.

## Lint Rules (taxonomy-specific)

- `id` must match the YouTube channel ID regex (`^UC[A-Za-z0-9_-]{22}$`).
- A channel with `funding: [state]` and `leaning: []` should warn — state-funded channels have known editorial slant; absence of any rater coverage is suspicious, not neutral.
- A channel with `live_format: [live_24h]` and no entry in the live-refresh tier configuration should warn.
- Duplicate `id` is a hard error.

## Open Questions

- Where does the taxonomy file live — repo root, `config/`, `data/`? (Tentative: `config/taxonomy.yaml`.)
- Should `region_group` be free-form or enumerated? Enumerated is safer for playlist naming consistency.
- How do we handle channels that are clearly news-adjacent but not pure news (e.g., commentary shows on news networks)? Probably an `is_news_primary: bool` flag — TBD.

## Sources

- User project brief, 2026-04-30.
- [ADR-0001](../decisions/0001-allowlist-safety-model.md), [ADR-0002](../decisions/0002-political-leaning-sources.md).
- [Project Overview](../overview.md).
