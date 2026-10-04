# Execution and reconciliation contract

Status: DEFINED state/authority design; venue adapter and tests MISSING. One selected engine owns authoritative order state. The application must not run a competing sender/state machine beside NautilusTrader if that engine is selected.

## Separate control and venue state

Intent/authorization states: PROPOSED -> VALIDATING -> AUTHORIZED or DENIED/EXPIRED; authorization can become INVALIDATED before send. They are not exchange order states.

Order lifecycle and legal observations:

| Current state | Input | Result and guard |
|---|---|---|
| PREPARED | Current single-use authority and persisted send attempt | SUBMITTING before network I/O |
| SUBMITTING | Venue accepted acknowledgment | OPEN, unless fills already establish PARTIALLY_FILLED/FILLED |
| SUBMITTING/OPEN | Authoritative execution report before or after ack | PARTIALLY_FILLED or FILLED; deduplicate fill facts first |
| SUBMITTING | Timeout/disconnect/ambiguous venue result | UNKNOWN; hold capital and block related duplicate action |
| OPEN/PARTIALLY_FILLED | Authorized cancel request | CANCEL_PENDING; not terminal; fills can still arrive |
| CANCEL_PENDING | Fill | Update inventory/hold; FILLED if complete, otherwise remain pending |
| CANCEL_PENDING/UNKNOWN | Authoritative cancel with cumulative quantity | CANCELLED with already-filled quantity retained |
| Any unresolved | Authoritative rejection/expiry | REJECTED/EXPIRED only when no acceptance ambiguity; retain known fills |
| UNKNOWN | Order/trade history reconciliation | Restore venue-derived state or RECOVERY_REQUIRED; no blind send |
| Terminal | Duplicate/late fact | Idempotent no-op or add previously unseen genuine fill; never regress cumulative quantity |

Persist cumulative filled quantity monotonically and enforce no overfill except reported venue anomaly, which is recorded and escalated. An ack cannot regress FILLED to OPEN. Cancellation failure is not cancellation success. Canceled partially-filled orders retain inventory. Derived order status and fill completeness can differ until backfill resolves gaps. Terminal order status alone is insufficient to declare the ledger complete.

## Submission identity and absence proof

Use stable client IDs meeting the pinned Binance adapter's allowed length/character constraints, backed by a local uniqueness registry. Do not assume the venue prevents duplicate economic action across all terminal states. Record request fingerprint, attempt timestamps and response/error payload classification without secrets.

On ambiguous submit: stop resends; query order by client/venue ID, user stream, open/recent order and trade history, and balances over a documented bounded consistency window. One NOT_FOUND is insufficient proof. Account for event delay, symbol-scoped history and request window. If the selected API cannot conclusively establish absence, retain RECOVERY_REQUIRED for explicit investigation; timeout is not permission to retry. Only a proven unsent or definitively rejected action may be retried with renewed valid authority according to the adapter's certified identity rules. Any new client ID remains linked to the same logical action and is prohibited while earlier acceptance is possible.

## Venue capabilities and realistic orders

Initial implementation scope: Spot BUY/SELL, no borrowing or implicit shorting; limit GTC and bounded marketable limit IOC are the preferred first certified order types. Market orders and native stops/OCO require separate capability/precision/failure tests before enabling; do not imply they already protect positions. IOC remainder can cancel without filling. Marketable limits bound price but cannot guarantee completion. Local software stops require connectivity and running software; stops can gap, reject or slip and are not loss guarantees.

Pin and version symbol status, tick size, step size, quantity/notional filters, percent-price constraints and allowed order/time-in-force combinations. Query current filters; round quantities conservatively; revalidate price/quantity after rounding. A risk reduction below minimum notional may leave unsellable dust; record it, never buy more automatically just to make it sellable. Handle commission in base, quote or third asset, fee-tier changes and insufficient fee balances.

## Connectivity and scheduling

Maintain WebSocket heartbeat/gap/reconnect logic and REST backfill; do not assume a reconnected socket recovers missed facts. Rate-limit centrally by actual venue weights and order counters. Reserve measured capacity for cancellations/reconciliation ahead of research/ordinary requests. Respect 429/backoff and 418 bans; retries have jitter, bounds and incident escalation. API timestamp windows require monitored host/server clock offset; unsafe skew blocks new risk. Distinguish public market-data, signed account and trading connectivity.

Timeout may mean matching engine success. Explicit venue rejection, transient server failure and unknown execution status are separate error classes. Exchange downtime can prevent all automated actions; alert owner, retain authoritative state and reconcile before resuming.

## Reconciliation and shared account

At startup and continuously compare account balances (free/locked), open/recent orders, fills, holds and portfolio-attributed inventory. Use durable watermarks with overlap and deduplication, not a timestamp-exclusive scan that misses equal-time events. External/manual trades, deposits, withdrawals initiated elsewhere, fee-asset movements and transfer deltas go to an unassigned/suspense account until resolved; they never become arbitrary strategy profit. Freeze affected new risk on material mismatch.

Prefer a dedicated exchange account for the application where available. Manual trading on the same account must be detected even though it is discouraged. Inventory for Math and Strategy is internally attributed; a SELL can spend only its assigned available base unless an explicit ledger transfer is authorized. Never net away attribution without a transfer record.

## Restart and shutdown

Safe startup: acquire exclusive sender authority -> load immutable config/certification -> verify journal/backup epoch -> replay internal state without sends -> query unresolved venue facts -> reconcile balances/fills/holds -> health gate -> explicitly permitted operation. A restore never automatically reactivates live.

Safe shutdown: latch new risk off, stop producers, reconcile pending sends, apply configured cancel policy, persist watermarks/holds, verify no orphan sender; report remaining inventory/orders. Crashes at every step require deterministic recovery. See LOCAL_DEPLOYMENT for emergency, RPO/RTO and host fencing.

## Primary venue references

Verify current behavior against the selected version of the [Binance Spot API](https://developers.binance.com/docs/binance-spot-api-docs/rest-api/general-api-information), [filters](https://developers.binance.com/docs/binance-spot-api-docs/filters), and [user-data stream](https://developers.binance.com/docs/binance-spot-api-docs/user-data-stream). Testnet is an integration aid, not evidence of real liquidity, fees or production availability.
