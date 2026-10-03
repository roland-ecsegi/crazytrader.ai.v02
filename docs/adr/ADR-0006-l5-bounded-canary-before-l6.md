# ADR-0006 — L5 bounded canary is the sole pre-L6 real-money exception

## Status

Accepted.

## Context

The baseline contained a contradiction: one invariant said real-money mode was impossible before L6, while the certification ladder requires actual owner-local L5 canary trades as evidence needed to reach L6.

Treating L5 canary as ordinary LIVE mode would weaken the certification boundary; forbidding all real-money activity before L6 would make L6 unattainable.

## Decision

General LIVE trading authority remains impossible before L6 LIVE CERTIFIED and explicit owner activation.

The sole pre-L6 real-money exception is an L5 bounded canary that:

- runs only on the owner-controlled Enterprise Local deployment;
- requires explicit owner authorization;
- uses withdrawal-disabled Binance credentials held only in the local secret store;
- has a tiny owner-defined hard capital cap and dedicated canary policy;
- requires healthy deterministic Hard Risk, OPA, reconciliation, duplicate-order protection, kill switches and monitoring;
- cannot grant or imply unrestricted/general LIVE authority;
- has explicit stop/rollback criteria;
- preserves full audit, ledger and reconciliation evidence.

Codex Cloud never receives live credentials and never executes the canary.

## Consequences

L5 can collect genuine real-money evidence without collapsing the L6 boundary. Any implementation that treats L5 as equivalent to unrestricted LIVE is non-compliant.
