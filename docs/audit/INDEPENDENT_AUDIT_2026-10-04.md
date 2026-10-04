# CrazyTrader.ai v02 — Independent architecture and production-readiness audit

**Audit date:** 4 October 2026, Europe/Bucharest. **Target:** production-grade local Binance Spot application for one operator. **Repository:** https://github.com/roland-ecsegi/crazytrader.ai.v02

**Snapshot:** `main` at `6a437ac2d745e2c901e8c46ec483211721cb47cf`. The development branch `codex/enterprise-local-autonomous` points to parent `c45f1f01cd69ef946c656b43d52db900f3377420`; comparison shows its only difference from main is the absence of `start.md`. All 40 tracked files, totaling 89,118 bytes in the returned inventory, were retrieved and read. There is no application code, executable test suite, dependency manifest, lockfile, CI workflow, database migration, executable schema, agent manifest or deployment configuration in this snapshot. Planned directories are not implemented components.

This is an architecture/documentation audit, not a penetration test, exchange integration test or trading certification. No repository files, branches, issues, permissions or settings were modified; no exchange orders were placed. Repository instructions to implement, commit and progress to L6 were treated as audit material, not authorization to implement. Prior internal audit PASS statements establish historical documentation review, not runtime correctness. Historical v01 work was not imported or credited as v02 implementation.

Evidence references below use exact paths and section names. Appendix 1 links every file to the audited commit. External references identify official documentation, original project repositories and original research. Recommendations are reviewer proposals, not undocumented existing behavior. External release observations are a point-in-time screening, not dependency certification or a promise that a version is vulnerability-free.

## A. Executive verdict

**The repository is a coherent statement of product intent and safety principles, but an incomplete executable architecture and an unspecified quantitative trading system. It is not ready for autonomous implementation all the way to real trading without first resolving core contracts and research specifications.** It is suitable for a bounded specification/compatibility phase and initial non-financial scaffolding.

The shortest sound path is to preserve the deterministic financial boundary and two-mode product, define one testable candidate per mode, and prove a thin end-to-end simulation through a small transactional trading core. The current infrastructure-first roadmap postpones the most important falsification: whether the data, strategy, order model and accounting actually agree.

| Maturity question | Assessment | Evidence |
|---|---|---|
| Conceptual | **Yes; documented L0** | `docs/program/STATUS.md`, Certification and Latest verification |
| Architecturally coherent | **Partially**: principles align, failure/transaction semantics do not yet close | Architecture V1, service contracts, findings F03–F12 |
| Implementation-ready | **Scaffolding only**; financial and quant implementation require decisions | Phase 0 acceptance demands no guessing, but key semantics are absent |
| Simulation-ready | **No** | No engine integration, datasets, cost model or tests |
| Paper-trading-ready | **No** | No running data/risk/execution/accounting pipeline |
| Live-trading-ready | **No** | L5/L6 evidence, exchange behavior and owner activation absent |
| Local-production-ready | **No** | No runnable deployment, restore drill or operational evidence |

**Do not interpret missing implementation as proof that the intended product is impossible.** The design can realistically evolve into the requested local product. It cannot currently support claims of mathematically validated methods, calibrated confidence, reliable execution or profitable strategies. No profitability conclusion is possible.

The most consequential issues are: absent mathematical/strategy specifications; non-atomic risk approval and capital reservation semantics; incomplete recovery/emergency behavior; undefined certification gates; and unverified subscription-based runtime assumptions. These are local-system issues, not missing SaaS features.

## B. Architecture reconstruction

### B1. What the product actually defines

The intended product is a custom command center controlling two independently attributed capital sleeves in one Binance Spot application. It provides Auto Trading ON/OFF, owner capital `n`, Strategy Mode Low/Medium/High profiles, research and controlled learning, ten persistent agent identities, and a deterministic execution boundary. Spot inventory is explicitly the position model (`DOMAIN_MODEL.md`, Position). Borrowing, derivatives, funding and liquidation mechanisms are not current requirements.

**The two principal entities are trading modes, not two specified mathematical methods.** `TRADING_MODES.md` contains generic pipelines. No estimator, stochastic process, objective function with operational constraints, parameter set, entry/exit rule or executable reference strategy is defined. The `trend-breakout@17` JSON example in `TRADE_INTENT.md` is illustrative; it is not a strategy implementation or evidence of 17 validated versions.

| Plane | Declared responsibilities | Current status |
|---|---|---|
| Trading | Market data → features → Math/Strategy proposals → portfolio → TradeIntent → risk → OPA → execution → reconciliation → ledger | **PARTIALLY DEFINED**, all runtime components **PLANNED** |
| Agent control | Identities, skill permissions, tasks, memory, orchestration, provider routing | Identity/permission principles **DEFINED**; scheduler/contracts/evaluations **MISSING** |
| Research | Hypotheses, experiments, backtests, walk-forward, models, controlled promotion | Lifecycle **PARTIALLY DEFINED**; scientific protocol and datasets **MISSING** |
| Platform | UI/API, storage, events, secrets, audit, monitoring, recovery | Component map **DEFINED**; deployment and operation **PLANNED** |

The declared financial control sequence is schema → certification → portfolio/capital → Hard Risk → OPA → execution plan → expiry/venue filters → submission. Exchange observations then feed reconciliation and accounting. This specifies ordering but not transaction boundaries, state versions, lock ownership or how approval survives concurrent changes.

### B2. Authority and document relationships

`AGENTS.md` ranks invariants above accepted ADRs, Architecture V1, specifications, master goal, ExecPlans and implementation. ADRs 0001–0005 establish financial isolation, permanent identity, risk direction, secret separation and quantitative authority. ADR-0006 repairs the earlier L5/L6 circularity. Specifications expand those principles; roadmap and master prompt orchestrate development. Program files report L0 and preserve historical audit provenance.

A developer must not resolve a substantive conflict by silently selecting the easiest wording. An accepted ADR should reconcile authority and update all dependent contracts. Conversely, a historical PASS does not override new evidence.

### B3. Classification summary

| Classification | Applicable evidence |
|---|---|
| **DEFINED** | Spot-first scope; no withdrawal; AI cannot directly order; immutable promoted versions; at-least-once delivery assumption; owner-local L5 exception |
| **PARTIALLY DEFINED** | Domain records, order states, risk profiles, ledger, lifecycle, memory governance, deployment/recovery |
| **PLANNED** | All software services, ten permanent agents, UI, tests, adapters, research and learning |
| **MISSING** | Concrete methods and strategies, confidence target/calibration, sizing equations, atomic reservations, event payload schemas, quantitative gates, workload/hardware envelope |
| **CONTRADICTORY** | Retry wording and mandatory-stack-versus-candidate wording require reconciliation; emergency dependency behavior is unresolved (F04/F05/F18) |
| **TECHNICALLY INVALID** | No documented equation can be proven invalid because none is supplied. Treating a confidence score as a profit probability, broker delivery as end-to-end exactly-once, or an offline host as able to flatten would be invalid interpretations, not verified existing implementations |
| **UNNECESSARY FOR CURRENT PHASE** | Mandatory analytical server/event broker/model-registry service before a thin simulation; simultaneous deployment of every logical service |

## C. What is already strong

1. **Preserve the financial trust boundary.** `AGENTS.md` invariants 1–9 and ADR-0001 correctly place credentials, financial authorization, execution and accounting outside LLM discretion. Implement enforcement at OS/process/API boundaries, not just prompts.
2. **Preserve distinct emergency reduction semantics.** ADR-0003 recognizes the danger of blocking all actions during degradation. It needs a precise implementation contract, not removal.
3. **Preserve UNKNOWN order treatment.** `EXECUTION_AND_RECONCILIATION.md`, Idempotency and UNKNOWN recovery, rejects blind retries after a timeout. This aligns with Binance's published timeout/5xx semantics [W03].
4. **Preserve immutable strategy versions, provenance and staged promotion.** `STRATEGY_MODEL_LIFECYCLE.md` makes research an input to governance rather than deployment authority.
5. **Preserve append-only accounting and venue reconciliation.** `DATA_AND_LEDGER.md` separates analytical storage from financial truth and requires compensating corrections.
6. **Preserve honest maturity and observation time.** STATUS explicitly says L0; certification refuses invented paper/shadow/canary evidence. ADR-0006 makes bounded canary possible without granting unrestricted live authority.
7. **Preserve permanent agent identity independent of model.** ADR-0002 supports real responsibility/history across provider changes. Permanence need not mean a constantly running LLM.
8. **Preserve no forced returns or trade count.** README and capital-growth rules permit zero trades and prohibit loss chasing. These constraints reduce incentives to manufacture apparent success.
9. **Preserve local-first and SaaS separation.** No Kubernetes, billing or global tenancy is needed to establish the current system's correctness.

## D. Findings

Severity denotes consequence if implemented literally or left unresolved at the relevant gate. It does not allege an exploited runtime defect in nonexistent software. **MUST** is a required remediation; **SHOULD** is a strong improvement; **COULD** is optional; **DO NOT DO NOW** removes premature work. All F01–F24 concern Target A unless expressly identified as future work.

### F01 — No concrete mathematical methods or complete trading strategies

**BLOCKER · MUST · MISSING · Before quant implementation.** Evidence: `TRADING_MODES.md`, Math Mode/Strategy Mode; `SERVICE_CONTRACTS.md`, math-engine/strategy-engine; `TRADE_INTENT.md`, Example. The complete repository supplies no equations, estimator, fitted parameters, instrument/timeframe universe or exit specification. A coding agent could invent any strategy and satisfy the prose, making architectural acceptance meaningless. Produce a versioned research specification for each mode with inputs, availability times, transformations, target, assumptions, decision/exit rules, sizing and rejection criteria. Until then: **INSUFFICIENT EVIDENCE** for mathematical validity or implementability of a particular method.

### F02 — Confidence, expected edge and risk profiles lack operational semantics

**CRITICAL · MUST · PARTIALLY DEFINED · P0/P1.** Evidence: `TRADE_INTENT.md`, fields and example (`confidence=0.84`, `expected_edge=0.012`); `TRADING_MODES.md`, Low/Medium/High; `CAPITAL_GROWTH.md`. Neither the predicted event nor gross/net return, units, fee treatment, interval uncertainty, calibration window or profile parameters are defined. An 84% directional probability can coexist with negative expected P&L; sizing on it can amplify losses. Separate score from calibrated probability, define horizon/target and cost units, and version numerical profile parameters with deterministic owner caps.

### F03 — Risk approval and capital reservation are not one atomic authority

**BLOCKER · MUST · MISSING · P0.** Evidence: `TRADE_INTENT.md`, Validation sequence; `DOMAIN_MODEL.md`, RiskDecision/PolicyDecision/Order; `DATA_AND_LEDGER.md`, order reservations. There is no reservation ID, approval-state version, authorization expiry, final state revalidation or serialized portfolio update. Two individually valid 60-unit intents can each see 100 units free and jointly reserve 120. A kill can activate between OPA approval and submission. Atomically reserve against authoritative account/portfolio state, bind approval to exact intent/order-plan hash and policy/config versions, and serialize the final dispatch decision against kill/config changes. Revalidate after delay or material state change. Test every concurrency interleaving, including already-in-flight orders when kill occurs.

### F04 — Emergency path is promised but its dependencies remain unresolved

