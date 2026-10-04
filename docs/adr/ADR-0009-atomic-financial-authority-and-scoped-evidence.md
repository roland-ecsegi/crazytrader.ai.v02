# ADR-0009 — Atomic financial authority and scope-bound certification

Status: ACCEPTED design, 2026-10-04. Refines ADR-0001, ADR-0003 and ADR-0006 without granting runtime authority.

## Decision
Final authorization binds an immutable intent to risk/policy/config versions, current account and portfolio revisions, an atomic capital reservation, expiry and a single-executor fencing epoch. The financial state transaction and its outbox are committed before sending. No earlier risk approval is a reusable blank cheque.

Unknown submissions are reconciled, not retried under new IDs to escape ambiguity. Logical order actions, network attempts and revised economic intentions have different identities. Fill posting, reservation changes, derived inventory and outbox publication are one idempotent transaction.

Emergency availability has physical limits. A narrow deterministic pre-authorized cancellation/reduction policy may survive selected dependency failures, with current position evidence and durable local journal. It cannot promise flattening through venue/network failure or authorize ambiguous holdings after corruption. If both trustworthy state and durable journaling are unavailable, no new order is sent; alert owner for exchange-side action and reconcile later.

Certification applies to an explicit build, artifacts, config, data, adapter, venue/account environment and supported order capability set. Engineering certification and strategy economic eligibility are separately recorded. General LIVE still requires L6 and owner activation; L5 remains the sole bounded pre-L6 real-capital exception.

## Consequences
Concurrency, restart, replay and invalidation tests become mandatory. Emergency logic is a constrained second policy profile of the same controlled sender, not an agent bypass or second executor. Strategy rejection does not justify weakening tests or forcing trades.
