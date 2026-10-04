# Architecture V1 — Enterprise Local

Revision: 2026-10-04 documentation remediation. Status: specification baseline; executable product MISSING, L0. PRODUCT_REQUIREMENTS is the product acceptance contract. Accepted ADRs and AGENTS.md invariants govern conflicts.

## Target and invariant boundary

A complete private single-owner Binance Spot application with two independently controlled/attributed modes, eleven permanent event-driven agents including Loki, supported owner-local Claude subscription routing, custom UI, rigorous research and deterministic financial safety. SaaS is not needed for the owner to trade after validation. No guaranteed return or forced trades.

Math computes live quantitative evidence; versioned strategies generate structured proposals. Agents may research/explain/propose. Schema/certification, Hard Risk, OPA, atomic reservations, sole execution authority, reconciliation and balanced ledger enforce financial consequences. Neither UI, model output, owner manual action nor research bypasses that boundary.

## Four logical planes

| Plane | Responsibilities | Authority |
|---|---|---|
| Trading | Data/features, Math/Strategy, portfolio, risk, policy, execution, reconciliation, ledger | Deterministic, scoped financial authority |
| Agent control | Eleven identities, durable tasks, skills, memory, AI routing, Loki | Bounded advisory/research/user-workflow proposals |
| Research | Versioned datasets, candidate methods, all experiments, validation, learning | No live credentials or self-promotion |
| Platform | Authenticated UI/API, storage/audit, configuration, local secrets, health, backup/recovery | Owner workflows and enforced infrastructure controls |

## Data and control flow

Market ingestion validates timestamps/quality -> shared pure features -> immutable model/strategy -> TradeIntent. Account/portfolio serialization evaluates current risk and OPA, reserves capital and persists a single-use state-bound authorization/outbox. The fenced sole sender rechecks current bounds and persists SUBMITTING before exchange I/O. Venue observations drive idempotent fill posting, order state, holds and reconciliation. Ambiguous submissions remain UNKNOWN until resolved; event replay never resends financial actions.

In parallel, deterministic monitors qualify domain events. The task scheduler deduplicates/coalesces and admits bounded agent work; AI Gateway invokes the approved CLI only when needed. Agent results become evidence, hypotheses or typed owner-command proposals. Loki retrieves permitted actual state and versioned knowledge. None of this is on the critical path for protection, reconciliation or Kill.

## Minimal local topology

Preserve logical ownership in SERVICE_CONTRACTS but deploy a modular financial core, PostgreSQL, local OPA evaluation, UI/API, isolated research worker and isolated agent/Claude processes. Keep exchange credentials in the execution process boundary only. Safe modules may share transactions/processes; research/AI cannot run privileged code alongside execution.

PostgreSQL owns operational state, outbox/inbox and task leases. Versioned local Parquet/artifact storage plus DuckDB handles initial research. NATS, ClickHouse, MLflow server and object-store server require measured justification. A vetted OS secret facility may satisfy local secret isolation; OpenBao is optional with tested recovery. Instrument first, then choose monitoring packaging that fits the host. No Kubernetes/Kafka/service mesh/multi-region baseline.

## Execution foundation

NautilusTrader remains the primary reuse candidate, subject to early compatibility/license spike. Choose one order-state engine/sender and map its facts to the independent accounting ledger. Official Binance SDK is a reference/limited adapter candidate, never an alternate agent-accessible sender. Reject integration that bypasses outer authority or creates conflicting state ownership.

## Quantitative and adaptation architecture

Original modes were not complete algorithms. QUANTITATIVE_METHODS defines MATH-RIDGE-01 and STRATEGY-DONCHIAN-01 as unvalidated baselines. Features, labels, decision/sizing/exit rules, costs and trial protocol are explicit. Same logical pipeline runs through backtest/paper/live with adapter/time/cost differences exposed. Learning observes abstentions/rejections as well as executed trades; new artifacts require new evidence. Initial live artifacts do not self-modify.

## Degraded and operational behavior

Block new risk on unknown safety state. Preserve only verified pre-authorized cancellation/reduction capabilities under the RISK_SECURITY dependency matrix. Exchange/network/power failure can make flattening impossible; never promise otherwise. Single-host operation requires tested recovery, single sender, measured RPO/RTO and honest residual availability limits.

## Certification and future seams

L0–L6 is scoped to build/artifacts/config/venue/account/order capabilities and supporting evidence. Candidate economic eligibility and complete product acceptance are separate. L5 bounded owner-local canary is the sole pre-L6 real-risk exception; L6 plus explicit owner activation grants only its certified scope.

Cheap seams now: owner/account IDs, provider/venue interfaces, immutable artifacts, versioned contracts, module ownership and usage metadata. Future SaaS adds real tenancy/identity/billing/compliance/global operations only when justified; these absences do not reduce current local completeness.