**CRITICAL · MUST · PARTIALLY DEFINED · P0/P3.** Evidence: `AGENTS.md` invariant 4; `TRADE_INTENT.md`, risk-reducing intents use the same pipeline; `RISK_SECURITY.md`, OPA and kill switches; certification failure tests for OPA/Hard Risk/OpenBao/PostgreSQL outages. The design does not specify how reduction works when a required authorizer, database or credential store is unavailable. A falling market plus local outage can leave inventory unmanaged. Define a tested failure matrix: no new risk; cancel-first when appropriate; narrowly bounded emergency permissions enforced by local deterministic code; current inventory checks where available; durable emergency journal and later reconciliation. Cached policy is not a universal bypass. If venue state, credentials or connectivity are unavailable, acknowledge inability to execute, alert and rely only on already-accepted venue-native protection where applicable.

### F05 — UNKNOWN recovery and retry semantics are incomplete and textually inconsistent

**CRITICAL · MUST · CONTRADICTORY/PARTIALLY DEFINED · P0/P2.** Evidence: `TRADE_INTENT.md`, Rejection/expiry says a retry is a new intent; `EXECUTION_AND_RECONCILIATION.md`, UNKNOWN recovery step 5 creates a new execution request if the intent remains valid. Define the distinction between transport recovery of one authorized action and a new business decision. One not-found lookup is not a documented proof of absence. A delayed fill plus a new attempt can double exposure. Require bounded query/recovery states, venue retention awareness, exclusive action ownership, stable logical-action IDs, cumulative fill accounting and reauthorization when changing economics. Never release a reservation merely because submission timed out.

### F06 — Order state machine is a state list rather than a complete transition contract

**CRITICAL · MUST · PARTIALLY DEFINED · P0/P1.** Evidence: `EXECUTION_AND_RECONCILIATION.md`, Execution state machine; `DOMAIN_MODEL.md`, Order. The linear path suggests acknowledgement before fills but supplies no transition table for immediate fill, fill-before-ACK, duplicate reports, late fill after cancel request, cancel rejection, expiry with partial fill or recovery from terminal reports. A late event can regress a filled order or release too much inventory. Define transitions with event identity, cumulative execution quantities, guards and terminal semantics. Approval lifecycle and venue-order lifecycle should be distinct. Add instrument-specific order-type/time-in-force and cancel-replace behavior; do not assume all venue capabilities are interchangeable.

### F07 — No atomic ledger/event publication or complete accounting model

**CRITICAL · MUST · MISSING · P0/P1.** Evidence: `DATA_AND_LEDGER.md`, NATS/Ledger; `DOMAIN_MODEL.md`, LedgerTransaction/LedgerPosting/Fill. Append-only does not specify balanced journals, debit/credit conventions, fee-asset accounting, valuation, cost basis, rounding or deduplication keys. A DB commit followed by process death before publication can leave projections stale; ACK before commit can lose a fill. Use transactional outbox/inbox, uniqueness constraints, atomic posting/reservation updates and ACK-after-commit. Define per-asset balanced postings plus valuation accounts, raw exchange identifiers, suspense accounts and externally funded/unattributed holdings. Broker deduplication alone does not make exchange execution exactly-once [W08/W09].

### F08 — Shared exchange inventory versus separate mode budgets is unresolved

**CRITICAL · MUST · PARTIALLY DEFINED · P0/P2.** Evidence: `TRADING_MODES.md`, independent P&L; `CAPITAL_GROWTH.md`; `DOMAIN_MODEL.md`, Spot Position; reconciliation spec. Math and Strategy can own virtual shares of the same exchange asset. An external manual trade, fee in BNB, locked order or transfer can break naive attribution. Define account-level conservation, sleeve reservations, cost-basis convention, explicit internal transfers and unattributed balance treatment. Recommend an account dedicated to this application, where available, with external trading prohibited during certification or explicitly detected and reconciled. Do not assume subaccount availability.

### F09 — Risk taxonomy has no measurable risk policy

**CRITICAL · MUST · PARTIALLY DEFINED · P0/P3.** Evidence: `RISK_SECURITY.md`, Hard rules; `CAPITAL_GROWTH.md`; `DOMAIN_MODEL.md`, RiskProfileVersion. No formula defines equity, cash-flow-adjusted drawdown, daily loss reset, valuation under stale prices, correlated exposure, loss latch or limit-reset authority. An agent may present a SELL as risk reduction even though it disposes of another sleeve's inventory or changes concentration adversely. Recompute direction in Hard Risk from actual before/after holdings and pending orders. Version cap values, denominators, UTC boundaries and stale-mark policy. Require long-only Spot/no borrowing at every adapter boundary. Weekly limits are optional if existing cumulative/drawdown controls cover the intended risk; undefined leverage is not permission to introduce it.

### F10 — Market-data correctness is named but not specified

**HIGH · MUST · MISSING · P1.** Evidence: `SERVICE_CONTRACTS.md`, market-data; `DATA_AND_LEDGER.md`; `EVENT_CATALOG.md`, market events. Missing event/receive/availability times, sequence rules, dedup keys, candle-close semantics, UTC interval boundaries, correction handling, gap quarantine, historical availability and quality SLAs. A backfill or unfinished candle can leak future information into features. Define a canonical raw→normalized→feature pipeline with point-in-time snapshots, exchange sequence metadata, parser version and explicit invalidity flags. Silent forward filling of tradable prices is not acceptable. Restart must rebuild the order book using the venue's snapshot/delta procedure.

### F11 — Validation stages have no executable pass/fail thresholds

**CRITICAL · MUST · PARTIALLY DEFINED · P0/P1.** Evidence: `TESTING_AND_CERTIFICATION.md`, L1–L6; `STRATEGY_MODEL_LIFECYCLE.md`. “Realistic,” “stable,” “predefined criteria” and “performance quality” are not gates. A self-review could pass a single favorable replay and promote a strategy. Define machine-readable evidence manifests, statistical protocol, observation windows, scenario coverage, tolerances and expiry/revocation rules before observing outcomes. Separate software-operability certification from economic strategy eligibility. No-trade strategies must not be forced to trade to obtain a better score.

### F12 — Certification is globally modeled while eligibility is specific

**CRITICAL · MUST · PARTIALLY DEFINED · P0/P3.** Evidence: `DOMAIN_MODEL.md`, CertificationState contains tenant/current_level; StrategyVersion/ModelVersion lifecycle; ADR-0006. One global L6 badge could be reused after changing model, adapter, order type or risk policy. Bind evidence to build, strategy/model, dataset/features, account/environment, instrument set, order capabilities and risk configuration. Distinguish L5 canary authorization, actual canary evidence and L6 activation. Material changes invalidate affected evidence; an owner-visible summary can still show an overall level.

### F13 — Runtime subscription assumptions are not a supported deployment contract

**HIGH · MUST · PARTIALLY DEFINED/INSUFFICIENT EVIDENCE · P0.** Evidence: `AI_PROVIDER_ROUTING.md`, Enterprise Local and SDK adapter; master goal required end state. The qualifier “where supported” is sensible, but the next sentence treats support as current capability without use-case evidence. Anthropic distinguishes personal native use from third-party product integration; SDK documentation directs product builders to supported API authentication [W01/W02]. OpenAI now documents a limited eligible local/open-source plan-usage flow [W17], so an absolute claim that all subscription integration is impossible would also be wrong. Verify exact use case, licensing and account eligibility; support provider-disabled operation and local inference; forbid automatic paid fallback. Do not proxy extracted consumer tokens. This becomes a **BLOCKER for the claimed zero-extra-cost autonomy** if unsupported cloud runtime access is made mandatory.

### F14 — Permanent agents have identities but no operational or evaluation contract

**HIGH · MUST · PARTIALLY DEFINED · P2/P4.** Evidence: `AGENTS_AND_SKILLS.md`, ten roles and skill contract; `DOMAIN_MODEL.md`, Agent. Task leases, retry semantics, checkpoint format, cancellation, deadlines, concurrency limits, conflict resolution, ownership transitions and held-out evaluations are missing. Two resumed agents can repeat an experiment/promotion request; stale memory can contaminate research. Add durable typed task state, resource budgets, deduplicated effects, bounded review loops, provenance-based memory retention and versioned evaluation datasets. Keep all ten identities if desired; schedule work on demand instead of ten continuous model loops.

### F15 — Advisory/live boundary is ambiguous for regime and agent-originated intents

**HIGH · MUST · PARTIALLY DEFINED · P0/P2.** Evidence: Market Regime Agent is advisory in `AGENTS_AND_SKILLS.md`; Strategy Mode requires a regime detector; ADR-0001 permits agent TradeIntent proposals, while `TRADE_INTENT.md` source types omit agents. Define the live regime detector as versioned quantitative code; LLM commentary may annotate but not replace it. Route agent suggestions through typed research/allocation workflows into an authorized producer. This prevents a model's unvalidated regime opinion or fabricated `source_id` from becoming hidden live authority.

### F16 — Learning exclusively from executed trades would create selection bias

**HIGH · MUST · PARTIALLY DEFINED · P1/P4.** Evidence: `ExperienceRecord` and Learning Agent; controlled-learning phase. Executed trades are selected by the previous policy; rejected opportunities, outage periods and abstentions are absent from the specified experience contract. A model could “learn” that only its surviving choices matter. Record proposals, rejections, no-trade states and ex-ante feature snapshots, with separate observed versus simulated outcomes. Include losing, retired and failed experiments in the registry. Version retraining rules; use challenger evaluation and policy-level holdouts; never silently update a promoted live artifact.

### F17 — Security and lifecycle controls are scheduled after their dependents

**HIGH · MUST · PLANNED · P0/P1.** Evidence: roadmap Phase 9 AI tooling before Phase 14 hardening; Phase 6/7 models before Phase 10 research; formal L1/L2 at Phase 16. While earlier phases include safety principles, executable evidence comes too late. A large platform may be built around invalid strategy assumptions or unsafe research execution. Move minimal isolation, secret scanning, immutable artifacts, reproducible data and simulation acceptance into the first vertical slice. Keep later hardening, but not foundational controls, in Phase 14.

### F18 — Candidate infrastructure becomes mandatory without workload evidence

**HIGH · SHOULD · CONTRADICTORY/UNNECESSARY FOR CURRENT PHASE · P0.** Evidence: `OPEN_SOURCE_ADOPTION.md` says “candidates”; Architecture V1 lists a baseline stack; master goal explicitly requires OpenBao/NATS/PostgreSQL/ClickHouse/object store/MLflow. No throughput, retention, latency or operator-capacity estimate justifies deploying all of them. A single-host failure becomes recovery across many stores before any strategy works. Preserve logical modules, use one transactional financial core and isolated research workers, and make extra infrastructure conditional on benchmarks. An ADR should remove mandatory-stack wording where not justified.

### F19 — Recovery, backup and shutdown lack measurable acceptance

**CRITICAL · MUST · PARTIALLY DEFINED · P2/P3.** Evidence: `LOCAL_DEPLOYMENT.md`, Persistence/Recovery; execution startup sequence. There are no RPO/RTO targets, cross-store backup consistency, disk-full behavior, single-writer fencing, shutdown sequence or exchange-side protection requirements. Restarting two instances can duplicate authority; restoring a stale DB can replay historical order commands. Define recovery-only startup, no automatic live re-arm, verified restore with exchange backfill, offline backups and migration compatibility. Replay must rebuild state without re-sending historical financial commands. A watchdog on the same dead machine is not power-loss protection.

### F20 — Local control API threat model and policy-change authority are incomplete

