# CLAUDE.md — sdv-swagger

A **flat collection of OpenAPI / Swagger spec files** for the sports-data APIs used
across SportsDataverse (mostly reverse-engineered / community-sourced). No app, no build,
no server in-repo — the specs are the deliverable. `origin` is
`github.com/saiemgilani/sdv-swagger`.

## Structure

Every spec lives at the repo root (no subdirs); `README.md` is the authoritative index
(per-spec API host + provenance table). Formats: **OpenAPI 3.1 / 3.0 YAML and JSON**, plus
one Swagger 2.0 file. Naming is mixed (`*.openapi.yaml`, `*-swagger.json`, `*_openapi.yaml`,
`*.yml`) — the README, not the filename, is the source of truth for what each describes.

Coverage by provider:

- **ESPN** (by host, OpenAPI 3.1) — `espn-core-v2`, `espn-site-v2`, `espn-web-v3`,
  `espn-cdn`, `espn-now`. `{sport}`/`{league}` are path params, so each spec covers all
  sports; `x-espn-schema-source` marks data-backed vs. documented schemas.
- **Fox** — `foxsports-api-openapi.yaml` (Bifrost `api.foxsports.com/bifrost/v1`).
- **Yahoo** — `yahoo-sports-editorial`, `yahoo-sports-shangrila`, `yahoo-sports-ncp`.
- **NHL** — `nhl_api_web_openapi`, `nhl_stats_rest_openapi`, `nhl_records_openapi`,
  `nhl_api.yaml` (deprecated statsapi, retired Sep 2023).
- **MLB** — `mlb-stats-api`, `mlb-statcast-api` (Baseball Savant, 43 paths),
  `mlb_game_api.yml` (legacy community spec, 103 paths).
- **Basketball** — `nba-stats.openapi.yaml` (unified `stats.nba.com`/`stats.wnba.com`, 4 league
  projections: `nba-stats-nba`, `nba-stats-wnba`, `nba-stats-gleague`, `nba-stats-summer`);
  `nba-live-cdn.openapi.yaml` (concrete CDN file-level URLs, `cdn.nba.com`/`cdn.wnba.com`);
  `nba-openapi.json` (data.nba.com), `pbpstats.json` (api.pbpstats.com).
- **Football** — `nfl_api_openapi.yaml` (NFL.com Shield), `cfbd-swagger.json` (CFBD),
  `247sports-recruit-database.openapi.yaml` (RDB, guest-JWT) + `247sports-site-pages.openapi.yaml`
  (247sports.com front-end page-JSON, auth-free, 35 routes),
  `pff-premium.openapi.yaml` (PFF Premium Stats
  2.0, `premium.pff.com/api/v1`, cookie-auth/paywalled, NFL/NCAA/AAF/UFL),
  `espn_fantasy_v3.json` (**Swagger 2.0**).
- **Cross-sport / odds** — `the_odds_api.openapi.yaml` (The Odds API v4),
  `cbssports-napi.openapi.yaml` (CBS NAPI, 82 endpoints).

## Conventions

- **The README table is part of the deliverable.** Adding/renaming a spec means adding a
  row (spec filename + API host + provenance/source) in the correct provider section.
- Provenance matters: most specs are reverse-engineered and sourced from
  `sdv-internal-refs/` (`espn/`, `fox/`, `yahoo/`, `cbs/`, `247sports/`, `nfl/`, `pff/`) or named
  upstream projects — record the source, and keep ESPN `x-espn-schema-source` accurate
  (captured-bodies vs. documented).
- ESPN/Fox specs are generated from `sdv-internal-refs` and are sport-agnostic via path
  params — don't fork a spec per sport. The per-sport breadth reference is the league-slug
  catalogs in `sdv-internal-refs/espn/catalogs/`, not this repo.
- Prefer the official upstream spec when one exists (e.g. The Odds API v4 is the official
  `the-odds-api/odds-api` spec, not a hand-rolled one).

## Gotchas

- These describe **third-party/undocumented** APIs; auth and access vary and are noted in
  the README (Fox public `apikey`+`api-version`; Yahoo editorial/shangrila need
  `Origin`/`Referer` headers; Yahoo NCP is session-gated and unusable headless; several are
  community-sourced and may drift). Treat each spec's accuracy as best-effort.
- Mixed OpenAPI versions (3.1 / 3.0.x / and one Swagger 2.0 `espn_fantasy_v3.json`) — a
  validator/tool run against the whole tree must handle all of them.
- `.pytest_cache/` is present but there is no in-repo `pytest.ini`/`conftest.py`,
  `package.json`, CI workflow, or hosting config — validation/serving is done with external
  tooling, not committed here. Don't invent an in-repo build/serve command.
- Never add AI co-author trailers to commits or PRs.

## Reference

- Repo: <https://github.com/saiemgilani/sdv-swagger> (consumed by `sportsdataverse-py`'s
  codegen layer + the `sdv-internal-refs` capture pipeline).
