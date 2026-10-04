# Trading modes

Status: DEFINED product behavior; no implemented or validated strategies yet. PRODUCT_REQUIREMENTS governs acceptance; QUANTITATIVE_METHODS defines the first two unvalidated research candidates.

## Math Mode

Point-in-time validated data -> versioned features -> frozen quantitative model -> gross return estimate -> execution-size-aware cost estimate -> net edge/uncertainty evidence -> deterministic candidate decision -> capped sizing -> TradeIntent -> independent Hard Risk/OPA/reservation -> execution -> reconciliation/ledger.

MATH-RIDGE-01 is the initial research baseline. It does not yet establish predictive edge. LLMs may propose hypotheses, interpret evidence and explain; they never calculate authoritative live edge by improvisation. No eligible signal is a normal result.

## Strategy Mode

Point-in-time market state -> deterministic regime/entry predicates -> eligible immutable strategy/profile -> declared exit/sizing rules -> TradeIntent -> same financial boundary. STRATEGY-DONCHIAN-01 is the first research baseline. A strategy library may expand after evidence; no other trading algorithms are implied as implemented.

LOW/MEDIUM/HIGH are versioned numeric limits/selectivity settings, not verbal confidence. Risk/profile semantics live in RISK_SECURITY. High never overrides absolute caps, data quality or certification.

## Independent control, shared account

Each mode has its own allocation, realized/unrealized P&L, costs, cash-flow-adjusted return/drawdown, inventory ownership, audit and Auto Trading toggle. Both share account-level reservations, venue limits and risk caps. Cross-mode transfers are explicit balanced ledger operations.

Start enables only certified eligible opportunities within preauthorized settings. Pause/Auto Trading OFF prevents new exposure but maintains deterministic order/position protection, accounting and reconciliation. Stop has a declared cancel/hold or authorized flatten policy; it cannot silently abandon positions. Global Kill applies across both modes without Claude.

Math evidence may inform strategy research, but no duplicate economic position is created by routing the same candidate through both modes unnoticed. Attribute every proposal/fill to one origin and aggregate correlated exposure. Owner manual trades also pass risk/policy and have explicit accounting ownership.

Financial correctness is necessary but does not prove economic viability. Neither mode is required to trade when its candidate has no defensible net edge.