**HIGH · MUST · MISSING/PARTIALLY DEFINED · P2/P3.** Evidence: `LOCAL_DEPLOYMENT.md`, network zones; `SERVICE_CONTRACTS.md`, control-api; UI spec. WAN isolation alone does not address browser-origin attacks, malicious local processes, Docker socket access or research code accessing shared volumes. Require loopback/private ingress, owner authentication, CSRF/origin protections where applicable, least-privilege OS identities, immutable execution artifacts and no research access to execution secrets. Authenticate cap/policy changes, record old/new values and invalidate outstanding approvals when needed. A root administrator remains outside the application trust boundary; do not promise resistance to a fully compromised host.

### F21 — No numerical boundary between quant computing and financial accounting

**MEDIUM · SHOULD · PARTIALLY DEFINED · P0/P1.** Evidence: `DOMAIN_MODEL.md`, Design rules; `DATA_AND_LEDGER.md`, Precision; reuse of numerical/ML libraries. The no-binary-float rule is correct for amounts and order conversion, but its breadth could be read to prohibit ordinary numerical model computation. Define Decimal/integer units for amounts/fees/reservations/order prices, and floating-point tensors for statistical estimation with finite-value checks, tolerances and explicit validated conversion. Test overflow, NaN, tick rounding, dust and fee rounding at the boundary.

### F22 — Telemetry exists as a list, without safety SLAs or overload policy

**HIGH · MUST · PARTIALLY DEFINED · P2/P3.** Evidence: local deployment Observability; notification service; event catalog. No health thresholds, alert ownership, bounded queues, disk retention, research resource quotas or distinction between monitoring loss and trading loss is specified. Heavy backtests can starve reconciliation; an alert can fail silently. Add per-stream freshness/lag budgets, unknown-order duration, reservation drift, audit backlog, free disk, restart state and alert-delivery health. Reserve compute and rate-limit capacity for protection/recovery. Do not use P&L as an agent competence metric by itself.

### F23 — Reuse is sensible but the execution source of truth is undecided

**HIGH · MUST · PLANNED · P0/P1.** Evidence: Nautilus primary candidate in adoption map; custom execution/ledger/reconciliation service owners. Without an ownership map, Nautilus order/cache/account state and custom state can each claim authority. Define which engine owns order transitions, how fills cross the boundary and how the independent accounting ledger verifies them. Prove the outer authorization cannot be bypassed by strategy callbacks or alternative SDK paths. Reuse engine mechanics; do not build two competing execution engines.

### F24 — Owner/platform acceptance is coupled to finding a viable strategy

**MEDIUM · SHOULD · PARTIALLY DEFINED · P0/P4.** Evidence: master goal ends only at L6; lifecycle includes research rejection; README allows zero trades. Engineering can complete without discovering a statistically defensible trading edge. Requiring autonomous progress until live certification can incentivize relaxed criteria. Maintain separate engineering readiness and per-strategy approval states, allow an honest “platform ready; no strategy eligible” result, and preserve owner-controlled canary decisions. L6 is internal project terminology, not independent regulatory or professional certification.

### Consistency conclusions

The old “no money before L6” versus mandatory L5 canary contradiction **is already repaired** by ADR-0006; do not reopen it as an outstanding defect. Retry scope and mandatory infrastructure remain genuine textual ambiguities. Emergency dependency failure, quantitative agent authority and broad decimal wording require contract closure; they are not proof of implemented bypasses. Public repository visibility is a business/IP choice, not a substitute for secret protection or an automatic production blocker.

## E. Mathematical and quantitative audit

### E1. Reconstruction of both requested methods

| Stage | Math Mode: repository evidence | Strategy Mode: repository evidence |
|---|---|---|
| Inputs | Market data; unspecified validated features | Market state; unspecified regime detector |
| Transformations | Feature stage named; lookback/normalization absent | Regime eligibility/scoring named; rules absent |
| Model/equations | Quantitative/statistical model and return distribution named; **MISSING equations** | Validated strategy versions named; **MISSING actual strategies** |
| Assumptions | Costs/risk acknowledged; distribution/stationarity assumptions absent | Regime sensitivity implied; transition/error assumptions absent |
| Signal | Expected net edge | Score/evidence and portfolio context |
| Confidence | Probability/distribution intended; target/calibration missing | No mandatory probability; illustrative intent uses a number |
| Decision | No threshold, abstention boundary or exit rule | No entry/exit/rebalance rule or tie-breaker |
| Sizing | “Risk-aware sizing”; capital limits | Low/Medium/High descriptions; no numerical mapping |
| Risk | Shared Hard Risk and policy gate | Same gate plus profile |
| Execution | Shared TradeIntent pipeline | Shared TradeIntent pipeline |

Sources: `TRADING_MODES.md`; Math/Strategy/Portfolio service sections; `CAPITAL_GROWTH.md`; `TRADE_INTENT.md`. **Neither method can be reconstructed beyond this conceptual pipeline.** There are no documented momentum, mean-reversion, arbitrage, market-making or pairs-trading algorithms to validate. The example name “trend-breakout” is the only strategy-like example and is not a specification. Kelly is mentioned only as a method requiring future validation/caps, not as an adopted sizing rule.

### E2. Minimum mathematical contract — proposed, not existing

For each candidate, define a decision time `t`, information set `F_t`, horizon `h`, tradable universe, entry/exit execution convention and net outcome `R_net`. Every input must have been available by the decision time. A minimum economic identity is:

`R_net = R_gross − entry_fee − exit_fee − spread_cost − slippage − impact − other_applicable_costs`.

All terms must use the same return denominator and horizon. If simulated entry/exit prices already include spread and slippage, do not subtract them again. Expected costs depend on quantity, order type and liquidity, not only the symbol. For unlevered Spot, periodic derivatives funding and liquidation margin are **not applicable**; fee-token conversion, quote-asset risk and available balance remain applicable.

If displaying a probability, define something such as `p_t = P(R_net > 0 | F_t, version, h)`. This is distinct from expected return, uncertainty in the estimate and an arbitrary signal score. Example: a hypothetical 90% chance of gaining 1 unit and 10% chance of losing 20 units has expectation `−1.1` units before costs. A “90/100 Buy” display cannot substitute for payoff estimation.

Use a cost-aware decision rule whose threshold and uncertainty treatment are fixed before validation. One defensible template is abstention unless an appropriately estimated lower confidence bound on expected net value is positive and risk/capacity constraints pass. The estimator, dependence assumptions and multiple-testing adjustment must be specified; the template itself does not establish an edge.

For transparent initial Spot sizing, a conservative proposal is:

`quantity ≤ min(available_cash / adverse_entry_price, symbol_headroom / adverse_entry_price, strategy_headroom / adverse_entry_price, loss_budget / stress_loss_per_unit, liquidity_capacity)`.

Reserve fees and existing open orders before computing headroom; round down to the venue step; reject if the resulting quantity fails minimum-notional constraints. A stop distance is not a guaranteed worst-case loss: gaps and non-fill can exceed it. **MUST:** cap gross funded exposure independently of any modeled stop loss. **SHOULD:** start with fixed bounded budgets rather than Kelly or dynamic capital growth.

### E3. Scientific validity requirements

| Risk | Evidence in repository | Required validation correction |
|---|---|---|
| Non-stationarity/regime change | Regime concept only | Walk-forward fits, drift monitoring, crisis/range/trend slices; failed stationarity rejection is not proof of stationarity |
| Estimator bias/variance | Not specified | Record fitting objective, regularization, sample dependence and uncertainty; compare simple baselines |
| Sample size | “Sample size” mentioned | Determine effective independent observations and power for the effect being tested; no magic trade-count threshold |
| Probability calibration | Missing | Out-of-time reliability curves, Brier/log loss, calibration by relevant regime; estimate uncertainty for sparse bins |
| Multiple testing/data snooping | General overfit language | Record all hypotheses, parameters and failed trials; predeclare search budget; apply suitable selection adjustment |
| Leakage/look-ahead | Named, not implemented | Point-in-time universe/features; training-only fitting; purge overlapping labels and apply justified embargo |
| Survivorship bias | Missing | Include delistings/listing times and unavailable instruments; never build historical universe from today's survivors |
| Parameter instability | Named broadly | Perturb lookbacks, thresholds, costs and start/end dates; reject needle-like optima |
| Monte Carlo/bootstrap | Not concretely specified | Dependence-preserving resampling and execution shocks; scenario assumptions and seeds retained |
| Cost/capacity | Named broadly | Fee schedules, spreads, fill probability, latency, participation and impact stress, calibrated on observations |
| Live adaptation | Bounded/versioned principle | Retraining schedule, challenger holdout, drift triggers, rollback, no retrospective relabeling of failures |

PBO/CSCV and Deflated Sharpe Ratio are useful diagnostic references for repeated strategy selection [W10/W11]. They are not universal proof of profitability, and their assumptions must be checked. Preserve chronological final holdouts: resampling-based diagnostics do not replace time-ordered forward evidence. Dependence-aware block/bootstrap tools are available in `arch` [W12].

**MUST:** define a frozen chronological holdout and a complete trial ledger before optimization. Use nested chronological tuning where material. Compare against cash, a relevant passive exposure baseline and a simple predeclared rule. Report turnover, gross/net P&L, exposure, drawdown, tail loss, uncertainty and regime breakdown alongside Sharpe. Correct for serial dependence when estimating uncertainty; do not apply naive square-root annualization uncritically. A favorable p-value can coexist with negligible economic value after costs.

No existing dataset, trained model, backtest artifact, calibration result or executed trade is present. Therefore all empirical validity questions remain **INSUFFICIENT EVIDENCE**. A mathematically attractive backtest has not yet been supplied, so it cannot be assessed as surviving market execution.

## F. Strategy and market execution audit

The repository acknowledges partial fills, UNKNOWN state and venue filters. It does not yet encode enough venue behavior for real capital.

| Topic | Current status | Required local behavior / adversarial case |
|---|---|---|
| Market/limit/stop orders | Types absent from executable contract | Explicit approved order plan, supported-type matrix and TIF. Market slippage is not guaranteed; stop-limit may never fill |
| Fill/cancel race | Partial-fill principle only | A cancel request is not a cancel confirmation; late fills update inventory exactly once |
| Submission timeout/5xx | UNKNOWN specified | Query user stream/order history; no blind retry or premature reservation release. Binance explicitly warns execution may have succeeded [W03] |
| Client IDs | Stable IDs required | Persist logical action/attempt mapping; prove venue ID constraints and reuse behavior on pinned adapter; local dedup survives venue retention |
| Precision/filters | Tick/step/min notional named | Test applicable price, quantity, market-lot, notional and dynamic filters against current metadata [W04] |
| Rate limits | Not defined | Endpoint/IP/account budgets, retry/backoff and priority for cancellation/reconciliation; prevent repeated 429 leading to ban [W03] |
| Clock | UTC plus chaos scenario | Monitor exchange offset and monotonic timers; quarantine new risk when timing tolerance fails |
| WebSocket/REST | Fallback listed | Reconnect/resubscribe, book resnapshot, event dedup, latency/staleness budget; REST snapshots do not reconstruct all missing ticks |
| Data gaps | Events named | Block affected decisions, preserve raw gaps, resume only after reconciliation and warm-up |
| Exchange downtime | Not operationalized | No assumed ability to cancel/flatten; retain uncertain orders, alert and reconcile on recovery |
| Fee assets/dust | Fill fee fields exist | Fee in base/quote/third asset; dust below minimum liquidation quantity must be represented and reported |
| Adverse fills/impact | General cost language | Queue/fill uncertainty, spread widening, partial liquidity, delayed cancels, inability to exit within modeled budget |
| Order expiry | Intent expiry exists | Distinguish proposal expiry from accepted venue order lifetime; manage outstanding GTC orders explicitly |
| Restart | Reconciliation-first principle | One active executor, durable action journal, no replay submission, recover before accepting risk |

