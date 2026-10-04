# TradeIntent contract

Status: DEFINED v1 target; not an executable schema. TradeIntent is a proposal, never an exchange order or authorization. All math, strategy, owner-manual and rebalance actions use it.

## Required fields

intent_id, schema_version, owner_id/tenant_id, portfolio_id, exchange_account_id, mode, source_type, source_id, strategy_version_id where relevant, model_version_ids, risk_profile_version_id, capital_budget_ref, instrument_id, side, intent_type, proposed_risk_effect, exactly one authoritative requested_quantity OR requested_notional, sizing_method_version, expected_horizon, max_slippage_fraction, reason_code, evidence_refs, market_state_ref, portfolio_state_ref, created_at, expires_at, trace_id. Decimal financial fields serialize as canonical decimal strings with explicit units; timestamps are UTC with precision.

Optional typed evidence includes expected_gross_return_fraction, estimated_round_trip_cost_fraction, expected_net_return_fraction and uncertainty_ref. `confidence` is not an unexplained 0–1 score: if present it must identify the estimated event, horizon, estimator, calibration dataset and calibration metrics. Otherwise omit it. Missing/unavailable evidence is explicit, never zero-filled or invented by an LLM.

source_type is math_engine, strategy_engine, owner_manual or portfolio_rebalance. source_id identifies the deterministic generating component or authenticated owner, not a research agent. An agent proposal enters governed research or an owner command workflow; it cannot label itself math_engine.

intent_type is OPEN, INCREASE, REDUCE, CLOSE or REBALANCE. Exchange cancellation is a typed control command referencing an existing order; it does not invent new exposure. Initial certified scope may support fewer types; unsupported types are rejected explicitly.

## Units and sizing

Notional is in the instrument quote asset; quantity is base units. Returns, fees and max_slippage_fraction are dimensionless fractions, not percentages or basis points unless explicitly converted. Evidence includes reference price/time, price source, data quality, feature/config/cost hashes and rounding rules. The final risk engine recomputes permitted quantity and risk effect; it may reduce size, never increase an approved bound silently. A change requiring a new economic decision creates a new intent.

For a BUY, approved quote/fee bound is reserved conservatively; for a SELL, owned attributable base is reserved. Quant libraries may use finite float64 internally; the money boundary uses Decimal with conservative rounding and fresh venue filters. NaN/Inf and unavailable conversion rates reject proposals.

## Authorization and lifecycle

schema -> scoped certification -> atomic portfolio/Hard Risk/OPA authorization and reservation -> fenced execution claim -> final freshness/expiry/filter/latch check -> durable SUBMITTING -> venue. FINANCIAL_AUTHORIZATION is authoritative for transaction boundaries.

A rejected/expired intent is immutable. A new economic attempt uses a new intent linked to its predecessor. Transport retries and lost acknowledgments belong to the existing OrderAction and never create a replacement intent merely to escape UNKNOWN. Absence proof and action retry are governed by EXECUTION_AND_RECONCILIATION.

## Example without invented confidence

A research fixture may request MATH BUY, requested_notional="40.00" USDT, expected_horizon="PT4H", max_slippage_fraction="0.001", and reason_code="RIDGE_NET_EDGE_ACCEPTED" with evidence hashes. It is not live-ready without complete canonical fields, certified model/cost data, hard limits and current reservation. Examples are not permissive defaults.

Owner manual requests pass the same financial controls. Owner changes to limits are versioned and audited; no direct bypass exists. Emergency reductions use the narrowly scoped deterministic policy in RISK_SECURITY, not this example.
