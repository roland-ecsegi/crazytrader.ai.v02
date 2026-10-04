# ADR-0003 — Risk Direction and Emergency Reduction

Status: Accepted

## Context

A fail-closed system that denies every order during degraded state can accidentally trap an open position.

## Decision

TradeIntent proposes RISK_INCREASING, RISK_REDUCING or RISK_NEUTRAL; Hard Risk independently recomputes direction from current and worst-case post-action state (2026-10-04 refinement, ADR-0009).

Unknown/degraded safety state denies new risk by default while preserving a deterministic, policy-governed path for cancellation and exposure reduction where venue state permits.

## Consequences

Risk/OPA/execution tests must separately validate increase and reduction behavior. Global Kill policy must specify cancel-only vs flatten-approved semantics.


## 2026-10-04 clarification

ADR-0009 and the RISK_SECURITY dependency matrix define emergency prerequisites and physical limits. This ADR never promises execution through unavailable venue/network, missing credentials, ambiguous holdings or absent durable journal.