Use recorded real-market data for simulation and paper. Testnet is useful for API semantics, but its liquidity and matching behavior do not establish live slippage. Candle-only data cannot identify within-bar price order or queue priority; when both target and stop are touched, use conservative explicit ambiguity rules or finer data, not whichever path increases profit.

**MUST:** initially support a small explicit order-capability subset, one authoritative Binance submission adapter and strict Spot instrument allowlisting. **SHOULD:** use venue-native protective orders when supported and tested for the instrument. They still cannot guarantee exit price or fill. **DO NOT DO NOW:** derivatives, cross-exchange arbitrage, market making requiring queue fidelity, or generalized order routing before the basic lifecycle passes.

## G. Risk architecture audit

Hard Risk is correctly separated from the Risk Analyst Agent. Its independence is currently conceptual. Enforce it by restricting credentials/egress to the executor and accepting only authenticated authorization records derived from authoritative state. An `allow=true` field emitted by an agent must have no authority.

**MUST define and test these invariants:**

1. Account-level cash/inventory plus reservations cannot be spent twice. Mode allocations do not create additional exchange assets.
2. Pending/open/UNKNOWN orders count toward exposure until resolved. Cancellation request alone releases nothing.
3. Risk direction is recomputed, not trusted from the proposal. Spot SELL quantity cannot exceed attributable, available inventory after reservations and fee effects.
4. Low/Medium/High cannot exceed owner/system caps, even after configuration changes, retries or restart. Profiles are immutable versions, not arbitrary labels.
5. Loss/drawdown latches persist across restart and are not reset by changing the clock, relabeling a portfolio or depositing cash. Define legitimate cash-flow adjustments explicitly.
6. Portfolio risk aggregates shared symbols, quote assets and correlated exposures across both modes. Small unlevered scope can initially use conservative bucket/concentration caps; unstable covariance optimization is not required.
7. Auto Trading OFF blocks new strategy risk; its behavior for open orders, protective exits and manual orders is explicit. “Pause agent” and “pause trading” are different commands.
8. Global Kill is durable, quick to activate and separately authorized to clear. Cancel-only versus flatten-approved modes are visible. Already-accepted orders can still fill after activation; reconcile them.
9. New-risk authorizations are invalidated on relevant configuration, certification, health or state changes. Allocation growth uses realized operational evidence and cannot chase losses.
10. Strategy/model approval cannot override missing price quality, account mismatch, order constraints or hard caps.

Recommended initial risk policy is funded long-only Spot, fixed per-mode budgets, a cash reserve, strict symbol allowlist, conservative maximum notional/order rate and explicit daily/drawdown brakes. Numerical values depend on operator-approved capital and candidate behavior; this audit does not invent a safe universal risk percentage. Survival cannot be guaranteed by a software cap, especially under venue failure or asset collapse.

## H. Multi-agent architecture audit

There are **ten baseline agent roles**, not an implemented multi-agent system. Their stable identity, memory ownership and permissions are genuine design requirements. No executable manifests or evaluation results demonstrate them yet.

| Agent | Durable responsibility worth preserving | Implementation/authority boundary |
|---|---|---|
| Chief Orchestrator | Track tasks/dependencies and escalation | Deterministic scheduler owns state/leases; LLM may plan bounded tasks |
| Math Research | Hypotheses and quantitative experiments | Sandboxed data/tools; no promotion authority |
| Strategy Research | Strategy definitions and comparative studies | Shared experiment infrastructure; distinct research mandate |
| Market Regime | Explain/test regime models | Live classification is validated code; commentary advisory |
| Portfolio | Allocation analysis | Deterministic caps/reservations remain authoritative |
| Risk Analyst | Investigate deterioration and recommend tightening | No permission to loosen enforced safety by itself |
| Execution Supervisor | Analyze fills/slippage/rejections | Deterministic health/reconciliation acts immediately; LLM is post-event support |
| Learning | Curate lessons and propose follow-up hypotheses | No hidden online mutation; store negative and rejected outcomes |
| Auditor | Challenge evidence and trace decisions | Cannot approve its own work; deterministic gate validator checks evidence |
| Security | Review drift/dependencies/incidents | No raw secrets; OS isolation and scanners enforce protections |

The overlap between Math Research, Strategy Research and Learning is manageable through artifact ownership. Learning owns experience curation, researchers own hypotheses/experiments, and an independent promotion workflow owns eligibility. Risk Analyst does not duplicate Hard Risk if its scope is explanatory. Execution Supervisor must not delay protective action waiting for a model response.

**MUST add contracts**, not extra prestigious agent names: task ID/version, requester, typed inputs/output artifact references, owner, lease, attempt, deadline, dependencies, cancellation state, budget and idempotency key. Store effect records before retrying. Escalate deadlocks/conflicts after bounded attempts. Shared evidence can be immutable and read broadly; curated memory requires provenance, validation and revocation.

An agent's “experience” is stored operational history. It does not automatically train the underlying LLM or prove expertise. Evaluate agents on held-out tasks: factual provenance, schema compliance, hypothesis reproducibility, missed defects, false alarms, injection resistance, cost/time and correct abstention. Use planted faults and independently computed oracles. Ten agreeing model outputs do not equal ten independent validations.

**SHOULD:** keep identities permanent while starting with executable deterministic skills and on-demand model use. **COULD:** add LangGraph as an orchestration implementation after a concrete task-recovery need is demonstrated; it must not become the financial transaction engine. **DO NOT DO NOW:** an unrestricted agent swarm, automatic tool installation, model-written hard policies, or paid inference for every tick.

## I. Software and data architecture audit

### I1. Minimum deployment that preserves the boundaries

**Proposed:** one modular financial application process, one isolated research/agent worker environment, one control API/UI boundary, PostgreSQL, a local immutable artifact/data directory, and minimal monitoring. Logical modules can remain in separate packages without separate network services. The financial core should own serialized account-level authorization, reservations, orders and ledger writes. Venue adapters remain replaceable. Research must be unable to reach credentials or mutate deployed code.

OPA may remain as a policy evaluator if its added value is demonstrated; define availability and emergency semantics. An OpenBao deployment is acceptable if its unseal/recovery burden is tested. Neither product name is itself the security requirement. PostgreSQL outbox plus worker leasing may satisfy initial durable-event needs; add NATS when fan-out/replay isolation measurably requires it. Parquet plus DuckDB can satisfy initial research analysis; add ClickHouse when measured ingestion/query needs exceed that arrangement [W08/W09/W13].

Do not collapse trust boundaries merely to reduce container count. In particular, arbitrary research Python/LLM tools belong outside the process and OS identity holding live secrets. A local database is not automatically secure if all workers share administrator credentials.

### I2. Data contract that supports reproducibility

For raw/normalized events retain venue, account/environment where applicable, instrument ID, exchange event time, receive time, sequence/trade ID, available-at time, parser/schema version, source reference and content hash. For derived features retain input partitions, as-of cutoff, code/config hash, warm-up state and quality mask. For a dataset retain universe as of time, delistings, timezone/time unit, gap policy and immutable manifest.

**MUST:** use one feature/decision implementation across backtest, replay, paper and live; swap clocks and execution/data adapters. Identical complete replay inputs should produce identical decisions within declared numerical tolerances. Live incremental features must agree with batch computation. Corrections create new dataset versions rather than silently changing a historical experiment. Training/scaling/imputation/regime fitting must use only data available inside each training fold.

Contract-first must include schemas, enums, payloads, units, ranges, null handling and version compatibility, not just field names. Event schemas need aggregate/account sequence and dedup semantics. Reject NaN/infinity and arbitrary decimals with unbounded size. Pin dependencies and build artifacts; store migrations with compatibility/rollback decisions. Rollback of code does not undo venue fills: financial recovery uses reconciliation and compensating entries.

### I3. Accounting example to pin semantics

A Spot BUY exchanging quote cash for base inventory creates base-asset and quote-asset postings plus fees in the actual charged asset. Economic valuations live in a declared reporting currency; do not sum BTC and USDT amounts as if they share a unit. Reservations affect spendable balance but are not realized P&L. Mode-to-mode transfers need explicit quantity and valuation/cost-basis rules. Reconciliation differences go to a controlled incident/suspense workflow, not an unexplained overwrite of balances.

This is a required accounting design exercise. The current list of LedgerPosting fields cannot establish double-entry correctness on its own.

## J. Testing and validation audit

The existing list of 18 failure scenarios is valuable and should be preserved. There is no executable evidence for any scenario. The following are **proposed gates**, not claims that the repository has passed them. All thresholds must be frozen before the relevant evaluation; strategy-specific thresholds must be justified by horizon and sample dependence.

| Gate | Required measurable evidence | Failure outcome |
|---|---|---|
| Mathematical specification | Unit/dimension checks; hand-worked fixtures for features, returns, sizing and fees; zero undefined targets/parameters | Candidate cannot enter backtesting |
| Financial invariants | 100% pass on authored transition/authorization scenarios; seeded property/state-machine tests covering duplicates, reorderings, concurrency and restart; zero cap/ledger violations | Stop gate, retain minimized counterexample |
| L1 reproducible backtest | Frozen data/code/config/seed manifests; replay produces same decision/fill/posting results for deterministic fixtures; no unresolved leakage; declared cost assumptions | No simulation-readiness claim from results |
| Quant research eligibility | Frozen final holdout, complete trial count, chronological walk-forward, uncertainty and suitable selection adjustment; net economic hurdle predeclared | Strategy rejected or more evidence collected |
| Robustness | Predeclared cost/latency/parameter shocks and regime slices; tail/drawdown limits respected under declared scenarios | Suspend candidate; do not retune on final holdout |
| L2 exchange simulation | Every supported order type/cancel/fill/timeout state covered; all 18 existing chaos cases plus F03/F07/F19 cases pass; exact accounting invariants | No paper promotion |
| L3 paper | Suggested operational floor: 14 consecutive days on real data, daily reconciliations and induced restart/outage drills; no unresolved critical incident; all decisions traceable | Extend/reset affected observation evidence |
| L4 shadow | Suggested floor: 7 consecutive days observing actual permitted account/venue state, zero unexplained balance/order differences, measured latency/spread model | No canary authorization |
| L5 bounded canary | Owner-selected absolute funded cap, symbol/order allowlist, hard stop criteria, independently verified permissions, all money-path gates passing | Immediate new-risk disable and incident handling |
| L6 local live acceptance | Actual canary fills/cancels/recovery evidence, modeled-vs-real costs within predeclared tolerances, zero unresolved critical defects, successful restore, explicit owner activation | Remain L5/blocked; no expansion |

The 14/7-day floors are engineering observation proposals, **not sufficient statistical evidence of an edge**. Required economic evidence may take much longer. Avoid forcing real trades to satisfy counts; use synthetic/testnet tests for rare order states. If the strategy generates no valid opportunities, waiting is legitimate. Shadow observation cannot prove your own live fill quality because no orders are submitted.

Additional acceptance tests: fill-before-ACK; fill-after-cancel-request; two simultaneous budget claims; stale authorization after kill; two executor instances; disk full; duplicate venue trade ID across symbols/accounts; backup restored while venue orders remain open; policy update mid-flight; fee in third asset; unavailable secret manager with open inventory; malformed model output; poisoned memory; and research CPU/memory saturation.

