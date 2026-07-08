# GitHub Copilot instructions — sdv-swagger

A **flat collection of OpenAPI / Swagger spec files** for the sports-data APIs used across
SportsDataverse (mostly reverse-engineered / community-sourced). No app, no build, no server
in-repo — **the specs are the deliverable**. `CLAUDE.md` has the full guide.

## Structure

Every spec lives at the repo root (no subdirs). Formats: OpenAPI 3.1 / 3.0 YAML & JSON, plus
one Swagger 2.0 (`espn_fantasy_v3.json`). Naming is mixed (`*.openapi.yaml`, `*-swagger.json`,
`*_openapi.yaml`, `*.yml`) — **`README.md`, not the filename, is the source of truth** for
what each spec describes and where it came from.

## Conventions

- **The README table is part of the deliverable.** Adding/renaming a spec means adding a row
  (filename · API host · provenance/source) in the correct provider section of `README.md`.
- Most specs are **generated in `sdv-internal-refs`** (`espn/`, `fox/`, `yahoo/`, `cbs/`,
  `247sports/`, `nfl/`, `pff/`, …) and copied here **byte-identical**. Don't hand-edit a
  mirrored spec — edit its generator in sdv-internal-refs and re-copy. Record provenance.
- ESPN/Fox specs are sport-agnostic via `{sport}`/`{league}` path params — don't fork a spec
  per sport. Prefer an official upstream spec when one exists (e.g. The Odds API v4).
- These describe third-party/undocumented APIs; auth/access vary and are noted in the README
  (e.g. PFF Premium is Clerk-cookie + `_premium_key`, paywalled; Yahoo NCP is session-gated).
- Mixed OpenAPI versions coexist — a validator run over the whole tree must handle 3.1/3.0/2.0.
- No in-repo build/serve/CI — validation is external tooling. Don't invent a build command.
- **Never add AI co-author trailers** to commits or PRs.
