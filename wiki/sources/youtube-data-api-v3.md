---
type: source
tags: [youtube-api, reference, external]
updated: 2026-04-30
---

Placeholder summary of the YouTube Data API v3 — the API surface the maintainer depends on. To be fleshed out during implementation.

## Status

**Not yet ingested.** This page exists so other pages can cite a stable URL. Promote during the first implementation pass.

## Scope (when ingested)

Endpoints we expect to use:

- `channels.list` — channel metadata, default uploads playlist ID.
- `playlists.list`, `playlists.insert`, `playlists.update` — managing the owner's playlists.
- `playlistItems.list`, `playlistItems.insert`, `playlistItems.delete` — reading and mutating playlist contents.
- `videos.list` — batch metadata fetch (50 IDs per call).
- `liveBroadcasts.list`, `search.list?eventType=live` — discovering live streams (cost-sensitive; see [Quota Budget](../ingestion/quota-budget.md)).

OAuth: desktop application flow, refresh token cached locally. Scopes needed (TBD precisely): `youtube`, `youtube.force-ssl` for write operations.

## Authoritative URLs (to ingest)

- API reference root: https://developers.google.com/youtube/v3/docs
- Quota costs: https://developers.google.com/youtube/v3/determine_quota_cost
- OAuth setup: https://developers.google.com/youtube/v3/guides/auth/installed-apps

## Sources

- Pending ingest.