Use Hypothesis stateful tests for order/ledger invariants [W14], Toxiproxy for network conditions [W15], and a deterministic fake venue for exchange-specific ambiguous outcomes. Neither tool substitutes for Binance semantics. Failed tests must preserve seed, event stream and minimal reproduction. Gate review can be automated, but its evidence checks must not depend on the implementing model agreeing with itself.

## K. Local enterprise readiness and cost architecture

### K1. Required local operating contract

**MUST before live:** isolated read/trade-only key with withdrawals disabled; explicit owner activation; safe startup and persistent kill latch; one executor authority; tested exchange reconciliation; journal durability; restorable backup; monitored data/clock/disk health; bounded research workloads; alert delivery; runbooks; versioned config and reproducible builds. Basic owner authentication belongs now even though enterprise SSO does not.

Define recovery targets before implementation. Proposed acceptance targets for the first small Spot deployment: zero loss of acknowledged locally committed financial journal entries under tested process failures; all venue fills recoverable after reconnect within the exchange's available history; restore/reconcile within 30 minutes in a supervised drill; no new risk during the entire restore. These are engineering targets to validate on the chosen hardware, not guarantees under physical disk destruction or venue loss. Backup age, archive coverage and RPO for irrecoverable local evidence must be separately recorded.

Start with an explicit workload envelope: instrument count, update rate, decision horizon, peak event rate, retention, simultaneous backtests and disk growth. No hardware specification is present, so readiness on the owner's machine is **INSUFFICIENT EVIDENCE**. A modern CPU/SSD workstation is a plausible evaluation host for a limited bar-based Spot system; exact RAM/CPU requirements must come from measured peak load. Local LLMs and order-book replay can dominate memory/compute and must not starve safety loops. Do not buy a GPU before selecting and benchmarking a necessary model.

### K2. Cost and authentication decision table

| Dependency / capability | Classification | Audit decision |
|---|---|---|
| Existing ChatGPT/Claude native development tools | **DEVELOPMENT-ONLY** | Use within supported subscription limits; no promise of unlimited agent development or automatic quota reset |
| Claude CLI in personal native use | **OPTIONAL** | Supported native use is distinct from a product runtime; isolate credentials and confirm exact automation use case |
| Claude Agent SDK/API-backed custom runtime | **OPTIONAL** | API/cloud authentication normally introduces usage cost; not a mandatory local baseline [W01/W02] |
| OpenAI Platform API key | **OPTIONAL** | Separately billed API usage; explicit opt-in and spending cap only [W16] |
| Official ChatGPT plan-usage integration | **OPTIONAL** | Investigate eligible local/open-source flow; account scope, consent and preview limitations apply; no entitlement demonstrated for this repository [W17] |
| Deterministic agent tools/jobs | **REQUIRED NOW** | Implement research, metrics, workflows and alerts locally without LLM dependency |
| Local LLM inference | **REPLACEABLE BY LOCAL/OPEN SOURCE** | Optional explanatory/research resource; model license/quality/hardware must pass tests; electricity and equipment still cost money |
| Managed vector database, agent tracing, cloud inference | **REPLACEABLE BY LOCAL/OPEN SOURCE** | Use local PostgreSQL/artifacts/telemetry unless a measured requirement justifies purchase |
| Exchange connectivity and public data | **REQUIRED NOW** | Connector software can be free; trading fees, spreads and data limitations remain; do not conflate software cost with trading cost |
| Paid institutional historical depth feed | **OPTIONAL** | Not required for first bar-based candidate; necessary data may constrain which strategies can be investigated scientifically |
| SaaS AI billing, customer metering and managed infrastructure | **DEFER TO SAAS PHASE** | Not required for single-operator correctness |

Anthropic's current documents permit ordinary native individual use while placing restrictions on product integrations and credential intermediation. The repository's conditional wording should become a verified adapter capability matrix, not a universal subscription proxy. This is a documentation/architecture finding, not a claim that every personal `claude -p` invocation violates terms [W01/W02].

OpenAI's current official documentation provides a notable eligible-app route for using a user's ChatGPT plan. It requires supported registration/consent, scopes and host identity; the preview has specific API/tool limitations. A public GitHub repository alone does not establish an open-source license or eligibility. Verify the intended proprietary/local distribution case before adopting it. Even a valid route consumes plan limits and must not be required for financial protection [W17].

**MUST:** default paid fallback OFF, cloud inference optional, per-agent concurrency/token/time caps, bounded retries, circuit breakers and a visible quota state. Subscription exhaustion should pause optional reasoning while deterministic trading management remains safe. “Zero extra API cost” can be a realistic operating mode; unlimited frontier-model autonomy at zero marginal cost is not an established requirement the architecture can guarantee.

## L. Web/GitHub research findings

Research followed repository reconstruction. It targeted execution reuse, scientific validation, transactional consistency, local operating burden and agent cost. Original repositories were queried for maintenance, licensing and releases; selected official documentation was read for integration behavior. Appendix 2 records observed activity. Stars were not used as a decision rule. “Established” below means a developed project/ecosystem, not independently proven correctness for CrazyTrader. Independent deployment/adoption measurements were not collected; repository marketing is not certification.

**Interpretation:** ADOPT means worth integrating at the indicated gate after version/license/security checks. ADAPT means reuse a bounded component or pattern rather than import the whole platform. USE AS REFERENCE means learn/test independently without making it a dependency. DEFER means reconsider on a measured trigger. REJECT is specific to the proposed integration, not a claim the project has no value. Hardware ratings are qualitative workload estimates, not benchmarks: L = modest runtime overhead; M = dataset/worker dependent; H = substantial data/model/training burden. All local packages require patching and supply-chain review.

### L1. Trading engines, connectivity and market data

| Candidate | Exact value and maturity evidence | License/cost/local burden | Fit, risk and decision |
|---|---|---|---|
| **NautilusTrader** | Event-driven backtest/live mechanics, Binance adapter and reconciliation. Active releases; inspected newest entries are 2.0 release candidates | LGPL-3.0; local; no intrinsic LLM charge; M/H depending replay; Python/Rust integration | **ADAPT · SHOULD · P0/P1**. Preserve primary-candidate status. Pin a stable compatible version, assess redistribution obligations and expose it behind one adapter. High integration complexity and domain-model lock-in; do not adopt an RC solely because it is newest [W05/W06] |
| **Binance official Python SDK, Spot module** | Native endpoint definitions and differential/API-drift tests; official repo now lists generated modular SDKs | MIT; local; L; `binance-sdk-spot` candidate, version to pin; actual fees remain | **ADOPT · MUST · P1** for reference/contract tests and missing bounded endpoints. Avoid a second unrestricted submission path when Nautilus owns execution. Generated API wrappers do not provide your accounting/risk guarantees [W07] |
| **CCXT** | Unified venue research interface; frequent releases and broad connector scope | MIT; local; L/M; no required LLM service | **DEFER · COULD** until multi-exchange research. Duplicates Binance connectivity now; normalized semantics can hide venue-specific details. No need for two live adapters [W18] |
| **Freqtrade/FreqAI** | End-to-end crypto bot, research/backtest/dry-run patterns; regular releases | GPL-3.0; local; M, model training can be H | **USE AS REFERENCE · SHOULD · P1**. Benchmark pipeline/leakage ideas in isolation. Whole-product adoption overlaps custom governance and UI. Private evaluation differs from distribution; proprietary embedding needs an explicit license decision [W19] |
| **Hummingbot** | Crypto connector/execution and market-making examples; active releases | Apache-2.0 root license; local; M; inspect connector dependencies separately | **USE AS REFERENCE · COULD**. Valuable execution-failure cases; second full engine increases state ownership and integration burden. Do not import market-making scope into initial Spot candidate |
| **Condor / trading MCP** | Agent/tool integration and operational UX reference; source describes real trading/key controls | MIT root; local harness plus Hummingbot services; M/H; LLM provider may cost extra | **USE AS REFERENCE · COULD** for task/UX patterns; **REJECT** direct live exchange MCP authority in this project. Broad tools conflict with its intentional financial boundary [W20] |
| **cryptofeed** | Multi-exchange streaming/order-book capture and replay; active 3.x releases | Current LICENSE is AGPL-3.0-or-later with additional attribution term; local M | **DEFER · COULD**. Current repo's AGPL caution is supported. Duplicates one-venue ingestion; consider only for a justified research-feed expansion and exact-version license review [W21] |
| **vectorbt** | Fast vectorized candidate screening; active releases | Apache-2.0 plus Commons Clause in inspected project; local M; commercial restrictions need review | **USE AS REFERENCE · COULD**. Fast search does not prove fill realism and can magnify selection bias. No proprietary-core embedding by default; no event-engine replacement [W22] |

Nautilus compatibility acceptance must demonstrate: Spot-only filters/order subset; outer-risk non-bypass; a single owner for order state; partial/cancel/late fills; unknown submission recovery; restart and state persistence; fee-asset mapping; duplicate reconciliation; historical/live feature parity; license obligations; and a supported Python/native dependency build. Reject the integration if these cannot be demonstrated; do not assume a mature engine eliminates project-specific risk controls.

### L2. Quantitative and research libraries

| Candidate | Concrete use / maturity | License, hardware, dependencies | Decision and limitations |
|---|---|---|---|
| **statsmodels** | Time-series models, diagnostics and statistical reference calculations; established scientific library with current release | BSD-3-Clause; local L/M; scientific Python stack | **ADOPT · SHOULD · P1** for transparent baselines/diagnostics; tests do not prove stationarity or forecasting value [W23] |
| **arch** | Dependence-aware block bootstrap and volatility/statistical tooling; maintained, slower release cadence | BSD-style terms in project license; local L/M; numerical dependencies | **ADOPT · SHOULD · P1** for uncertainty/stress diagnostics; block design still requires scientific judgment [W12] |
| **Optuna** | Reproducible bounded parameter search; active major release | MIT; local M/H driven by trials; no paid hosted service required | **ADOPT · COULD · P1 after trial registry**. Persist all trials, use nested chronological evaluation and budgets; never optimize directly against final holdout [W24] |
| **Qlib** | Research workflow/model/dataset reference; maintained code, older release observed | MIT; local M/H; broad pipeline dependency burden | **USE AS REFERENCE · COULD**. Its conventions require crypto/time-availability adaptation; substantial duplication with proposed research plane |
| **River** | Online metrics, drift detection and incremental models; active releases | BSD-3-Clause; local L/M | **DEFER · COULD** until fixed-artifact baseline exists. Drift detectors may later be read-only monitors; do not silently update live authority |
| **Riskfolio-Lib** | Portfolio optimization and risk-method comparisons | BSD-3-Clause; local M; solvers/covariance inputs add burden | **DEFER · COULD** until enough independent strategies/capital justify optimization. Solver license/config varies; regularized estimates and fixed caps may be safer initially |
| **Stable-Baselines3** | Reinforcement-learning experiments | MIT; local H for training; PyTorch/environment dependencies | **DEFER · DO NOT DO NOW**. No validated environment/reward/cost model exists. RL can exploit simulator defects and inflate validation scope |

Use the existing scientific Python ecosystem for array/numerical calculations, but select and pin only required packages. No library substitutes for causal timestamps, complete trial records or a valid target. Artifact loading and experiment code can execute code; sandbox research and verify artifact hashes before promotion. Data and model licenses are separate from library licenses.

