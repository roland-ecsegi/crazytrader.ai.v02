# Deterministic risk and local security

Status: DEFINED design; numerical owner live configuration and executable controls MISSING. Hard Risk is deterministic and independent of Risk Analyst Agent. OPA authorizes after Hard Risk; neither is replaced by LLM advice.

## Risk state and measurements

Account and per-mode equity are marked in a declared reporting asset using fresh conservative executable-price references. Net equity includes owned inventory and cash less applicable liabilities/fees; reservations do not reduce equity a second time. Report realized P&L, unrealized P&L, costs and external cash flows separately. Cash flow is never trading return.

Maintain unitized/flow-adjusted equity for performance and high-water drawdown, plus actual currency equity for solvency/caps. For a no-cash-flow interval, drawdown_fraction = 1 - equity/high_water_equity, high_water_equity > 0. Deposits/withdrawals reset units through explicit flow accounting, not by hiding losses. Daily and rolling-week loss calculations include realized and unrealized changes net of flows; timezone/cutoff is fixed UTC and recorded. Overnight holdings do not escape limits at midnight.

## Required versioned limits

Owner-configured live limits have no permissive default. Before activation require positive account/mode/strategy/symbol caps, cash reserve, maximum active/unknown orders, maximum position count, per-order notional, per-trade planned loss, account/mode daily and rolling-week loss, maximum drawdown, spread/slippage bounds, liquidity participation, stale-data/clock/reconciliation tolerances and cancel/retry rates. Validate dimensions, cross-field constraints and exchange minimums. Missing configuration blocks activation.

Initial product has zero borrowing/leverage authority and no liquidation model because it is Spot-only. Fees, asset collapse, gap risk and exchange/custody loss remain possible. Adding margin or futures requires a separate specification and certification, not a profile toggle.

Exposure includes inventory plus worst-case remaining buys/sells and UNKNOWN orders. Apply account-wide and per-mode limits atomically. Treat crypto assets as correlated under stress; version a conservative group cap and correlation stress matrix. Estimate correlations with uncertainty and missing-history fallbacks; absence of estimates cannot unlock more exposure. Concentration limits include shared quote/stablecoin and venue concentration where meaningful.

Per-trade stop-distance sizing is a planned loss budget, not a guaranteed maximum realized loss. Gaps/illiquidity and operational failures can exceed it. Reserve additional fee/slippage allowance and stress portfolio loss rather than trusting stops alone.

## Profiles and direction

LOW/MEDIUM/HIGH are immutable parameter profiles with explicit numeric versions before use. For identical data, LOW permitted size/concentration must not exceed MEDIUM, and MEDIUM must not exceed HIGH; all remain under absolute caps. Selectivity thresholds must be monotonic in the opposite direction. Profiles do not alter certification or permissions.

Recompute RISK_INCREASING / RISK_REDUCING / RISK_NEUTRAL from current and worst-case post-action holdings, holds and asset exposure. A SELL label is not proof of reduction. Reject oversells; Spot has no generic venue reduce-only guarantee. Currency conversion, rebalancing or closing one asset to open another can increase another risk dimension. Unknown direction denies new exposure.

## Latches and kill behavior

Loss/drawdown breaches, stale data, mismatch and invalid scope latch new risk off at affected scope. Automatic reset requires an explicit documented rule and healthy evidence; financial loss/Kill latches require an authenticated owner acknowledgment and current revalidation. Restart, day boundary or model change cannot clear them silently.

Global Kill immediately latches new risk off without waiting for an LLM or modal confirmation. Separate authenticated actions request Cancel Open Orders and, if pre-authorized, Flatten Available Inventory. UI clearly states cancel-only versus flatten, unsupported dust, unavailable venue and already in-flight actions. No kill claims instantaneous or guaranteed liquidation. Clear Kill and live activation are distinct commands.

## Degraded dependency matrix

| Failure | New/increased risk | Protective action allowed | Limit |
|---|---|---|---|
| Claude/agent unavailable | Only independently certified deterministic opportunities under existing policy | Deterministic protection, reconciliation and Kill continue | No AI approval requirement is invented |
| Stale quotes/gap | Block affected market | Cancel; sell only with certified fresh price/state and bounded policy | Blind pricing is forbidden |
| Hard Risk or OPA unavailable | Block | Preloaded signed/versioned emergency policy may cancel or reduce confirmed owned inventory | Emergency evaluator must be independent, deterministic, tested and tightly capped |
| Secret store unavailable | Block new activation; block increases by default | Existing execution process may use already-loaded valid key within approved emergency policy | No recovered key means no signed action |
| PostgreSQL unavailable | Block | Emergency action only with trustworthy recent position/hold evidence, current venue query, exclusive sender and fsynced local emergency journal | Journal/state unavailable means no order; cancel path also needs recorded identity |
| Ledger corruption/ambiguous holdings | Block | Verified cancellation; sell only independently established safe free quantity within policy | Never infer ownership from corrupt state |
| Exchange/network unavailable | Block | Retry bounded queries/cancel when connectivity returns | Cannot promise flattening; alert owner for venue-side response |
| Host power loss | No local action | Previously confirmed native exchange protection, if separately supported | Local stops and UI are unavailable |

Emergency journal records action IDs, policy/config hash, observations and resulting venue facts, then imports idempotently after recovery. It cannot create new budgets or clear latches. If this path is not implemented and tested, advertise it as unavailable and block live acceptance requiring it. This refines “never traps capital” into a physically achievable safety contract.

## Local security required now

Bind UI/control to loopback by default; authenticated sessions, origin/host validation, CSRF protection for cookie-authenticated mutations and WebSocket origin checks. Remote access requires explicitly configured TLS/secure tunnel and authentication; no WAN-exposed data or policy stores. Browser pages, Loki and research never possess exchange secrets.

Use OS keyring or a vetted local secret facility; OpenBao is optional if its operational burden is justified. Inject read/trade-only, withdrawal-disabled live key only into the owner-local execution boundary, with venue-supported IP restrictions where feasible. Test/live and Claude auth contexts are separate. Fail if withdrawal capability or environment cannot be verified before live.

Research/agent code runs under separate OS/container permissions with no production artifact writes or live key mounts. Deployed core, policy bundles and promoted artifacts are immutable to agents. Deny unnecessary network/shell access; tool permissions are enforced server-side, not by prompts. Redact logs and snapshots, pin dependencies, scan secrets/vulnerabilities and generate SBOM before live. Backup secrets independently with owner-controlled recovery; never put them in fixtures or Codex Cloud.

General LIVE requires scoped L6 plus owner activation; L5 canary requires its distinct owner authorization and strict capital/capability limits. No manual trade, skill, hook or emergency path bypasses these scope rules for new risk.
