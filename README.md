# CrazyTrader.ai v02

Enterprise Local crypto trading application under specification. **Current status: L0 DEVELOPMENT; documentation exists, executable application and trading evidence do not.** This is not currently simulation-, paper- or live-ready.

## Intended complete product

One operator, Binance Spot long-only, two independently controlled/accounted modes, eleven permanent event-driven agents including Loki, custom UI, research and controlled learning, deterministic financial safety, recovery and evidence-gated real trading. Global SaaS is a separate future product phase; it is not required for eventual owner-local live use.

Math Mode's authoritative signal is quantitative code; Strategy Mode uses immutable validated strategies and LOW/MEDIUM/HIGH parameter profiles. The first two fully described research candidates are in QUANTITATIVE_METHODS; neither is validated or claimed profitable.

Local data processing, mathematical signals, risk, execution and accounting use ordinary local software and venue APIs, not LLM tokens. Qualified tasks invoke Claude; idle permanent agents do not. The intended AI route is the owner's official unmodified Claude Code CLI through native subscription login, subject to a real local compatibility/terms/quota test. No subscription-token proxy or automatic paid API fallback.

## Financial principle

AI researches and explains. Deterministic models propose. Hard Risk constrains. OPA authorizes. Atomic reservations protect capital. One sender executes. Venue facts are reconciled. A balanced immutable ledger records. Learning creates reviewed candidates.

No guaranteed returns, monthly target or required number of trades. Zero trades is valid. A well-built engine does not prove economic edge. L5 bounded owner-authorized local canary is the sole pre-L6 real-capital exception; general LIVE requires scoped L6 evidence and explicit owner activation.

## Reading order

1. [Repository invariants](AGENTS.md) and [ExecPlan standard](.agent/PLANS.md).
2. [Product acceptance](docs/specs/PRODUCT_REQUIREMENTS.md), [architecture](docs/architecture/ARCHITECTURE_V1.md) and [reuse decisions](docs/architecture/OPEN_SOURCE_ADOPTION.md).
3. [Quantitative candidates](docs/specs/QUANTITATIVE_METHODS.md), [trading modes](docs/specs/TRADING_MODES.md) and [market data](docs/specs/MARKET_DATA.md).
4. [Atomic financial authority](docs/specs/FINANCIAL_AUTHORIZATION.md), [execution](docs/specs/EXECUTION_AND_RECONCILIATION.md), [risk/security](docs/specs/RISK_SECURITY.md) and [ledger](docs/specs/DATA_AND_LEDGER.md).
5. [Agents](docs/specs/AGENTS_AND_SKILLS.md), [durable runtime](docs/specs/AGENT_RUNTIME.md), [Loki](docs/specs/LOKI.md), [AI routing](docs/specs/AI_PROVIDER_ROUTING.md) and [subscription spike](docs/specs/CLAUDE_SUBSCRIPTION_SPIKE.md).
6. [Validation gates](docs/specs/TESTING_AND_CERTIFICATION.md), [local operations](docs/specs/LOCAL_DEPLOYMENT.md) and [UI](docs/specs/UI_COMMAND_CENTER.md).
7. [Roadmap](docs/roadmap/IMPLEMENTATION_ROADMAP.md), [task dependencies](docs/roadmap/CODEX_TASK_GRAPH.md), [status](docs/program/STATUS.md) and [known gaps](docs/program/KNOWN_ISSUES.md).
8. [Independent audit](docs/audit/INDEPENDENT_AUDIT_2026-10-04.md) and [documentation change report](docs/audit/DOCUMENTATION_REMEDIATION_2026-10-04.md).

## Current checkpoint

2026-10-04 documentation remediation refines the original 40-file baseline and records accepted ADR-0007–0009. It does not implement services, schemas, strategies, agents, tests or deployment. Start future implementation explicitly using [start.md](start.md); no development program is implicitly launched by this documentation task.

Security is required now: withdrawal-disabled local keys, authenticated control interface, isolated agents/research, no live secrets in cloud, backups and tested recovery. “Local enterprise” does not mean postponing these protections until SaaS.