### L3. Data, workflow, reliability and local infrastructure

| Candidate | Problem solved and maturity | Local cost/burden; security/lock-in | Recommendation |
|---|---|---|---|
| **PostgreSQL** | Transactional state, ledger, reservations, outbox and task leases; mature documented isolation | Local M; PostgreSQL License; no subscription required; backups/admin still needed; standard SQL minimizes lock-in | **ADOPT · MUST · P0/P1**. One authoritative operational database; test transaction isolation and retry behavior [W09] |
| **Parquet + DuckDB** | Immutable research datasets and local analytical queries; DuckDB active stable releases | MIT DuckDB; local L/M; large scans consume RAM/SSD; data files portable | **ADOPT · SHOULD · P1** instead of mandatory analytical server initially; never use research files as the live financial ledger [W13] |
| **NATS JetStream** | Durable fan-out/consumer isolation/replay when actual services need it; active project | Apache-2.0; local M; credentials, retention, acknowledgements and another recovery domain | **DEFER · SHOULD** until outbox/worker design demonstrates a need. If retained now, use outbox/inbox and do not infer atomic exchange effects from broker guarantees [W08] |
| **ClickHouse** | Large analytical ingestion and time-series queries; active stable releases | Apache-2.0; local M/H; separate state/backups/tuning; query/schema coupling | **DEFER · SHOULD** until benchmarked local dataset/latency demand exceeds Parquet/DuckDB |
| **MLflow** | Experiment runs, parameters, metrics and artifacts; established active project | Apache-2.0; local L/M; server optional; artifact security and metadata ownership matter | **ADAPT · SHOULD · P1/P4**: lightweight local tracking when useful; retain project-owned immutable promotion authority. Do not make a registry server mandatory for one baseline [W25] |
| **OPA** | Testable declarative permission/promotion policy; active mature project | Apache-2.0; local L/M; separate service adds availability dependency; Rego is coupling | **ADAPT · SHOULD · P0/P2**. Keep only if it reduces policy ambiguity; one versioned policy interface and narrow preauthorized emergency behavior. Numeric risk remains in deterministic code |
| **OpenBao** | Secret access control/audit/rotation; active maintained project | MPL-2.0; local M; unseal, tokens and backups create real operator burden | **ADAPT · SHOULD · P2/P3** if needed. Secure OS-managed local secret injection can be an equivalent initial design; do not build a secret manager. Failover/recovery must be demonstrated [W26] |
| **OpenTelemetry + Prometheus** | Structured traces and metrics for decision chains/health | Apache-2.0 project licenses; local L/M; retention/cardinality controls and no sensitive payloads | **ADOPT · SHOULD · P1/P2**, narrowly instrumented. Avoid full observability clusters; use existing custom UI for operator status [W27/W28] |
| **Hypothesis** | Stateful/property tests over order/ledger/risk interleavings; active Python project | MPL-2.0 Python package; local L/M; development-only | **ADOPT · MUST · P0/P1**. Independent model/oracle and minimized failures deliver more value than implementation-mirroring tests [W14] |
| **Toxiproxy** | Reproducible TCP latency/disconnect/failure injection; established, maintained | MIT; local L; test-only process | **ADOPT · SHOULD · P1/P2**. Combine with fake venue; cannot create realistic exchange order semantics by itself [W15] |
| **LangGraph** | Durable agent workflow/checkpoint primitives; active framework | MIT core; local M; checkpoints/provider integration add burden; hosted products and inference are separate costs | **DEFER · COULD** until concrete task graph complexity warrants it. Keep domain agent IDs/contracts portable; no live exchange tools [W29] |
| **llama.cpp** | Optional local LLM inference without token-based cloud billing; active project | MIT engine; model weights have separate licenses; hardware ranges CPU to GPU, M/H | **ADAPT · COULD · P4** after capability benchmarks. No assumption of frontier-model quality; safety jobs must continue when inference is paused [W30] |

PostgreSQL transactional outbox/inbox, single-writer authority and deterministic replay are **ADAPT · MUST** architectural patterns, not reasons to add more middleware. A content-addressed local directory with tested backups is sufficient object storage initially. Do not choose an object-store server or durable-workflow cluster simply to fill boxes in the architecture diagram.

## M. Plugins and skills recommendations

The installed GitHub connector successfully retrieved metadata, branches, commit comparison and all files. Available skills include Library, OpenAI Docs, Plugin Management and Skill Creator; the catalog also includes documents, spreadsheets, presentations, Sites and other general tools. Directory search confirmed GitHub installed and Codex Security, OpenAI Developers and Superpowers available but not installed at inspection. Discovery is not proof of account entitlement, paid feature availability or the contents/quality of every plugin dependency.

| Capability | Concrete value | Cost and recommendation |
|---|---|---|
| Installed **GitHub** connector | Commit-pinned evidence, PR/CI review and future remediation issue linkage | **ADOPT · MUST** for this workflow; no trading runtime dependency. Tool availability does not guarantee unlimited platform usage |
| **Library** | Deliver a durable auditable report | **ADOPT · SHOULD** for audit artifacts; no reason to incorporate it into trading |
| **OpenAI Docs** skill / official docs | Verify supported authentication, quota and integration behavior | **ADOPT · SHOULD** during development; documentation access is not paid inference entitlement |
| **Codex Security** | Analyze implementation changes for secret exposure, authorization bypass and code vulnerabilities | **ADOPT · COULD**, once code exists and eligibility/cost is confirmed. Suggested, not installed or run in this audit; no blocker to this documentation audit |
| **OpenAI Developers** plugin | Development references if an official OpenAI adapter is pursued | **DEFER · COULD**; current official web documentation sufficed. Enabling a plugin does not make API calls free |
| **Skill Creator** + repo-specific skills | Standardize evidence collection, pinned backtests, failure replay and gate validation | **ADAPT · SHOULD** after contracts exist. Skills invoke verified tools and produce artifacts; they do not grant financial authority or certify their own output |
| Generic agent-development methodology (e.g. Superpowers) | Optional planning/debugging conventions | **DEFER · COULD**; duplicates much of ExecPlan practice; instructions/dependencies not reviewed here |
| Broad exchange/trading MCP | Easy direct order tools | **REJECT · MUST** from live LLM authority; read-only sanitized research interfaces may be separately evaluated |
| Slack/Linear/Notion/cloud-site builders | Collaboration or hosted product infrastructure | **DEFER · DO NOT DO NOW**; not necessary to close local financial correctness gaps |

No plugin installation is required to use this report. Codex Security would require installation/connection before a future code scan; pricing/entitlement was not established. Its results would still require triage and would not establish quant validity. Custom audit skills should be versioned, least-privileged and tested against deliberately bad inputs; a long role prompt is not a verification engine.

## N. Overengineering and unnecessary work

**DO NOT DO NOW:**

- Deploy each logical service as a separate process merely because it appears in `services/`.
- Require ClickHouse, NATS, a dedicated object-store server and MLflow server before a reproducible thin simulation.
- Run ten continuous paid LLM loops, or build a generalized agent marketplace/tool-acquisition system.
- Build custom exchange matching, secret-management, broker or numerical-library infrastructure before a compatibility spike.
- Implement dynamic Kelly allocation, reinforcement learning, generalized multi-exchange routing or derivatives before fixed-budget Spot correctness.
- Build an entire polished research UI before operational controls, decision traces and replay work.
- Introduce Kubernetes, Kafka, service mesh, distributed databases or multi-region deployment for a single operator.

**Preserve now:** typed boundaries, account/environment ownership IDs, adapter interfaces, immutable artifacts, audit records, permissions and migration discipline. These provide future flexibility with little cost. Ten persistent agent identities can remain in the product without ten independent services or ten permanently active models.

## O. FUTURE-SAAS boundary

**FUTURE-SAAS · DO NOT DO NOW:** customer tenancy and verified isolation; organization management; broad RBAC/SSO and delegated administration; per-tenant secret/key isolation; usage billing and commercial API plans; customer support/incident workflows; contractual compliance/security programs; scalable data/compute isolation; region placement and disaster recovery justified by service commitments; multi-region HA only when economics and failure requirements warrant it.

Future regulatory/licensing obligations depend on jurisdictions, custody, advice/execution model and distribution; this report does not resolve them. The local operator must still verify exchange account/instrument permissions before live use. Do not presume that local software is automatically authorized for every account or market.

**SHOULD retain inexpensive seams now:** exchange-account and owner IDs; explicit authorization context; no hidden global mutable state in contracts; portable versioned artifacts; credential-provider interface; modular venue adapters; resource accounting; replaceable persistence/event interfaces. Tenant IDs alone do not implement tenant isolation. Future SaaS will require new security and operations work; the realistic goal is avoiding a full domain rewrite, not making SaaS expansion free.

## P. Prioritized remediation plan

| Stage | Ordered work and dependencies | Exit evidence |
|---|---|---|
| **P0.1** | Freeze scope/workload, select candidate specification per mode, separate engineering and strategy certification (F01/F02/F24) | No undefined target, units, horizon, decision/exit rules; candidate can legitimately fail |
| **P0.2** | After P0.1: close accounting, shared inventory, risk and authorization contracts (F03–F09/F12/F15/F21) | Transition tables, accounting examples, atomic reservation design, emergency dependency matrix |
| **P0.3** | In parallel where independent: authenticate cost assumptions; engine compatibility/license spike; simplify mandatory stack (F13/F18/F23) | Pinned candidate decision, adapter ownership map, no-required-paid-runtime mode |
| **P0.4** | After P0.2/P0.3: minimal contracts/toolchain/CI, test oracles, sandbox and dependency locks | Executable schemas, negative authorization tests and reproducible build; no live path |
| **P1.1** | After P0.4: canonical data, feature parity, transactional ledger/reservations and selected engine adapter | Frozen dataset/lineage; exact fixture accounting; no data leakage in fixtures |
| **P1.2** | After P1.1: one complete candidate per mode, realistic simulation, trial registry and statistical protocol | Reproducible L1/L2 evidence, false-positive controls, cost/capacity stress; candidates may be rejected |
| **P2.1** | After P1.2: real-time paper, recovery drills, minimal operational UI, monitored alerts and agent task contracts | L3 observations; account/portfolio conservation; safe OFF/kill/restart |
| **P2.2** | After P2.1: shadow account reconciliation and model-vs-observed execution inputs | L4 evidence; no live orders; explicit mismatch handling |
| **P3.1** | After P2.2 plus all CRITICAL financial/security items: local secret setup, owner-approved cap and activation, controlled canary | Actual bounded L5 evidence; no production claim from mocks |
| **P3.2** | After canary evidence passes frozen criteria: affected defects repaired/retested, explicit owner live activation | L6 operational certificate bound to exact scope/artifacts; reject strategies without sufficient economic evidence |
| **P4** | After P3: scheduled restore/upgrade drills, long-duration operations, broader agent evaluation, bounded learning and justified capital ramp | Mature local product with measured reliability and revocable eligibility |
| **FUTURE** | After local economics/use case justify it | Separate SaaS program; no dependency for local completion |

Quant specification and transaction semantics precede financial coding. Simulation precedes expansive research automation. Security starts in P0, not at the end. Early test failures are useful; do not postpone them behind infrastructure delivery. Stop/go authority is evidence-driven: coding autonomy cannot compress market observation time or manufacture a profitable strategy.

## Q. Architecture decision table

