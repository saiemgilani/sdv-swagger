# sdv-swagger

Collection of OpenAPI 3.0 / Swagger specs for the sports-data APIs used across the
SportsDataVerse. Reverse-engineered / community-sourced unless noted.

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
| `cfbd-swagger.json` | CollegeFootballData API (`api.collegefootballdata.com`) | cfbd_starter_pack |
| `247_swagger.json` | 247Sports recruiting API | recruitR |
| `espn_fantasy_v3.json` | ESPN Fantasy v3 | — |

## Cross-sport / odds

| Spec | API | Source |
|---|---|---|
| `the_odds_api.json` / `the_odds_api.yaml` | The Odds API (`api.the-odds-api.com`) | — |
| `cbssports.json` | CBS Sports | — |

> ESPN's site/web/core v2 APIs are reverse-engineered as endpoint **catalogs**
> (see `sdv-internal-refs/_notes/*_endpoint_catalog.md`), not a single OpenAPI spec.
