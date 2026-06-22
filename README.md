# radar

**Surface repo** for the investor deal radar product (Free / Pro / Max tiers).

Scaffold only (Architecture-homes phase 1, Track D). The operator creates the GitHub remote; full code move sequences with the cortex-api decomposition.

## Home

| Role | Repo |
|---|---|
| This Surface | `radar` — extension UX, billing UX, deal-radar orchestration |
| Reporting BFF (today) | `legacy-design-tools` / `cortex-api` — `/api/brokerage/v1/*` |
| Gate | `hauska-mcp-server` |
| Map layers | `hauska-map` |
| Spine | `hauska-engine` |

## Consumption model (target)

Radar consumes **reporting + map + gate** — not direct Postgres on the spine. Extension auth, entitlement, and brief/run orchestration move here; composition calls cross-project through the gate-front seam until sprint 56 completes.

## Status

- [x] Folder scaffold (`P:\radar`)
- [x] GitHub remote (operator)
- [ ] Backend extraction from cortex-api (see `docs/EXTRACTION_SCOPE.md`)
- [ ] Extension repoint to radar API host

See [`docs/EXTRACTION_SCOPE.md`](./docs/EXTRACTION_SCOPE.md) for the cortex-api decomposition inventory.