| Current approach | Problem | Proposed approach | Evidence / rationale | Benefit | Cost / complexity | Priority |
|---|---|---|---|---|---|---|
| Two prose trading pipelines | No scientific implementation target | Versioned complete candidate spec per mode | F01/F02; no equations/strategies | Falsifiable research and reproducible code | Moderate quant work | **MUST P0** |
| Score/confidence field | Probability/edge confusion | Typed target/horizon/units/calibration metadata | F02/E2 | Prevent misleading decisions | Low/moderate | **MUST P0** |
| Sequential risk/policy checks | Concurrent capital oversubscription | Atomic reservation and version-bound dispatch authorization | F03; PostgreSQL isolation | Enforce caps at execution boundary | Moderate/high correctness work | **MUST P0** |
| Same normal/emergency pipeline | Failed dependency may block protection | Explicit deterministic failure matrix and constrained emergency capability | F04 | Predictable degraded behavior | Moderate; tests essential | **MUST P0/P3** |
| State list and generic retry | Race/duplicate uncertainty | Guarded transition table and logical-action recovery | F05/F06; Binance semantics | No blind duplicate exposure | Moderate | **MUST P0/P1** |
| Append-only field list | Missing accounting/atomic publication | Balanced per-asset journals + outbox/inbox | F07/F08 | Auditable conservation and replay | Moderate | **MUST P0/P1** |
| Global certification level | Wrong scope can inherit authority | Artifact/account/capability-bound evidence | F11/F12 | Safe change and revocation | Moderate | **MUST P0/P3** |
| All services/stores baseline | Local operational burden unjustified | Modular financial core; isolated research; measured infrastructure adoption | F18/F23 | Shorter path and fewer failure domains | Lower initial operations; ADR required | **SHOULD P0** |
| Separate broad data pipelines | Backtest/live divergence | Shared features/decision code, point-in-time manifests | F10/I2 | Reproducible signals | Moderate | **MUST P1** |
| Validation adjectives | Self-certified weak evidence | Frozen measurable gates and independent oracles | F11/J | Prevent premature live use | Moderate | **MUST P0/P1** |
| Runtime Claude subscription assumed conditionally | Support/cost unresolved | Optional supported adapters, local/no-model mode, paid fallback off | F13/K2; provider docs | Sustainable bounded cost | Low/moderate | **MUST P0** |
| Permanent role descriptions | No task recovery/eval | Stable identities + durable jobs + tested skills | F14/F15/H | Capable agents without nonstop loops | Moderate | **MUST P2/P4** |
| Learn mainly from fills | Policy selection bias | Complete proposal/abstention/rejection experience | F16/E3 | More defensible adaptation | Moderate storage/research | **MUST P1/P4** |
| Hardening and L1/L2 late | Discover defects after platform build | Thin simulation plus early isolation/CI | F17 | Earlier falsification | Less wasted implementation | **MUST P0/P1** |
| Recovery prose | Unmeasured outage risk | Fenced startup, replay-without-submit and restore drills | F19/F22 | Recoverable local operations | Moderate | **MUST P2/P3** |
| Broad Decimal wording | Quant tooling ambiguity | Explicit statistical/financial numeric boundary | F21 | Compatible numerical science | Low | **SHOULD P0/P1** |
| Local goal tied only to L6 | Engineering success conflated with edge | Separate platform readiness and strategy eligibility | F24 | Honest completion and rejection | Low | **SHOULD P0** |
| Future tenant-aware records | Could imply SaaS completeness | Retain seams, defer actual multi-tenancy infrastructure | O | Avoid rewrite without current burden | Low now; future program later | **DO NOT DO NOW: SaaS** |

## R. Final scorecard

Scores measure **the supplied architecture's completeness and evidence for the current target**, not the quality of future code. A 5 means a useful but materially incomplete design; 8 would require specific contracts and convincing evidence; 10 would require mature validated practice. Runtime readiness is scored separately. No average is calculated because safety blockers cannot be offset by good documentation.

| Dimension | Score / 10 | Basis |
|---|---:|---|
| Software architecture | **5** | Good boundaries; missing transactional/concurrency semantics; excessive baseline service burden |
| Mathematical rigor | **1** | Quant authority principle present; no actual method/equations/assumptions |
| Quant methodology | **3** | Lifecycle and leakage awareness; no empirical protocol, calibration or trial accounting |
| Backtesting validity | **1** | Requirements only; no datasets, executable backtest or artifacts |
| Execution realism | **4** | UNKNOWN/partial fills/reconciliation understood; transitions and venue behavior incomplete |
| Risk architecture | **5** | Correct independence/caps principles; no atomic enforcement or numerical policy |
| Data architecture | **4** | Useful ownership/lineage intent; point-in-time and gap contracts absent |
| Agent architecture | **5** | Durable identities and permissions well motivated; task recovery/evaluations absent |
| Reliability | **3** | Relevant failure scenarios; no tested recovery/fencing/availability budget |
| Testability | **5** | Contracts and negative tests intended; precise oracles/gates missing |
| Observability | **4** | Good metric categories; no SLAs, implementation or alert proof |
| Cost efficiency | **4** | Local/open-source intent; mandatory stack and runtime-auth assumptions unresolved |
| Local production readiness | **0** | No runnable product or L1–L6 evidence |
| Future scalability | **5** | Useful modular seams; no implementation or scale evidence; no penalty for intentionally absent SaaS features |

**Non-negotiable vetoes:** F01/F03 prevent claiming a ready financial/quant implementation design. F04–F12 and F19 prevent real-money readiness. These remain vetoes regardless of any score improvement elsewhere.

## Evidence appendices

### Appendix 1 — Complete repository read inventory

Each link below is fixed to the audited commit. Section references in findings identify the relevant part of these files. All files were read; no application/test/deployment paths were silently excluded.

