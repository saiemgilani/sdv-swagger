# sdv-swagger

Collection of OpenAPI 3.0/3.1 / Swagger specs for the sports-data APIs used across the
SportsDataVerse. Reverse-engineered / community-sourced unless noted.

## ESPN (all sports — by host)

Reverse-engineered OpenAPI 3.1 specs for ESPN's public APIs, organized **by host**.
Endpoint shapes are identical across every sport — `{sport}`/`{league}` are path
parameters — so each spec covers all sports. The `x-espn-schema-source` field on each
spec records whether its schemas are data-backed (captured response bodies) or derived
from documented representative responses.

| Spec | API host | Source |
|---|---|---|
| `espn-core-v2.openapi.yaml` | `sports.core.api.espn.com` — reference data: seasons, events, competitions, athletes, teams, odds, statistics, draft | sdv-internal-refs `espn/` — captured bodies (basketball; sport-agnostic shapes) |
| `espn-site-v2.openapi.yaml` | `site.api.espn.com` — scoreboard, summary, teams, rosters, standings, groups, rankings, injuries, transactions, news | sdv-internal-refs `espn/` — documented |
| `espn-web-v3.openapi.yaml` | `site.web.api.espn.com` — athlete overview/stats/gamelog/splits, statistics-by-athlete, search | sdv-internal-refs `espn/` — documented |
| `espn-cdn.openapi.yaml` | `cdn.espn.com` — rich per-game packages (require `?xhr=1`; league-abbreviation slug) | sdv-internal-refs `espn/` — documented |
| `espn-now.openapi.yaml` | `now.core.api.espn.com` — cross-league news feed | sdv-internal-refs `espn/` — documented |

## Fox Sports (all sports — bifrost)

Reverse-engineered OpenAPI 3.1 spec for the Fox Sports **Bifrost** API
(`api.foxsports.com/bifrost/v1`) that powers foxsports.com. Like ESPN, endpoint
shapes are uniform across sports — `{sport}` is a path parameter — so one spec
covers all sports. Auth is a public `apikey` + `api-version` query pair (no account).

| Spec | API host | Source |
|---|---|---|
| `foxsports-api-openapi.yaml` | `api.foxsports.com/bifrost/v1` — scoreboard, league hub (teamnav / standings / conferences / polls / stats-con / odds), event (data / matchup / recap / odds), team (roster / stats / gamelog / standings / header), search / explore / trending | sdv-internal-refs `fox/` — captured bodies across 11 team sports |

## Hockey (NHL)

| Spec | API host | Source |
|---|---|---|
| `nhl_api_web_openapi.yaml` | `api-web.nhle.com/v1` — **modern** NHL API (incl. EDGE) | fastRhockey `data-raw` |
| `nhl_stats_rest_openapi.yaml` | `api.nhle.com/stats/rest` — stats query API | fastRhockey `data-raw` |
| `nhl_records_openapi.yaml` | `records.nhl.com` — franchise/records/draft | fastRhockey `data-raw` |
| `nhl_api.yaml` | `statsapi.web.nhl.com/api/v1` — **deprecated** (retired Sep 2023) | erunion/sport-api-specifications |

## Baseball (MLB)

| Spec | API host | Source |
|---|---|---|
| `mlb-stats-api.openapi.yaml` | `statsapi.mlb.com` — MLB Stats API | toddrob99/pseudo-r consolidated |
| `statcast-api.openapi.yaml` | `baseballsavant.mlb.com` — Statcast / Baseball Savant | pybaseball/baseballr derived |
| `mlb_game_api.yml` | MLB game API (legacy) | — |

## Basketball

| Spec | API host | Source |
|---|---|---|
| `nba-openapi.json` | `stats.nba.com` | — |
| `pbpstats.json` | `pbpstats.com` | — |

## Football

| Spec | API | Source |
|---|---|---|
| `nfl_api_openapi.yaml` | NFL.com "Shield" data API (`api.nfl.com`) | sdv-internal-refs `nfl/` |
| `cfbd-swagger.json` | CollegeFootballData API (`api.collegefootballdata.com`) | cfbd_starter_pack |
| `247_swagger.json` | 247Sports recruiting API | recruitR |
| `espn_fantasy_v3.json` | ESPN Fantasy v3 | — |

## Cross-sport / odds

| Spec | API | Source |
|---|---|---|
| `the_odds_api.json` / `the_odds_api.yaml` | The Odds API (`api.the-odds-api.com`) | — |
| `cbssports.json` | CBS Sports | — |

> The ESPN and Fox specs above are generated from `sdv-internal-refs` (`espn/`, `fox/`)
> and cover all sports via path parameters. The per-sport endpoint **catalogs** in
> `sdv-internal-refs/espn/catalogs/*_endpoint_catalog.md` remain the breadth reference
> for league-slug coverage across all sports.
