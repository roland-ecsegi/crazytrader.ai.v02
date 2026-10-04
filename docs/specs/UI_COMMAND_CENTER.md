# Custom Command Center and owner workflows

Status: DEFINED UI behavior; implementation MISSING. The product UI is custom; internal observability/research tools can supplement it.

## Dashboard and safety

Show actual environment (simulation/paper/shadow/canary/live), build/certification scope, owner activation, each mode's Auto Trading state, capital/P&L/drawdown/fees/holds, positions/orders, data/venue/reconciliation age, incidents, backup health and Global Kill. Show UNKNOWN, stale and unavailable states distinctly; never turn missing data into healthy/zero.

Global Kill is one authenticated immediate action to stop new risk. Clearly separate cancel-only and pre-authorized flatten requests, remaining in-flight orders/dust and venue failures. Clearing Kill, increasing capital and activating live require explicit confirmation showing scope/caps/evidence and backend policy enforcement. Pause retains protection/reconciliation. UI cannot call a raw exchange SDK.

## Loki

Persistent guide panel with stable identity, idle/running/waiting status, selected model, last observation time and linked evidence. Explain rejected/no-trade decisions and next permissible steps. Read-only navigation/help is direct; protected action proposals use the normal owner workflow. Show provider quota/unavailable errors and deterministic fallback help honestly. See LOKI for retrieval and evaluation contract.

## Agent and research pages

All eleven identities with role, allowed skills, permission summary, task state, provenance-aware memory metadata, evaluations and audit. Show shared plan quota state, invocation counts and usage estimates when available; don't invent remaining tokens or a precise subscription cost per agent. Default UI provider route is subscription-only; paid options are disabled unless explicitly configured by owner.

Research pages show full experiment registry, rejected candidates, trial count, data/split/cost lineage, uncertainty, effective sample size, lifecycle and economic eligibility separate from engineering certification. Learning views include abstention/rejection observations, not just winning executed trades.

## Required owner journeys

Configure simulated budgets; inspect both candidate modes; run/reproduce research; start paper; trace a signal through risk/ledger; see an expired/rejected/unknown order; pause without losing protection; ask Loki for grounded explanation; observe quota outage; trigger/clear Kill; inspect restore drill; verify local subscription integration; complete scoped canary/live activation. Every step must show what is actually implemented and certified.

No raw secrets, unrestricted SQL/shell, agent permission self-edit or hidden “force live” control. API validates auth, schema, current state, idempotency and permissions independently of UI confirmations. Accessibility and keyboard access to critical controls are acceptance requirements.
