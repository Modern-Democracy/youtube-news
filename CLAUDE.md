
## Wiki Maintenance

Keeping the wiki current is part of doing work on this project — see [wiki/CLAUDE.md](wiki/CLAUDE.md) for the full schema and ingest/query/lint workflows. In brief:

- **Ingest new source files as needed.** When a task reads a raw source not yet reflected in the wiki, update `wiki/sources/` and propagate to the pages it informs.
- **Record new lessons learned.** Platform quirks, DCC gotchas, memory/packing constraints, timing findings → the relevant `wiki/platform/` or `wiki/implementation/` page.
- **Record new plans and decisions.** Capture the decision AND its rationale (especially GDD "Option A vs B" resolutions, scope changes, workflow shifts).
- **Update the index.** Any new page must be linked from [wiki/index.md](wiki/index.md).
- **Append to the log.** Every ingest, substantive query, or lint pass gets a dated line in [wiki/log.md](wiki/log.md).
- **Prefer updating over creating.** Before adding a page, check whether the concept already has one.
