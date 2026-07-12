# sdv-swagger

Collection of OpenAPI 3.0/3.1 / Swagger specs for the sports-data APIs used across the
SportsDataVerse. Reverse-engineered / community-sourced unless noted.

## ESPN (all sports — by host)

Reverse-engineered OpenAPI 3.1 specs for ESPN's public APIs, organized **by host**.
Endpoint shapes are identical across every sport — `{sport}`/`{league}` are path
parameters — so each spec covers all sports. The `x-espn-schema-source` field on each
spec records whether its schemas are data-backed (captured response bodies) or derived
from documented representative responses. The `{sport}`/`{league}` slug set now covers
the expanded league catalog wrapped in sportsdataverse-py v0.0.59, including soccer
(100+ leagues), cricket, and the NCAA/UFL/XFL/CFL families; the per-league slug list
lives in `sdv-internal-refs/espn/catalogs/`.

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

## Yahoo Sports (all sports — by host)

Reverse-engineered OpenAPI 3.1 specs for the JSON APIs behind `sports.yahoo.com`,
organized **by host**. Endpoint shapes are uniform across sports — sport/league are
path or query parameters — so each spec covers all sports. No auth for the editorial
and shangrila hosts (send `Origin: https://sports.yahoo.com` + `Referer:
https://sports.yahoo.com/`); the NCP resource layer is session-gated (page consent
cookies + crumb) and documented-but-unusable headless. NOT official APIs.

| Spec | API host | Source |
|---|---|---|
| `yahoo-sports-editorial.openapi.yaml` | `api-secure.sports.yahoo.com` — editorial scoreboard + boxscore (game/event feed) | sdv-internal-refs `yahoo/` |
| `yahoo-sports-shangrila.openapi.yaml` | `graphite-secure.sports.yahoo.com` — stats graph: modern generic queries + 90+ registry resources + legacy football queries (105 paths) | sdv-internal-refs `yahoo/` |
| `yahoo-sports-ncp.openapi.yaml` | `sports.yahoo.com` — NCP matrix-param resource layer (session-gated; documented, unusable headless) | sdv-internal-refs `yahoo/` |

## Hockey (NHL)

| Spec | API host | Source |
|---|---|---|
| `nhl_api_web_openapi.yaml` | `api-web.nhle.com/v1` — **modern** NHL API (incl. EDGE) | fastRhockey `data-raw` |
| `nhl_stats_rest_openapi.yaml` | `api.nhle.com/stats/rest` — stats query API | fastRhockey `data-raw` |
| `nhl_records_openapi.yaml` | `records.nhl.com` — franchise/records/draft | fastRhockey `data-raw` |
| `nhl_api.yaml` | `statsapi.web.nhl.com/api/v1` — **deprecated** (retired Sep 2023) | erunion/sport-api-specifications |

## Hockey (HockeyTech / LeagueStat — 20 verified leagues)

OpenAPI 3.1 spec for the **HockeyTech (LeagueStat) feed API** — the JSON
backend behind the PWHL and many pro-minor/junior hockey league sites. One
physical endpoint (`/feed/index.php`) dispatched by query string: `feed=`
selects a family (`modulekit` / `gc` / `statviewfeed`) and `view=` (or `tab=`
for gc) selects the view, so the spec models each view as a
fragment-suffixed path (22 operations). Auth is a **public `key` +
`client_code` query pair** shipped in each league's front-end. The spec's
`x-league-registry` carries **20 live-verified league rows** (2026-07-12):
PWHL, AHL, ECHL, SPHL, CHL (umbrella: Memorial Cup), OHL, WHL, QMJHL, USHL,
BCHL, AJHL, SJHL, OJHL, CCHL, GOJHL, MHL, NOJHL, VIJHL, KIJHL, MJHL — plus
still-valid alternate key generations and an `x-league-registry-unverified`
list (NAHL/FPHL/LNAH/U Sports/PJHL). `statviewfeed` responses are
paren-wrapped JSONP; errors are HTTP-200 in-body sentinels; PBP granularity
varies per league (USHL ships goals/penalties only, no coordinates).

| Spec | API host | Source |
|---|---|---|
| `hockeytech.openapi.yaml` | `lscluster.hockeytech.com/feed/index.php` (serves all 20 leagues; `cluster.leaguestat.com` mirrors QMJHL) — seasons, scorebar, teams, roster, player stats, standings, leaders, brackets, transactions, game centre (summary / clock / preview / pxpverbose PBP), statviewfeed PBP + player profile, gameshifts | sdv-internal-refs `hockeytech/` — 21-view PWHL sweep + 16-league proof captures + 20-league key probe, live 2026-07-12; sdv-py 2026-06-09 design spec |

## Baseball (MLB)

| Spec | API host | Source |
|---|---|---|
| `mlb-stats-api.openapi.yaml` | `statsapi.mlb.com` — MLB Stats API | toddrob99/pseudo-r consolidated |
| `mlb-statcast-api.openapi.yaml` | `baseballsavant.mlb.com` — Statcast / Baseball Savant (43 paths: search/leaderboard/gamefeed/player) | reverse-engineered from live capture |
| `mlb_game_api.yml` | `statsapi.mlb.com` — MLB Stats API (legacy community-sourced comprehensive spec, 103 paths; predates the sdv-built `mlb-stats-api.openapi.yaml`) | community / legacy |

## Basketball

### stats.nba.com / stats.wnba.com (NBA Stats API family)

