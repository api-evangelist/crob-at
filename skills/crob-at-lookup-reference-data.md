---
name: Look up sample teams and type data
description: Read Smogon sample teams for a tier and Pokemon type data for effectiveness lookups.
api: openapi/crob-at-openapi.json
operations: [listSampleTeams, getTypeChartData]
generated: '2026-09-03'
method: generated
source: openapi/crob-at-openapi.json + https://crob.at/api
---

# Look up crob.at reference data

1. **Sample teams** — `listSampleTeams` (`GET https://crob.at/api/samples/{tier}`, e.g. `gen9ou`). Returns an array of `{slug, name, author, tier, views, created_at, url, image, source_url}`; an empty array is a valid "no samples for this tier" answer, and a `400` means the tier contained non-alphanumeric characters. Results are cached server-side for one hour.
2. **Type data** — `getTypeChartData` (`GET /api/type-chart-data`). Returns `[["Bulbasaur", ["Grass", "Poison"]], ...]` pairs; cached for 24 hours.
3. Follow a sample team's `url` (or `url` + `.md` for markdown) to read the full paste, or fetch it via `getTeamBySlug`.

Rules: both operations are anonymous, keyless, CORS-enabled reads with no documented rate limit (../conventions/crob-at-conventions.yml).
