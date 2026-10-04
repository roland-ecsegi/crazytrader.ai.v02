# Domain model and record ownership

Status: DEFINED v1 design; executable schemas/migrations MISSING. This is a field/ownership map; specialized specs govern semantics. IDs are globally unique internally, UTC timestamps have declared precision, versions/hashes are immutable, and raw credentials never appear in domain records.

## Common rules

owner_id/tenant_id are cheap future ownership seams, not implemented multi-tenant isolation. Monetary amounts, quantities, prices, fees and financial limits use canonical Decimal strings/units. Statistical features/estimators may use finite float64 internally with validated conversion at the financial boundary. Reject NaN/Inf and ambiguous units. Occurred/event time, received/available time and recorded time are distinct.

## Product and configuration records

| Entity | Required core fields / ownership |
|---|---|
| Tenant / Owner | tenant_id, owner_id, status, created_at; exactly one local owner; local auth still required |
| ExchangeAccount | account_id, venue, environment, credential_ref, permission_snapshot, IP-restriction state, reconciliation status; secret lives only in local facility |
| InstrumentVersion | instrument_id, symbol, base/quote, market_type, status, tick/step/min/max/filter sets, allowed capabilities, effective_at, metadata_hash |
| Portfolio | portfolio_id, account_id, mode MATH/STRATEGY/RESERVE/SUSPENSE, reporting_asset, status, profile/version, revision |
| CapitalAllocation | allocation_id, source/target portfolio, asset/amount, effective_at, reason, approved_by, cap/config reference, ledger_transaction_id |
| RiskProfileVersion | id, LOW/MEDIUM/HIGH, explicit numeric parameters/units, absolute-limit refs, artifact hash, lifecycle |
| SystemConfigVersion | immutable config hash, scope/environment, effective_at, actor, approval/audit refs; missing live limits block activation |

## Agent and research records

| Entity | Required core fields |
|---|---|
| Agent | agent_id, stable_name, role_version, status, provider_config_id, permission_profile_id, skill allowlist, memory_policy_id, created/updated_at |
| AgentSkillVersion | skill/version, input/output schemas, implementation hash, allowed agents/permissions, side-effect class, resource limits, idempotency and evaluation refs |
| AgentTask / SkillInvocation | Defined in AGENT_RUNTIME; task/lease/checkpoint/attempt identity and durable effect receipt |
| AgentMemoryRecord | memory_id, agent_id, type, content_ref/hash, source_refs, source version/time, validation status, quality, retention class, supersedes_id |
| AIProviderConfig | id, transport/auth class, model allowlist, timeout/retry/fallback/budget policy, local auth reference/status; no raw token |
| Strategy / Model | stable id/family and immutable version id, code/artifact/config/data/feature/label hashes, lifecycle, validation refs |
| Experiment | id, hypothesis, registered protocol, dataset/splits, feature/model/profile/cost versions, parameters, all trial IDs, result, uncertainty, reproducibility hash |
| DecisionObservation | id, candidate/agent/mode, decision_time, available input refs, signal/abstain/reject/accept status, reason, horizon, intended size and policy scope |
| OutcomeObservation | observation_id, matured_at, actual/counterfactual flag, realized data/fill refs, uncertainty/limitations; never mix hypothetical with ledger P&L |
| ExperienceRecord | id, agent/mode/candidate versions, decision/outcome refs, market/regime/risk/context, execution quality, anomaly refs, created_at |

## Financial and operational records

| Entity | Required core fields / canonical spec |
|---|---|
| TradeIntent | Immutable typed proposal in TRADE_INTENT; no authority by itself |
| RiskDecision | id, intent hash, independently recomputed risk_effect, allow/deny, reason codes, exposure/loss/data/reconciliation evidence, rules/config versions, state revisions, expiry |
| PolicyDecision | id, actor/action/resource/environment, risk decision hash, certificate scope, input/bundle hashes, allow/deny/reasons, expiry |
| CapitalReservation / SubmissionAuthorization | State-bound atomic hold and single-use approval in FINANCIAL_AUTHORIZATION |
| ExecutionRequest / OrderAction / NetworkAttempt | Distinct plan/logical action/transport identities in FINANCIAL_AUTHORIZATION; one sender and no ambiguous retry |
| Order | internal/account/venue/client IDs, originating intent/request, symbol/side/type/TIF, requested/fill quantity, price, lifecycle, venue/fill watermarks, revision, timestamps |
| Fill | account+venue+symbol-scoped fill identity, order, quantity/price, fee amount/asset, event/receive/recorded timestamps and raw evidence hash |
| Position | Attributed Spot inventory per portfolio/instrument, available/held quantity, cost basis, valuation/P&L refs and revision; derived from journal/facts |
| LedgerTransaction / Posting | Per-asset balanced immutable postings and compensating corrections in DATA_AND_LEDGER |
| ReconciliationRun | scope, watermarks/time window, queries/evidence, discrepancies, unresolved items, result and next action |
| Incident | id, severity/category, affected scope, trigger, state, observed_at, owner/action refs and resolution evidence |
| AuditEvent | id, actor/action/resource, trace, source and evidence hash, occurred/recorded time, redacted payload |
| CertificationRecord | Scoped engineering level, economic eligibility, product acceptance, evidence/config hashes, validity/invalidation in TESTING_AND_CERTIFICATION |
| OwnerActivation | authenticated actor, exact scope/caps/config/certificate refs, environment, created/expiry, revocation/latch refs; separate from certification |
| ProgramCheckpoint | development branch/containing commit, current task, next action, blocker/usage state and verification refs; never a trading authority |

Materialized views are rebuildable; immutable facts and journal are not rewritten to match a desired result. Implement schema compatibility and migration/replay tests before external consumers depend on v1.
