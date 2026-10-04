# Operational data and balanced ledger

Status: DEFINED contract; implementation MISSING. MARKET_DATA governs point-in-time market datasets. FINANCIAL_AUTHORIZATION governs reservations.

## Storage authority

PostgreSQL owns operational records, tasks, immutable financial facts, append-only journal, holds, risk/policy decisions, certification, audit and outbox/inbox. A single database transaction is the default consistency boundary. Local versioned Parquet/artifact files plus DuckDB support research; artifacts have hashes/manifests and atomic publish. Local files are not a replacement for the financial database.

NATS JetStream, ClickHouse, MLflow server and object-store server are optional measured expansions, not bootstrap dependencies. If added, they cannot become a second ledger authority. Replay transport never implies authority to resend financial actions.

## Balanced multi-asset journal

Every transaction has immutable transaction_id, operation type, source fact/dedup key, account/portfolio ownership, occurred_at/recorded_at, valuation references and zero or more linked corrections. Postings have asset, signed exact Decimal quantity, account and debit/credit convention. Require at least two postings and sum(amount) = 0 separately for each asset. Never balance 1 BTC against 60,000 USDT as if they were the same unit.

Use explicit venue clearing, owner capital, portfolio cash/inventory, fee expense, transfer and suspense accounts. Acquisition example, ignoring commission: portfolio inventory +0.001 BTC / venue clearing -0.001 BTC; portfolio cash -60 USDT / venue clearing +60 USDT. Commission of 0.06 USDT: portfolio cash -0.06 / fee expense +0.06 USDT. Actual posting template, counterparty semantics and valuation/cost-basis policy must be encoded and verified in fixtures before implementation acceptance.

A fee deducted in acquired base reduces net credited base through separate balanced fee postings; a BNB fee moves BNB and uses a timestamped valuation for reporting. Never silently charge fee to quote and again to base. Maintain reporting-currency valuation/cost-basis entries separately from conserved native asset units. Fix and version the initial accounting cost-basis policy (weighted average proposed); do not change it to improve measured returns.

Reservations move available to held subaccounts or an equivalent transactionally coupled hold table. Pick one representation and prove no double subtraction. Capital allocation/transfer reassigns ownership, not profit. Releasing a hold is not revenue. Unrealized mark-to-market is a derived valuation view; it must not fabricate physical asset movements.

## Atomic processing and idempotency

A fill-processing transaction performs: insert unique inbox/source key -> record fill -> balanced postings -> consume/release holds -> update attributed inventory/cost basis -> update order cumulative state -> append audit/outbox -> commit. Unique keys include exchange account, venue, symbol when needed, and venue fill ID. On duplicate, return the existing result without posting again. Crashes before commit have no partial financial effect; after commit, outbox republishes safely.

Consumers record inbox completion and their mutation atomically. Publisher can deliver more than once; end-to-end exactly-once transport is not claimed. Aggregate revisions provide optimistic checks and ordering; global wall-clock ordering is not assumed. Out-of-order fill facts are accepted idempotently and materialized views are recomputed consistently.

## Attribution and external truth

Shared exchange assets are partitioned internally into MATH, STRATEGY, RESERVE and SUSPENSE ownership. Sum of attributable inventories/holds plus explicitly unresolved external adjustments must reconcile with venue balances after fees and pending movements. A strategy cannot sell another mode's inventory without an authorized balanced transfer.

Binance is authoritative for external order/fill/balance facts; the journal records their provenance and internal ownership. Discrepancies are investigated, not overwritten to make balances match. External orders, manual trades, deposits/withdrawals and dust remain explicit. Compensating corrections reference their reason, original transaction and approval; history is never edited.

## Integrity and retention

Record database migrations, contract/config versions and artifact hashes. Test balance invariants, unique economic effects, replay reconstruction, exact asset precision, fee-currency combinations, partial/cancel races and flow-adjusted P&L. Hash manifests detect artifact corruption; backups and restore drills protect journal durability. Tamper-evident chains can detect edits only relative to a trusted checkpoint, not make a fully compromised host incorruptible.

Retention policy distinguishes immutable financial/audit evidence from reconstructable market caches and bounded agent transcripts. Never delete active certification evidence or transaction history to satisfy a generic log retention job. Document legal/tax retention requirements for the owner's jurisdiction before production; do not invent a universal period.