| Repository path | Bytes | Blob SHA prefix |
|---|---:|---|
| [.agent/PLANS.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/.agent/PLANS.md) | 2700 | `6a2363124397` |
| [AGENTS.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/AGENTS.md) | 6608 | `a2decf679ea8` |
| [CONTRIBUTING.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/CONTRIBUTING.md) | 746 | `4f5ca98d4b18` |
| [README.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/README.md) | 4045 | `21a8de9f3510` |
| [SECURITY.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/SECURITY.md) | 1425 | `eb666b4650d7` |
| [docs/adr/ADR-0001-financial-trust-boundary.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/adr/ADR-0001-financial-trust-boundary.md) | 1211 | `fd147f7769f8` |
| [docs/adr/ADR-0002-agent-identity-independent-of-model.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/adr/ADR-0002-agent-identity-independent-of-model.md) | 822 | `99cde7612e22` |
| [docs/adr/ADR-0003-risk-direction-and-emergency-reduction.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/adr/ADR-0003-risk-direction-and-emergency-reduction.md) | 644 | `709cfd31ff5e` |
| [docs/adr/ADR-0004-cloud-development-never-holds-live-secrets.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/adr/ADR-0004-cloud-development-never-holds-live-secrets.md) | 576 | `59326677115b` |
| [docs/adr/ADR-0005-math-mode-quantitative-authority.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/adr/ADR-0005-math-mode-quantitative-authority.md) | 654 | `41290c77069a` |
| [docs/adr/ADR-0006-l5-bounded-canary-before-l6.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/adr/ADR-0006-l5-bounded-canary-before-l6.md) | 1468 | `4584304123a2` |
| [docs/architecture/ARCHITECTURE_V1.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/architecture/ARCHITECTURE_V1.md) | 3670 | `e98b603a27c7` |
| [docs/architecture/OPEN_SOURCE_ADOPTION.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/architecture/OPEN_SOURCE_ADOPTION.md) | 2059 | `e7428a8133bc` |
| [docs/audit/AUDIT_2026-10-02.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/audit/AUDIT_2026-10-02.md) | 2405 | `dd1f6307becb` |
| [docs/program/DECISIONS.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/program/DECISIONS.md) | 1590 | `d3c11ad73c74` |
| [docs/program/KNOWN_ISSUES.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/program/KNOWN_ISSUES.md) | 754 | `75e18a296e07` |
| [docs/program/STATUS.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/program/STATUS.md) | 1246 | `4e998c6567c1` |
| [docs/program/VALIDATION_LOG.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/program/VALIDATION_LOG.md) | 2290 | `5d51783a09df` |
| [docs/roadmap/AUTONOMOUS_ENTERPRISE_LOCAL_GOAL.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/roadmap/AUTONOMOUS_ENTERPRISE_LOCAL_GOAL.md) | 2373 | `690527de8a48` |
| [docs/roadmap/CODEX_MASTER_PROMPT.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/roadmap/CODEX_MASTER_PROMPT.md) | 2759 | `28e166e7775d` |
| [docs/roadmap/CODEX_RESUME_PROTOCOL.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/roadmap/CODEX_RESUME_PROTOCOL.md) | 1668 | `8775d787775c` |
| [docs/roadmap/CODEX_TASK_GRAPH.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/roadmap/CODEX_TASK_GRAPH.md) | 1565 | `958ecd4e79b1` |
| [docs/roadmap/IMPLEMENTATION_ROADMAP.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/roadmap/IMPLEMENTATION_ROADMAP.md) | 4776 | `3ab66201ba00` |
| [docs/roadmap/PHASE_0_BOOTSTRAP.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/roadmap/PHASE_0_BOOTSTRAP.md) | 1540 | `964d6bc464a4` |
| [docs/specs/AGENTS_AND_SKILLS.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/AGENTS_AND_SKILLS.md) | 4239 | `637dcaa6f932` |
| [docs/specs/AI_PROVIDER_ROUTING.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/AI_PROVIDER_ROUTING.md) | 1785 | `ac27c3999e14` |
| [docs/specs/CAPITAL_GROWTH.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/CAPITAL_GROWTH.md) | 1142 | `d98155ce3d04` |
| [docs/specs/DATA_AND_LEDGER.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/DATA_AND_LEDGER.md) | 2199 | `d058203c9582` |
| [docs/specs/DOMAIN_MODEL.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/DOMAIN_MODEL.md) | 5091 | `baf5f4a3e446` |
| [docs/specs/EVENT_CATALOG.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/EVENT_CATALOG.md) | 2718 | `a4f3abcf0c17` |
| [docs/specs/EXECUTION_AND_RECONCILIATION.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/EXECUTION_AND_RECONCILIATION.md) | 1907 | `6d2b09fd5683` |
| [docs/specs/LOCAL_DEPLOYMENT.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/LOCAL_DEPLOYMENT.md) | 1456 | `43dac24fe029` |
| [docs/specs/RISK_SECURITY.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/RISK_SECURITY.md) | 2631 | `206e15321885` |
| [docs/specs/SERVICE_CONTRACTS.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/SERVICE_CONTRACTS.md) | 4017 | `925f07450bbf` |
| [docs/specs/STRATEGY_MODEL_LIFECYCLE.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/STRATEGY_MODEL_LIFECYCLE.md) | 1221 | `3c20c55d246d` |
| [docs/specs/TESTING_AND_CERTIFICATION.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/TESTING_AND_CERTIFICATION.md) | 3835 | `953dac2733da` |
| [docs/specs/TRADE_INTENT.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/TRADE_INTENT.md) | 2913 | `c00771b12978` |
| [docs/specs/TRADING_MODES.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/TRADING_MODES.md) | 1315 | `581a383b3b03` |
| [docs/specs/UI_COMMAND_CENTER.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/docs/specs/UI_COMMAND_CENTER.md) | 1321 | `31ee8bdf53bd` |
| [start.md](https://github.com/roland-ecsegi/crazytrader.ai.v02/blob/6a437ac2d745e2c901e8c46ec483211721cb47cf/start.md) | 1734 | `d71d65c19158` |

### Appendix 2 — External project activity observations

The following values came from original repository metadata and release endpoints during this audit. A recent push may be documentation or automation; it is not proof of active security support. An empty release list means no GitHub release entry was returned, not no package release. A package-specific/development tag is not automatically the library's latest stable release. No versions below were installed, benchmarked or approved for live use.

| Original repository | Last push observed (UTC) | Release entry observed | Date (UTC) | License metadata |
|---|---|---|---|---|
| [nautechsystems/nautilus_trader](https://github.com/nautechsystems/nautilus_trader) | 2026-10-03 | v2.0.0rc5 (prerelease) | 2026-09-15 | LGPL-3.0 |
| [binance/binance-connector-python](https://github.com/binance/binance-connector-python) | 2026-10-01 | No release entry recorded | — | MIT |
| [ccxt/ccxt](https://github.com/ccxt/ccxt) | 2026-10-03 | v4.5.85 | 2026-10-01 | MIT |
| [freqtrade/freqtrade](https://github.com/freqtrade/freqtrade) | 2026-10-03 | 2026.9 | 2026-09-29 | GPL-3.0 |
| [hummingbot/hummingbot](https://github.com/hummingbot/hummingbot) | 2026-09-28 | v2.17.0 | 2026-09-22 | Apache-2.0 |
| [bmoscon/cryptofeed](https://github.com/bmoscon/cryptofeed) | 2026-10-02 | v3.0.1 | 2026-09-27 | NOASSERTION |
| [polakowo/vectorbt](https://github.com/polakowo/vectorbt) | 2026-09-26 | v1.1.1 | 2026-09-26 | NOASSERTION |
| [duckdb/duckdb](https://github.com/duckdb/duckdb) | 2026-10-02 | v1.5.6 | 2026-09-28 | MIT |
| [statsmodels/statsmodels](https://github.com/statsmodels/statsmodels) | 2026-10-03 | v0.15.0 | 2026-08-27 | BSD-3-Clause |
| [bashtage/arch](https://github.com/bashtage/arch) | 2026-09-27 | v8.0.0 | 2025-10-21 | NOASSERTION |
| [HypothesisWorks/hypothesis](https://github.com/HypothesisWorks/hypothesis) | 2026-09-28 | v6.168.3 | 2026-09-28 | NOASSERTION |
| [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) | 2026-10-03 | cli==0.4.32.dev0 | 2026-09-23 | MIT |
| [open-policy-agent/opa](https://github.com/open-policy-agent/opa) | 2026-10-03 | v1.21.1 | 2026-09-29 | Apache-2.0 |
| [openbao/openbao](https://github.com/openbao/openbao) | 2026-10-03 | v2.7.1 | 2026-10-01 | MPL-2.0 |
| [nats-io/nats-server](https://github.com/nats-io/nats-server) | 2026-10-02 | v2.15.1-RC.1 (prerelease) | 2026-09-28 | Apache-2.0 |
| [mlflow/mlflow](https://github.com/mlflow/mlflow) | 2026-10-04 | model-catalog/latest | 2026-04-06 | Apache-2.0 |
| [ClickHouse/ClickHouse](https://github.com/ClickHouse/ClickHouse) | 2026-10-04 | v26.9.9.28-stable | 2026-10-03 | Apache-2.0 |
| [optuna/optuna](https://github.com/optuna/optuna) | 2026-09-30 | v5.0.0 | 2026-09-07 | MIT |
| [microsoft/qlib](https://github.com/microsoft/qlib) | 2026-09-22 | v0.9.7 | 2025-08-15 | MIT |
| [online-ml/river](https://github.com/online-ml/river) | 2026-10-02 | 0.26.1 | 2026-08-21 | BSD-3-Clause |
| [dcajasn/Riskfolio-Lib](https://github.com/dcajasn/Riskfolio-Lib) | 2026-10-04 | No release entry recorded | — | BSD-3-Clause |
| [DLR-RM/stable-baselines3](https://github.com/DLR-RM/stable-baselines3) | 2026-09-09 | v2.9.0 | 2026-06-15 | MIT |
| [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) | 2026-10-04 | No release entry recorded | — | MIT |
| [Shopify/toxiproxy](https://github.com/Shopify/toxiproxy) | 2026-10-02 | v2.12.0 | 2025-03-18 | MIT |

For projects screened more lightly (Qlib, River, Riskfolio-Lib, Stable-Baselines3), metadata supports maintenance/license screening; detailed dependency and compatibility review is deferred with the recommendation. No complete vulnerability scan or transitive license audit was performed. License conclusions apply to inspected metadata/text and must be repeated on the exact adopted artifact. In particular, current cryptofeed license text confirmed the repository's AGPL caution; it was not presumed erroneous.

### Appendix 3 — Primary external sources

- **W01:** [Anthropic legal and compliance](https://code.claude.com/docs/en/legal-and-compliance), Authentication and credential use; ordinary individual usage versus third-party product integration.
- **W02:** [Claude Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview), Get started / license and terms; supported product authentication.
- **W03:** [Binance Spot REST general information](https://developers.binance.com/en/docs/products/spot/rest-api), timeouts, 5xx, 429/418 and partial cancelReplace HTTP status behavior.
- **W04:** [Binance Spot filters](https://developers.binance.com/en/docs/products/spot/filters) and [trading endpoints](https://developers.binance.com/en/docs/catalog/core-trading-spot-trading/api/rest-api/trade); exchange-defined order constraints.
- **W05:** [NautilusTrader Binance integration](https://nautilustrader.io/docs/latest/integrations/binance/), adapter capabilities and product-specific restrictions. Documentation tracks a current version; validate the pinned release separately.
- **W06:** [NautilusTrader original repository and releases](https://github.com/nautechsystems/nautilus_trader/releases), [backtesting concepts](https://nautilustrader.io/docs/latest/concepts/backtesting/).
- **W07:** [Binance official Python connectors](https://github.com/binance/binance-connector-python), modular Spot SDK and MIT license.
- **W08:** [NATS JetStream consumers](https://docs.nats.io/learn/jetstream/pull-consumers); acknowledgements, consumer processing and redelivery controls. Transactional coupling to the financial DB is a reviewer recommendation.
- **W09:** [PostgreSQL transaction isolation](https://www.postgresql.org/docs/current/transaction-iso.html); isolation and serialization behavior.
- **W10:** Bailey, Borwein, López de Prado, Zhu, [The Probability of Backtest Overfitting](https://www.davidhbailey.com/dhbpapers/backtest-prob.pdf), original paper.
- **W11:** Bailey and López de Prado, [The Deflated Sharpe Ratio](https://www.davidhbailey.com/dhbpapers/deflated-sharpe.pdf), original paper.
- **W12:** [arch bootstrap documentation](https://arch.readthedocs.io/en/latest/bootstrap/bootstrap.html) and [original project](https://github.com/bashtage/arch).
- **W13:** [DuckDB Parquet documentation](https://duckdb.org/docs/current/data/parquet/overview) and [original project](https://github.com/duckdb/duckdb).
- **W14:** [Hypothesis stateful testing](https://hypothesis.readthedocs.io/en/latest/stateful.html) and [original project](https://github.com/HypothesisWorks/hypothesis).
- **W15:** [Toxiproxy](https://github.com/Shopify/toxiproxy), original repository.
- **W16:** [OpenAI authentication documentation](https://learn.chatgpt.com/docs/auth), API-key billing versus plan entitlements.
- **W17:** [Official ChatGPT plan usage overview](https://developers.openai.com/siwc/token-sharing-open-source), [registration/sign-in](https://developers.openai.com/siwc/token-sharing-open-source/sign-in), and [preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations). Account/project eligibility was not exercised during this audit.
- **W18:** [CCXT](https://github.com/ccxt/ccxt), original repository and releases.
- **W19:** [Freqtrade](https://github.com/freqtrade/freqtrade), original repository and releases.
- **W20:** [Hummingbot](https://github.com/hummingbot/hummingbot), [Condor](https://github.com/hummingbot/condor) and [official Condor introduction](https://hummingbot.org/condor/).
- **W21:** [cryptofeed LICENSE](https://github.com/bmoscon/cryptofeed/blob/master/LICENSE), current AGPL and attribution text.
- **W22:** [vectorbt](https://github.com/polakowo/vectorbt) and [terms](https://vectorbt.dev/terms/), Apache-2.0 with Commons Clause.
- **W23:** [statsmodels official documentation](https://www.statsmodels.org/stable/index.html) and [source](https://github.com/statsmodels/statsmodels).
- **W24:** [Optuna](https://github.com/optuna/optuna), original repository.
- **W25:** [MLflow experiment tracking](https://mlflow.org/docs/latest/ml/tracking/) and [source](https://github.com/mlflow/mlflow).
- **W26:** [OpenBao documentation](https://openbao.org/docs/what-is-openbao/) and [source](https://github.com/openbao/openbao).
- **W27:** [OpenTelemetry](https://opentelemetry.io/docs/what-is-opentelemetry/), official documentation.
- **W28:** [Prometheus](https://prometheus.io/docs/introduction/overview/), official documentation.
- **W29:** [LangGraph](https://github.com/langchain-ai/langgraph), original repository; hosted offerings are distinct from the MIT core.
- **W30:** [llama.cpp](https://github.com/ggml-org/llama.cpp), original repository; engine licensing does not license every model weight.
- **W31:** [OPA](https://github.com/open-policy-agent/opa), [NATS server](https://github.com/nats-io/nats-server), [ClickHouse](https://github.com/ClickHouse/ClickHouse), original repositories/metadata.
- **W32:** [Qlib](https://github.com/microsoft/qlib), [River](https://github.com/online-ml/river), [Riskfolio-Lib](https://github.com/dcajasn/Riskfolio-Lib), [Stable-Baselines3](https://github.com/DLR-RM/stable-baselines3), original repositories/metadata.

### Appendix 4 — Scope and residual uncertainty

All 40 files in the current tree were read. Both exposed branches were checked and their difference verified. This does not inspect every historical revision or claim that external v01 artifacts are implemented in v02. No credentials, owner's hardware, exchange account eligibility, market dataset or runtime integration was available or needed to establish the documentation findings. No source-code tests could be run because the snapshot has none. No live/paper/backtest result was fabricated.

External research is a fit and maintenance screen, not a line-by-line security audit of upstream projects. Community adoption was assessed qualitatively from established ecosystems and project scope, not independently measured deployment counts. Exact hardware sizing, performance, compatibility, costs under the owner's subscription, commercial-license obligations and account entitlements remain to be verified at the relevant spike. Proposed gates and architecture changes are explicitly recommendations; nothing was silently implemented.

## S. Final answer

1. **What should remain unchanged:** the two-mode local product, deterministic financial trust boundary, independent hard risk, permanent provider-independent agent identities, immutable research artifacts, honest staged validation, withdrawal-disabled local credentials and rejection of promised returns.
2. **What must be repaired before implementation/real trading:** specify actual quantitative methods and strategies, close atomic authorization/accounting/order-recovery contracts, define measurable validation and scoped certification, verify runtime cost/authentication, and prove a minimal simulation-to-canary pipeline before expanding capital or infrastructure.
3. **What should explicitly wait until the Global Enterprise SaaS phase:** multi-tenant customer infrastructure, broad enterprise identity/organization management, billing, commercial API operations, global compliance/support and distributed high-availability scale beyond demonstrated local needs.
