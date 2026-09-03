---
name: Share a Pokemon Showdown team as a visual page
description: Turn a Pokemon Showdown teambuilder export into a permanent shareable crob.at page and read it back.
api: openapi/crob-at-openapi.json
operations: [createTeam, getTeamBySlug, getTeamInPasteBySlug]
generated: '2026-09-03'
method: generated
source: openapi/crob-at-openapi.json + https://crob.at/api
---

# Share a Pokemon Showdown team

1. **Create the page** — `createTeam` (`POST https://crob.at/api/team`, JSON only, no auth or key).
   Body: `{"name": "...", "author": "...", "description": "...", "public": false, "teams": [{"name": "...", "format": "gen9ou", "paste": "<Showdown export>"}]}`.
   `teams` needs at least one entry; `paste` must be Pokemon Showdown export format; team text is capped at 500,000 characters. A `201` returns `{slug, url, image}`.
2. **Read it back** — `getTeamBySlug` (`GET /api/team/{slug}`). For a multi-team paste, fetch one child with `getTeamInPasteBySlug` (`GET /api/team/{slug}/{teamSlug}`).
3. **Agent-friendly rendering** — every team page has a markdown twin: append `.md` to the share URL or send `Accept: text/markdown`.

Rules: creation is rate-limited to 500/hour per client (`429` + `Retry-After` + `X-RateLimit-*` on exceed — see ../rate-limits/crob-at-rate-limits.yml). There is **no replay protection and no delete API**: a retried create makes a duplicate permanent page, so create once and reuse the returned `url` (../conventions/crob-at-conventions.yml). Errors arrive as `{error, message?}` (../errors/crob-at-problem-types.yml).
