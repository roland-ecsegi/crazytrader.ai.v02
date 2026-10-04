# Logical module contracts and authority

Status: DEFINED ownership, not a mandate for separate microservices. ADR-0007 governs local process boundaries. Existing service directory names may remain ownership namespaces; deploy fewer processes where safe.

| Module | Owns / input and output | Forbidden boundary crossing |
|---|---|---|
| control-api | Authenticated typed owner commands, config/mode/agent/certification views, activation and Kill workflow | Raw orders, secrets in frontend, authoritative risk math |
| agent-runtime | Agent identity, durable task/DAG/leases, tool permission checks, memory and evaluation metadata | Exchange credentials, direct financial authority, self-granted permissions |
| ai-gateway | Controlled CLI/provider invocation, validated response envelope, quotas/timeouts/cost-route visibility | Financial safety dependency or subscription-token proxy |
| market-data | Raw/normalized feed, availability timestamps, gap/stale state, immutable manifests | Inventing missing data or certifying its own signal quality |
| math-engine | Versioned deterministic features/model prediction, cost-aware edge, candidate size and TradeIntent evidence | LLM-derived live edge or bypassing independent risk |
| strategy-engine | Immutable strategies/profiles, deterministic regime/entry/exit predicates and TradeIntents | Free-form agent live rules or unvalidated artifact promotion |
| portfolio-engine | Account/mode allocation proposals, ownership, exposure and flow-adjusted performance | Unilateral cap growth or ignoring shared-account holds |
| risk-engine | Current-state direction/limit evaluation, loss latches, deterministic emergency evaluator | Trusting proposed risk label or AI approval |
| policy-engine | OPA actor/action/environment/certification/risk/config authorization, versioned policy evidence | Granting permission contrary to Hard Risk |
| execution | Sole authoritative selected order engine and sender, client/action IDs, state transitions, filters and rate scheduling | Unreserved/unscoped send, replay send, parallel custom engine authority |
| reconciliation | Query/compare venue facts, unresolved actions, balances/fills/holds and safe recovery findings | Erasing mismatches or blindly retrying UNKNOWN |
| ledger | Balanced journal/holds/attribution, atomic fill processing, reconstructable views | In-place historical correction or floating-point money |
| research | Isolated experiments, backtests, dataset/features/model lineage, all trial registry | Live credentials or writes to deployed policy/artifacts |
| audit | Durable redacted action/decision/event references and integrity/retention | Treating model narrative as evidence of venue execution |
| notification | Deterministic incident delivery, dedup/escalation and delivery health | Requiring Claude for a critical alert |

## Financial transaction API

Implement internal typed operations for propose_intent, evaluate_and_reserve, claim_authorized_action, record_venue_fact, post_fill_and_update_state, reconcile_scope and request_protective_action. FINANCIAL_AUTHORIZATION, EXECUTION_AND_RECONCILIATION and DATA_AND_LEDGER define their transactional/idempotency semantics. Do not implement these as independent eventually consistent microservices before proving the atomic invariants.

The selected execution engine may own its internal event loop/state; document mapping/transaction boundaries to the application ledger and reconciliation owner in its spike. Reject integration if it needs two contradictory authoritative order caches or cannot recover fills safely.

## Error and resource contract

Typed result distinguishes rejected policy/schema, transient retryable failure, ambiguous external side effect, unavailable dependency, stale state, insufficient evidence and permanent failure. Never flatten these to “failed, retry”. Every operation has deadline, bounded payload, trace ID and permission scope. Critical financial handlers outrank agent/research jobs; backpressure blocks new risk safely if durable state cannot progress.

Version changes carry compatibility/migration tests. Config and dependency changes invalidate only the relevant certification scope after impact review, not by silently trusting old evidence.
