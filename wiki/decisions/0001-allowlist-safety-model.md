---
type: decision
tags: [safety, allowlist, design]
updated: 2026-04-30
---

ADR-0001: The maintainer service operates exclusively on playlists that have been explicitly opted in via an allowlist; everything else on the owner's channel is invisible to it.

## Status

Accepted — 2026-04-30.

## Context

The owner's YouTube channel is multi-purpose. It contains news playlists (in-scope for this project) alongside unrelated personal/topical playlists (out-of-scope). The maintainer service has write access to the entire channel via OAuth; there is no per-playlist permission scope in the YouTube Data API.

Two safety models were considered:

- **Allowlist** — service maintains a list of managed playlist IDs; anything not on the list is untouchable.
- **Denylist** — service treats all playlists as managed by default, with an exclusion list of those to ignore.

## Decision

**Allowlist.** A playlist is managed only if it appears in `config/allowlist.yaml` (or equivalent), keyed by YouTube playlist ID with a human-readable label.

A playlist newly created by the owner outside this list is invisible to the service until explicitly added. There is no auto-enrollment heuristic (no name-prefix matching, no tag inference) for write operations — those may be used as *suggestions* during the audit step, but enrollment is always a manual decision.

## Rationale

- **Failure mode asymmetry.** A bug in the denylist (missed entry, stale ID, regex slip) silently corrupts personal content. A bug in the allowlist fails by *not* maintaining a news playlist — visible, recoverable, no data loss.
- **New-content safety.** The owner continues to add unrelated playlists over time. Allowlist defaults those to safe; denylist defaults them to at-risk.
- **Auditability.** "Did the service touch X?" is answered by checking whether X is in one short file.
- **Reviewability.** The allowlist is a small, human-readable artifact that can be diffed in PRs.

## Consequences

- Onboarding a new managed playlist is a deliberate two-step: create it (or identify it), then add its ID to the allowlist.
- The audit script must produce a *suggested* allowlist for review, never auto-apply.
- All write operations in code must take the allowlist as input and assert membership before mutating. This assertion belongs at the lowest sensible layer — ideally a single chokepoint function — so it cannot be bypassed by a caller.
- An *exclusion notes* file may still be maintained for the owner's reference (playlists known to be out-of-scope, with reasons), but it has no operational effect.

## Sources

- User confirmation, 2026-04-30.
- [Project Overview](../overview.md).
