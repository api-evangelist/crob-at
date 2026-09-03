---
name: Generate and save a random competitive team
description: Generate a usage-weighted random team for any Showdown format and save it to a permanent unlisted URL, idempotently.
api: openapi/crob-at-openapi.json
operations: [generateRandomTeam, saveRandomTeam]
generated: '2026-09-03'
method: generated
source: openapi/crob-at-openapi.json + https://crob.at/api
---

# Generate and save a random team

1. **Generate** — `generateRandomTeam` (`GET https://crob.at/api/random-team/{format}`, e.g. `gen9ou`; format is lowercase alphanumeric). Returns `{teamText, statsDate, cardsHtml}`. Nothing is saved. A `502` means the upstream Smogon usage data could not be read — retry later.
2. **Save (optional)** — `saveRandomTeam` (`POST /api/random-team/{format}/save`) with `{"teamText": "<from step 1>", "idempotencyKey": "<stable 16-64 char [A-Za-z0-9_-] key>"}`. `201` on first save, `200` with the same team when a retry replays the key. Always send an `idempotencyKey` and reuse it for retries of the same team only.
3. Share the returned `url`; the page also has a markdown twin (append `.md`).

Rules: saves are rate-limited to 30/hour per client; generation itself has no documented limit (../rate-limits/crob-at-rate-limits.yml). The saved URL is permanent and unlisted with no delete API (../conventions/crob-at-conventions.yml).
