# ADR-0007 — Modular local deployment and one financial state owner

Status: ACCEPTED design, 2026-10-04. Implementation pending. Refines ARCHITECTURE_V1 and the former mandatory infrastructure list.

## Context
The original service list and PostgreSQL/ClickHouse/NATS/OpenBao/MLflow/object-store list mixed responsibilities with mandatory deployment units. One local operator does not justify a separate process for every module. Duplicate engine and application order-state authorities would be unsafe.

## Decision
Use a modular deterministic trading core with PostgreSQL as operational authority; an isolated research worker; isolated agent/Claude process boundary; custom web/control interface; OPA authorization and owner-local secret provision. Modules may share a process/transaction where appropriate. Exchange secrets are restricted to the execution boundary; no research or AI code runs inside that privileged process.

Use PostgreSQL transactions, outbox/inbox and bounded workers first. Use local versioned Parquet artifacts and DuckDB for analytical reads where the workload fits. A local directory can implement the artifact-store interface. Introduce NATS, ClickHouse, MLflow server or object-storage server only after measured need and an ADR. OPA remains independent authority after Hard Risk; its exact local packaging is an integration decision.

Evaluate NautilusTrader before custom execution/simulation. Select exactly one authoritative order/fill state machine, exchange sender and recovery owner. If adopted, wrap that owner and prove adapter semantics; never let a parallel custom engine also send or mutate authoritative order state. The application ledger remains accounting authority with explicit mapping from engine facts.

## Consequences
Fewer failure surfaces and lower host burden. Module/API boundaries preserve later extraction. Database durability, backpressure and credential process isolation are still mandatory. No claim that a single host provides HA. Engine/license/performance decisions require spikes, not star counts.
