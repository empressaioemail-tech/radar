# Deal-radar extraction scope — cortex-api → radar Surface

Architecture-homes phase 1 (design + scaffold only). **No code move in this wave.**

Source monolith: `legacy-design-tools` / `artifacts/api-server` — today all investor radar backend lives under `/api/brokerage/v1/*`, mounted via `brokerageBrief.ts`.

## Extraction boundary

### Moves to `radar` (Surface-owned)

| Area | Current location | Notes |
|---|---|---|
| Brief orchestration | `routes/brokerageBrief.ts` | POST/GET brief, summarize, research chat |
| Workspaces | `routes/brokerageWorkspace.ts` | Recent, reopen, attachments, share |
| Entitlement + wallet | `routes/brokerageWalletRoute.ts`, `brokerageEntitlementRoute.ts` | Free/Pro/Max gating |
| Billing | `routes/brokerageBilling.ts`, `brokerageBillingPublic.ts` | Stripe checkout, landing |
| Profile | `routes/brokerageProfile.ts` | Investor buy-box, persona |
| GTM funnel | `routes/brokerageGtm.ts` | `radar_autorun`, paywall events |
| Encumbrances (extension) | `routes/brokerageEncumbrances.ts` | Tenant-private upload path |
| Place + map data | `routes/brokeragePlace.ts`, `brokerageMapData.ts`, `brokeragePlaceHydrology.ts` | Parcel drill, motivated-seller layers |
| Auth extension wedge | `routes/auth.ts` (brokerage slice), `lib/extensionLoginPage.ts` | Deal radar signup/login |
| Admin graph | `routes/brokerageAdminGraph.ts` | Internal funnel viz |

### Stays in `cortex-api` (reporting function package)

| Area | Rationale |
|---|---|
| Plan review / findings / adjudication | Codex function, not radar |
| Engagements / submissions / reviewer queue | Architect + Codex surfaces |
| Warming / codewarm / reasoning UPSERT | Spine-adjacent; lifts per sprint 56 |
| Calibration overlay / evidence ledger | Fuel spine (cc-agent-C) |
| Site-topography / site-drainage ingest workers | Engagement-scoped plan-review context |

### Shared libs to split at move time

```
artifacts/api-server/src/lib/brokerage*.ts   → radar/src/server/lib/
lib/map-embed (radar allocation)             → radar imports from npm/workspace
lib/db brokerage_* tables                    → radar owns writes; cortex-api read-only until decomposed
```

## Route inventory (today)

Mounted from `brokerageBrief.ts`:

```
GET  /brief-coverage
GET  /brokerage/v1/coverage
POST /brokerage/v1/brief
GET  /brokerage/v1/brief/:runId
POST /brokerage/v1/brief/summarize
POST /brokerage/v1/research/chat
POST /brokerage/v1/parcel-key
+ sub-routers: gtm, profile, place, workspace, encumbrances, wallet,
  entitlement, admin/graph, place/hydrology, map-data, billing
```

## Sequencing (post phase-1)

1. Operator creates `empressaioemail-tech/radar` remote; push scaffold.
2. Extract `brokerage*` routes + libs into radar BFF (thin Express or Hono).
3. Radar BFF calls cortex-api / gate for reporting composition + map manifest.
4. Repoint Chrome extension API base URL.
5. Remove duplicated routes from cortex-api after cutover tests green.

## Cross-project seam

Gate (`hauska-mcp-server`) → radar BFF → cortex-api reporting composition. Auth: extension API key + installId today; **tenant leg (sprint 54)** replaces anonymous default tenant before owner-isolation claims.

## Related docs

- [`01_homes_and_topology.md`](../../doc_repo/_architecture_homes/01_homes_and_topology.md) — Surfaces classification
- [`04_audit_and_sequence.md`](../../doc_repo/_architecture_homes/04_audit_and_sequence.md) — Track D scaffold
