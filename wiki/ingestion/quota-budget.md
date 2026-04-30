---
type: reference
tags: [youtube-api, quota, ingestion, constraints]
updated: 2026-04-30
---

How the maintainer stays inside the YouTube Data API v3 free-tier quota of 10,000 units/day.

## Hard Constraint

Per [Project Overview](../overview.md): until project funding is available, all operations must fit inside the default free-tier daily quota of **10,000 units** per project. Going over is not "degraded mode" — it is a hard stop until the quota resets at midnight Pacific time.

## Operation Costs (cheat sheet)

Verify against [YouTube Data API v3 Reference](../sources/youtube-data-api-v3.md) before relying on these — costs occasionally change.

| Operation                       | Cost (units) |
|---------------------------------|--------------|
| `search.list`                   | 100          |
| `playlistItems.list`            | 1            |
| `playlistItems.insert`          | 50           |
| `playlistItems.delete`          | 50           |
| `playlists.list`                | 1            |
| `playlists.insert`              | 50           |
| `playlists.update`              | 50           |
| `channels.list`                 | 1            |
| `videos.list`                   | 1            |
| `liveBroadcasts.list`           | 1            |

Headline: **reads are cheap, writes are expensive, search is the most expensive thing you can do**.

## Design Implications

- **Avoid `search.list` in the steady-state loop.** A single search call is 100 units; 100 of them blows the entire daily budget. Use search only for one-off discovery during taxonomy bootstrap, manually triggered.
- **Discover new uploads via channel uploads playlist, not search.** Every channel has a special `uploads` playlist (`UU...` ID, derivable from the `UC...` channel ID by replacing the second character). Polling it via `playlistItems.list` is 1 unit per page (50 items/page).
- **Batch via `id=` parameters.** `videos.list?id=a,b,c,...` (up to 50 IDs) is still 1 unit. Same for `channels.list`.
- **Cache aggressively.** Channel metadata rarely changes; cache it locally and refresh weekly, not per-run.
- **Idempotent updates.** Before inserting a video into a playlist, check it's not already there (free if you already paged the playlist). Avoid 50-unit no-ops.

## Tentative Daily Budget

10,000 units/day, allocated:

| Category                          | Budget | Notes                                                |
|-----------------------------------|-------:|------------------------------------------------------|
| Live channel polling (15-min)     | ~3,000 | `liveBroadcasts.list` + `playlistItems.list` cycles |
| VOD channel uploads polling       | ~2,000 | `playlistItems.list` per channel uploads feed        |
| Playlist mutations (writes)       | ~3,000 | ~60 inserts/deletes/day at 50 units each             |
| Owner-side reads (inventory etc.) |   ~500 | Periodic audit of managed playlists                  |
| **Reserve / burst headroom**      | ~1,500 | Don't run hot to the limit                           |

These are starting estimates. Real numbers come from a week of telemetry once the maintainer is running.

## Telemetry Requirements

The maintainer must log every API call's estimated cost and aggregate by day. A daily summary line goes to a local log; a single-line "today's spend" should be queryable on demand. Without this, we won't know we're about to bust the quota until we do.

## Mitigations If We Bust

In priority order:

1. Drop `search.list` entirely if any remains.
2. Lengthen live-poll interval (15 → 30 min).
3. Drop low-priority channels from the polling set.
4. Defer non-critical writes to the next day.

Funded mitigation (out of scope for now): request a quota increase or move to paid tier.

## Sources

- [YouTube Data API v3 Reference](../sources/youtube-data-api-v3.md).
- User constraint statement, 2026-04-30.
