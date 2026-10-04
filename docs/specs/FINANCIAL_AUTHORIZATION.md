# Atomic financial authorization

Status: DEFINED contract, executable schemas/transactions/tests MISSING. Authoritative for concurrency, reservations and final submission authorization. See ADR-0009.

## Separation of identities

| Record | Meaning | Replay/retry rule |
|---|---|---|
| TradeIntent | Immutable economic proposal and desired bounded action | Revised/expired/rejected economic decision needs a new intent linked by supersedes_id |
| ExecutionRequest | Authorized plan and reservation for an intent | Duplicate delivery resolves to the existing request, never another allocation |
| OrderAction | One logical submit, cancel or amendment action | Stable action_id and venue-compatible client_order_id; never replace merely because response was lost |
| NetworkAttempt | One transport attempt for an action | New attempt_id, same action identity; no blind resubmission of unknown submit |
| VenueOrder / Fill | Externally observed facts | Unique by exchange account + venue identifier; fills additionally scoped by symbol when venue IDs are symbol-local |

Initial Spot scope excludes cancel/replace and amendments until separately certified. Cancellation has its own action_id and references the target order. Agent task retries cannot mint financial authority.

## Durable records

CapitalReservation includes reservation_id, account_id, portfolio_id, intent_id, execution_request_id, asset amounts, maximum fee/slippage provision, held/consumed/released amounts, lifecycle, expiry, account_revision and ledger references. BUY reserves quote and possible fee asset; SELL reserves owned attributable base. Reservation is a hold, not P&L or a second deduction from equity.

SubmissionAuthorization includes authorization_id, execution_request_id, intent_hash, risk_decision_id, policy_decision_id, certificate_scope_hash, instrument_metadata_version, cost_model_version, portfolio/account/risk/config revisions, reservation_id, authorized bounds, expires_at, executor_epoch and consumption state. RiskDecision records the independently recomputed risk effect; the intent's label is untrusted.

## Final check and commit

1. Validate schema, environment, actor, strategy/model scope, lifecycle and expiry. Read the latest certified market snapshot and instrument filters.
2. Within a serialized account/portfolio critical section, lock affected capital/position rows in a fixed order. Include active holds, open/unknown orders, externally observed liabilities and both modes. Evaluate current deterministic limits and OPA using this exact revision.
3. Insert the hold and single-use authorization; advance state revision and append audit/outbox atomically. If external policy evaluation occurs outside the transaction, use compare-and-swap on all relevant revisions; any mismatch reruns evaluation. A cached policy allow is usable only for the same policy/input hash and unexpired scope.
4. The sole sender claims the request under the current fenced epoch. Immediately before transmission, verify expiry, global/new-risk latch, current price/filter bounds and that relevant state has not invalidated approval. Unknown or changed state requires re-evaluation and re-binding, never a best-effort send.
5. Persist SUBMITTING and the action/client ID before I/O. Consume authorization exactly once. A crash here is ambiguous until venue reconciliation. A durable outbox is not permission to replay a send after restart.

There is no atomic transaction spanning PostgreSQL and Binance. The protocol guarantees local serialization and detection of ambiguity, not distributed exactly-once execution. The sender checks the local stop latch immediately before I/O; an already in-flight order can still reach the venue after Kill. Reconcile/cancel it and report this race explicitly.

## Holds and release

A hold is released only for confirmed rejection, authoritative terminal unfilled remainder, or proven unsent expiration. UNKNOWN keeps the worst-case hold. Partial fills atomically consume held amounts into inventory/fees and retain the remainder. Expiry of a local timer alone does not release an exchange order's funds. Actual fills can differ from estimates; post actual facts, raise any deficit incident and block new risk rather than reject accounting truth.

Reservations serialize on the shared exchange account, not only per-mode budgets. Concurrent Math and Strategy intents cannot each spend the same quote balance. Conservative assumptions cover unknown order remaining quantity and fees. Local ledger holds and exchange locked balances must be reconciled without double-counting.

## Fencing and stale worker prevention

Use an exclusive local execution supervisor/OS lock plus database lease/epoch. Every send checks an active epoch; restored backups start disabled. On the single host, prove the previous sender process is stopped before replacement. During DB outage only the already-exclusive supervised sender may use the emergency policy/journal; no replacement worker acquires authority from an expired DB lease while DB state is unavailable. Binance does not enforce our database fencing token: two independent machines with the same key are NOT safely fenced by this design. Failover to another host requires fencing the original host or rotating/revoking its trading credential before activation.

## Required invariants

No negative available allocation caused by admitted concurrent orders; no spend beyond account/portfolio cap including holds; one send authority per action; immutable decision chain; no reuse after expiry/invalidation; no new risk from replay; no double posting; risk direction calculated from worst-case post-action exposure. Tests interleave competing modes, fills, policy changes, Kill, restart, lease loss and late venue events at every commit/I/O boundary.
