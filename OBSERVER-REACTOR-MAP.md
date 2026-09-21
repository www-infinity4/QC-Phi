# Observer / Reactor Map

_Last mapped: 2026-09-21_

## Current system

QC Phi is the network observer. It must detect broken contracts, preserve evidence, and produce a repair ticket. It must not silently edit production.

| Layer | Current owner | Responsibility |
|---|---|---|
| Observer UI | `www-infinity4/QC-Phi` | Runs checks, displays pass/fail/warn, builds `qc-phi-handoff/v2` |
| Infinity Phi | `www-infinity4/C13b0` | Search token, collections, media handoff, wallet and website-index contracts |
| Omni Phi | `www-infinity4/Omni-Phi` | Search/media collections, overview/storyboard, Code Phi handoff |
| Durable ledger | `www-infinity4/TV-Database/workers/starquest-ledger` | Star Coin balance, pending share credits, share count and history |
| Asset provenance | `www-infinity4/TV-Database/workers/infinity-assets` | Token assets, cross-token reuse, idempotent source-owner credit events |
| Search / Code Phi inspection | `www-infinity4/searxng` | Preserve `/search`; provide `/code-phi/inspect` and `/code-phi/plan` |
| Repair target | Code Phi / authorized coding agent | Verify failure, make smallest repair, rerun contracts |
| Supporting indexer | `www-infinity4/C13b0-Indexer` | Catalog markers for wallet, Star Coin, share rewards, durable ledger and history |

## Required contract chain

1. Trigger occurs.
2. A durable receipt identifies owner, site, token/search, action and idempotency key.
3. D1 ledger or destination stores the result.
4. The source and destination UI refresh.
5. QC Phi verifies the round trip.
6. A failure becomes a repair ticket.
7. Reactor verifies before editing, changes the smallest target, then reruns all previously passing checks.

## Contract inventory

| Contract | Trigger | Durable evidence | Expected result | Severity |
|---|---|---|---|---|
| Search token continuity | Successful fresh first-page search | Token ledger transaction | One token exists across overview, images, video and audio refinements | Critical |
| Media collection handoff | Collect image/video/audio | Token-scoped collection receipt | AI Overview receives every selected item without creating a new token | Critical |
| Builder output handoff | Generate Website action | Build receipt plus originating token | Builder produces the finished website package | High |\n| Web Phi publication | Publish finished website | Published-site ID, title, URL and searchable metadata | Web Phi displays and searches completed websites; deferred until builder pages are finished | Deferred |
| News Phi derivation | Collect source card | Stored keyword/entity provenance | Fresh source-backed story cards are generated; copied source card is not displayed as news | High |
| Builder Reserve feed | Create purple research direction | Indexed idea receipt | Purple idea appears with provenance and build routes | Medium |
| Star Coin share payout | Confirmed share | Idempotent ledger receipt | +0.1 progress; one Star Coin per ten confirmed shares; wallet refreshes | Critical |
| Reuse payout | Reuse a registered token asset | Infinity-assets reuse receipt | Idempotent one-Star-Coin source-owner credit event | High |
| Navigation/dependency routes | Load shared links and APIs | HTTP result plus expected marker | Required route is reachable and not 404 | High |
| Code Phi inspection | Send selected material | Inspect/plan receipt | Generated artifact reflects selected content; `/search` remains unchanged | High |
| Regression protection | Any repair/deploy | Before/after contract run IDs | Previously passing contracts still pass | Critical |

## Existing QC Phi coverage

- Application reachability
- Basic token/wallet/ledger source markers
- Share markers
- Collection markers
- QC receipt markers
- Manual failure capture for share and token credit
- JSON repair handoff

## Missing Observer work — ordered

1. Replace source-marker checks with route and round-trip checks.
2. Add one token-continuity probe covering overview → images → video → audio → overview.
3. Add collection destination probes for AI Overview, News Phi and Builder Reserve. Add Web Phi publication checks after the builder pages are finished.
4. Add StarQuest ledger read/write/read verification using uniquely tagged disposable test receipts.
5. Add share-payout idempotency verification: the same receipt must never credit twice.
6. Add infinity-assets registration/search/reuse verification.
7. Add dependency manifest checks for shared hamburger, wallet, remote and Worker routes.
8. Store scan runs and evidence durably instead of only in browser memory.
9. Emit one repair ticket per failed contract with exact repository, route, evidence and last-known-good commit.
10. Add Reactor status: queued → verified → repairing → retesting → complete/blocked.
11. Require a full regression scan before a ticket can be marked complete.
12. Alert on disconnect regressions associated with commits or deployments, including the earlier `64aa5f5` concern.

## Safety rules

- One repair job at a time.
- Verify deployed behavior before editing.
- Preserve unrelated Workers and the existing `/search` route.
- Never store GitHub, GPT or Cloudflare secrets in browser JavaScript, localStorage or public repositories.
- The iteration machine may run without the observer; Observer/Reactor improves reliability but is not a blocker.
- A Reactor never marks its own repair complete without a fresh Observer scan.
