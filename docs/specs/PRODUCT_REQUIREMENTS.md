# Enterprise Local product acceptance contract

Status: DEFINED target; implementation MISSING. This file fixes product scope, not readiness.

## Current target
One owner, one private installation, Binance Spot long-only with no borrowing, margin, derivatives or withdrawals. The finished product must operate end-to-end without a SaaS deployment: acquisition of data, research, both trading modes, risk, execution, accounting, agents, UI, operations and evidence-gated real trading. A single host is an accepted availability limit, not permission to omit recovery or baseline security.

Owner intent is to use a validated system to seek returns and potentially finance future development. Revenue, profitability, a monthly return and a minimum trade count are not acceptance criteria and cannot be promised. Cash/no-trade is a valid outcome. An economically rejected strategy must not be activated to satisfy the product roadmap.

## Required capabilities and evidence

| Capability | Required local behavior | Acceptance evidence |
|---|---|---|
| Two modes | Independent Math and Strategy allocation, P&L, drawdown and Auto Trading controls; shared account caps | Concurrent-mode ledger, reservation and attribution tests |
| Quantitative operation | Executable versioned method in each mode, reproducible signals, cost-aware sizing and exits | QUANTITATIVE_METHODS protocol and stage evidence; original repository contained no concrete method |
| Real execution | Supported certified Spot orders, fills, cancellation and restart reconciliation | Scoped L1–L6 gates; bounded local L5 canary and explicit L6 activation |
| Permanent agents | Eleven identities, real typed skills, durable task state, memory provenance, evaluations | AGENT_RUNTIME and LOKI acceptance suites |
| Claude subscription | Owner's official unmodified Claude Code CLI using native login; no automatic paid fallback | CLAUDE_SUBSCRIPTION_SPIKE on the actual owner machine and plan |
| Local calculations | Market ingestion, features, strategies, risk, ledger and monitoring work without LLM inference | AI outage test with orders/positions and deterministic protection |
| Recovery | Safe startup, kill, backups, restore, audit and single-executor fencing | Failure drills with measured recovery/data-loss outcomes |
| Usable UI | Honest mode/agent/health/certification states, Loki guidance, evidence links | End-to-end owner journeys including rejection, outage and recovery |

## Permanent does not mean continuously reasoning
Mathematical monitors and signal rules run locally on events/timers. They use CPU, RAM, disk, power, market-data bandwidth and venue API quotas, but do not consume Claude tokens. Qualified events enqueue bounded tasks. LLM tokens/plan allowance are used only when a task actually invokes Claude. Eleven agents share the owner's plan limits; they are not eleven independent subscriptions. Waiting/idle agents retain identity and state without model calls.

## Three separate readiness judgments
1. **Engineering readiness:** financial, operational and security controls pass scoped tests.
2. **Economic eligibility:** a specific strategy/model survives the preregistered research and forward-validation process. INSUFFICIENT EVIDENCE or rejected candidates are normal results.
3. **Product acceptance:** both modes, Loki and other agent skills, supported subscription operation, UI and required local operational evidence work together.

Do not mark full Enterprise Local complete when one of these is missing. A sound engine can exist with no eligible profitable strategy. A good backtest cannot compensate for an unsafe engine.

## Current/future boundary
Local security, secret isolation, browser protection, backups, correctness and observability are required NOW. Future SaaS is more than adding users, security and paid APIs: it needs tenant isolation, identity/organizations, billing, commercial licensing, operations, compliance and service economics. Carry owner/account IDs and provider/venue interfaces now; do not build those SaaS systems now.

## Requirement changes
Ordinary implementation choices need no repeated approval. Changing capital, live activation, supported trading product, commercial spend, secret access or a binding product promise requires the applicable explicit owner action. A missing subscription capability is a recorded product blocker, not permission to silently substitute paid API usage.
