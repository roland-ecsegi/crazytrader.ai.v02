# Verification, validation and scoped certification

Status: DEFINED acceptance design. Current system remains L0; no implementation/market/provider gate is passed by writing this file.

## Evidence and scope

CertificationRecord binds build/dependency hashes, schema/config/policy versions, strategy/model/profile hashes, data/feature/cost versions, exchange adapter/version, venue, account/environment, instrument universe, supported order types/time-in-force, capital/risk caps, hardware/OS profile and evidence manifest. Record issuer/reviewer, achieved_at, expiry/review conditions, invalidation reasons and owner activation separately.

A changed risk rule, execution adapter, schema, model, feature/cost method, venue filter behavior, account, order capability or material deployment setting triggers impact analysis and affected tests/stages. It does not inherit unlimited authority from an old “L6” boolean. Evidence from one profile/symbol/account is not automatically transferable.

Engineering readiness, candidate economic eligibility and complete product acceptance are separate fields. A safe engine with rejected strategies cannot be presented as a profitable/full-live product. Do not force live trades to obtain an evidence count when no eligible signal exists; use nonfinancial fixtures and wait/record insufficiency for market-dependent evidence.

## Mandatory test families

- Mathematical independent fixtures: formulas, units, scaling convention, labels, warmup, delay, sizing and adverse rounding.
- Unit/contract/property tests: schema rejection, state transition guards, no negative admitted available capital, per-asset balanced postings, monotonic fill quantity, loss-latch persistence, risk/profile monotonicity and scope invalidation.
- Transaction/concurrency tests: simultaneous modes, duplicate messages, outbox/inbox crash points, late fills, cancellation races, fee assets, account revisions, expired authorities and stale fenced workers.
- Venue simulation: reject/partial/late ack, UNKNOWN status, delayed history, insufficient funds, stale filters, dust, rate limits, clock skew and WebSocket backfill.
- Security: owner auth/CSRF/origin, secret redaction, agent/research privilege denial, malicious tools/docs, immutable deployment and dependency supply chain.
- Operations: provider/OPA/risk/DB/secret/venue outages, disk full/corruption, power-loss restart, backup restore, duplicate sender prevention, alert failure and safe shutdown.
- Agent/UI: real skills and durable tasks, Loki grounded answers, quota/cooldown behavior, live-activation refusal, accessible Kill and honest stale/unknown states.

No “100% test coverage” substitutes for invariants and failure scenarios. Use fault injection at each durable transition/I/O boundary and persist failing seeds/traces. An independent adversarial review pass is required for risk, execution, reconciliation and live gates; it may be an independent pass of the same development tool, with assumptions and evidence recorded.

## Ladder with measurable exit criteria

| Stage | Required evidence to pass | Prohibited claim |
|---|---|---|
| L0 DEVELOPMENT | Docs, implementation tasks, fixtures and known gaps are truthful | No simulation/paper/live readiness |
| L1 BACKTEST READY | Immutable dataset/config/artifacts; formula fixtures pass; deterministic rerun matches declared tolerances; no look-ahead split violations; all trials/costs/benchmarks recorded; candidate economic result separately reported | Backtest-ready does not mean candidate profitable or execution-realistic |
| L2 SIMULATION READY | Selected engine integrated; event simulator exercises all required order/risk/ledger failure cases; zero duplicate economic effects, unbalanced journal transactions or unauthorized actions; captured-event feature/signal parity passes | Simulated fills are not venue fills |
| L3 PAPER READY | Real-time data plus simulated execution uses same risk/ledger code; minimum 14 consecutive observed calendar days on target host; at least one planned restart/provider outage/data-gap drill; zero unresolved critical safety defects; no unexplained balance delta; operational SLOs measured | 14 days alone cannot establish statistical edge |
| L4 SHADOW READY | Minimum 14 observed calendar days, overlapping paper allowed if labeled; live account read-only reconciliation where permitted; predicted executable quantities/costs compared to observed quotes/depth with timestamped uncertainty; no sends; no unexplained account mismatch | Observation is not proof an order would fill |
| L5 CANARY AUTHORIZED | L1–L4 engineering evidence; candidate eligible under preregistered economic rule; tested emergency/recovery/secret controls; explicit owner local activation; withdrawal disabled; separate small owner-set cap, order scope, loss/stop criteria and rollback | L5 is not general LIVE permission |
| L6 LIVE CERTIFIED | Actual bounded canary evidence; predeclared observation and effective-sample criteria satisfied; zero unresolved BLOCKER/CRITICAL; supported order paths/reconciliation/cost deviations reviewed; restore and kill drills pass; owner explicit activation | No blanket strategy/account/profit certification |

The 14-day paper/shadow minima are initial operational observation floors, not scientific sample-size guarantees. Longer periods are required when workload/regime/case coverage is insufficient. Canary duration/trade-count/precision thresholds must be registered before activation based on the strategy's frequency and risk; absence of eligible trades leaves the gate pending. Do not invent canary capital amounts or relaxed thresholds after seeing results.

## Quantitative gates

Before first research run, freeze an experiment manifest and rejection criteria from QUANTITATIVE_METHODS. Verify future-label separation, training-only transformations, held-out model selection, dependence-aware uncertainty, multiple-testing accounting, cost stress and benchmark comparability. Economic eligibility requires the prespecified minimum net effect and uncertainty criterion, relevant risk bounds and robust neighboring-parameter/regime results. INSUFFICIENT_EVIDENCE is not PASS. If the initial candidates fail, retain their results and start a new preregistered hypothesis; do not optimize against the same holdout secretly.

## Operational targets to measure

Initial local engineering targets: Kill new-risk latch acknowledged within 1 second p99 under declared peak load; detection/alert of stale data or critical mismatch within the configured threshold plus 5 seconds; no new order when freshness/clock bounds fail; zero journal transaction loss on tested ordinary process crash with durable DB commits; backup disaster RPO <= 15 minutes and restore-to-reconciled-safe-state RTO <= 30 minutes under the documented test dataset/hardware. These are acceptance targets, not achieved SLAs or guaranteed venue liquidation times. L5 requires a justified tested target for the actual installation if workload makes these targets inappropriate.

Measure p50/p95/p99 end-to-end feature/decision/authorization/submit/ack latency, outbox lag, market gaps, unknown-order duration, reconciliation age, disk pressure, CPU/RAM and alert delivery. Register load assumptions and threshold config before the run. API rate priority and reserve must be demonstrated, not described as “fast”.

## Mandatory ambiguity, outage and restore cases

Timeout after accepted order; fill before ack; cancel/fill race; duplicate terminal/fill events; DB commit before send crash; send before response crash; uncertain NOT_FOUND; fee in third asset; external manual trade; disk full during emergency journal; OPA/risk unavailable; secret-store unavailable with/without cached key; clock skew; host restore with old sender still alive; stale certification; Claude unavailable; quota exhausted; permission revoked mid-task. Expected behavior must match FINANCIAL_AUTHORIZATION and the RISK_SECURITY failure matrix.

## Evidence log and honest completion

Each run records exact command, build/config, environment, seed/dataset, start/end UTC, actual elapsed observation, pass/fail criteria, result/artifact hashes, reviewer and remaining gaps. A mock test, accelerated clock or shortened run cannot be logged as real elapsed-market/provider evidence. Missing owner login/keys/capital activation produces the specific external blocker after independent work is complete. No live credential is uploaded to cloud development.