OpenAPI 3.1 specs for the `stats.nba.com` / `stats.wnba.com` family, reverse-engineered from live
captures and generated by `nba/tools/build.py` in `sdv-internal-refs`. The unified spec covers all
four leagues via the `LeagueID` parameter; the league projections are per-league subsets for
validators and narrower consumers. The live-CDN spec covers `cdn.nba.com` / `cdn.wnba.com` feeds and
uses concrete file-level CDN URLs with `application/json` response bodies (per-game feeds carry a
`game_id` path parameter). Note: `stats.nba.com` TLS-fingerprint-blocks plain HTTP clients — the
sdv-py runtime uses `curl_cffi` with `impersonate="chrome"`.

| Spec | API host / scope | Source |
|---|---|---|
| `nba-stats.openapi.yaml` | `stats.nba.com` — unified spec covering NBA (`LeagueID=00`) + G-League (`20`) + Summer League (`15`) + WNBA (`stats.wnba.com`, `LeagueID=10`); 151 NBA-side + 115 WNBA-side wrappers | sdv-internal-refs `nba/tools/build.py` — live captures |
| `nba-stats-nba.openapi.yaml` | `stats.nba.com` — NBA-only projection (`LeagueID=00`) | sdv-internal-refs `nba/tools/build.py` |
| `nba-stats-wnba.openapi.yaml` | `stats.wnba.com` — WNBA-only projection (`LeagueID=10`) | sdv-internal-refs `nba/tools/build.py` |
| `nba-stats-gleague.openapi.yaml` | `stats.nba.com` — G-League projection (`LeagueID=20`) | sdv-internal-refs `nba/tools/build.py` |
| `nba-stats-summer.openapi.yaml` | `stats.nba.com` — Summer League projection (`LeagueID=15`) | sdv-internal-refs `nba/tools/build.py` |
| `nba-live-cdn.openapi.yaml` | `cdn.nba.com` (NBA) / `cdn.wnba.com` (WNBA) — live data feeds: `scoreboard/todaysScoreboard_00.json` (NBA) / `_10.json` (WNBA), `boxscore/boxscore_{game_id}.json`, `playbyplay/playbyplay_{game_id}.json`, `odds/odds_todaysGames.json`, `staticData/scheduleLeagueV2_2.json` (G-League); concrete file-level URLs with JSON response schemas | sdv-internal-refs `nba/tools/render_openapi.py` — live captures |

### Other Basketball APIs

| Spec | API host | Source |
|---|---|---|
| `nba-openapi.json` | `data.nba.com` — NBA Mobile Stats Feed v2022 (live box scores, play-by-play, schedules, rosters; covers NBA / WNBA / G-League) | community / NBA Data Services |
| `pbpstats.json` | `api.pbpstats.com` — PBP Stats API (player/team/lineup totals, on/off splits, possessions; NBA/WNBA/G-League/NCAA) | community / pbpstats.com |

## Football

| Spec | API | Source |
|---|---|---|
| `nfl_api_openapi.yaml` | NFL.com "Shield" data API (`api.nfl.com`) | sdv-internal-refs `nfl/` |
| `cfbd-swagger.json` | CollegeFootballData API (`api.collegefootballdata.com`) | cfbd_starter_pack |
| `pff-premium.openapi.yaml` | PFF **Premium Stats 2.0** (`premium.pff.com/api/v1`) — By Game/Team/Position/Player over 34 stat reports (passing/receiving/rushing/defense/blocking/special-teams + signature); NFL/NCAA/AAF/UFL via `league`. Session-cookie auth (Clerk `__session` JWT + Phoenix `_premium_key`); paywalled (PFF+). NOT official. | sdv-internal-refs `pff/` — live captures |
| `247sports-recruit-database.openapi.yaml` | 247Sports Recruit Database (`ipa.247sports.com/rdb/v1/`, guest-JWT, curl_cffi) — coaches, recruits, rankings, transfers, predictions | sdv-internal-refs `247sports/` (recruitR) |
| `247sports-site-pages.openapi.yaml` | 247Sports **Site Pages** (`247sports.com/*.json` front-end page-model, **auth-free**, curl_cffi) — 35 routes: player/coach/institution/recruitment traversal, rank history, crystal-ball, draft picks; flat FK-graph payloads | sdv-internal-refs `247sports/` (`gen_site_pages_openapi.py` from real captures) |
| `on3-recruit-database.openapi.yaml` | On3 **public** Recruit Database (`api.on3.com/public/rdb/v1/*` + one `/rdb/v2`, **auth-free**, plain requests) — 82 endpoints (36 live-validated 2026-07-08): recruiting/rankings/NIL valuations/transfer-portal/draft/coaches/collectives. Sibling of the 247 RDB (On3 owns Rivals). NOT official | sdv-internal-refs `on3/` (`gen_openapi.js` from `on3_ts_api.ts` Api class + `public-contracts.ts` + live captures) |
| `espn_fantasy_v3.json` | ESPN Fantasy League Manager Platform (`/apis/v3`) — cross-sport fantasy leagues, draft, rosters, transactions (Swagger 2.0) | community / ESPN Fantasy Platform Engineering |

## Cross-sport / odds

| Spec | API | Source |
|---|---|---|
| `the_odds_api.openapi.yaml` | The Odds API (`api.the-odds-api.com`) — sports, current/historical odds, scores across bookmakers (v4) | the-odds-api/odds-api official spec |
| `cbssports-napi.openapi.yaml` | CBS Sports **NAPI** (`api.cbssports.com/napi`) — auth-free REST; registry-generated OpenAPI 3.1, 82 endpoints (leagues/seasons/teams/rosters/standings/scoring/odds). Live game scoring + recruiting are torq-only / id-gated. | sdv-internal-refs `cbs/` |

> The ESPN and Fox specs above are generated from `sdv-internal-refs` (`espn/`, `fox/`)
> and cover all sports via path parameters. The per-sport endpoint **catalogs** in
> `sdv-internal-refs/espn/catalogs/*_endpoint_catalog.md` remain the breadth reference
> for league-slug coverage across all sports.
