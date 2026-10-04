# ADR-0008 — Event-driven permanent agents, Loki and Claude subscription boundary

Status: ACCEPTED design, 2026-10-04. Implementation and owner-local provider compatibility pending. Preserves ADR-0002.

## Decision
Keep the ten existing permanent responsibilities and add Loki as the eleventh, user-facing guide. Identities, task history, permissions and memory persist independently of processes, prompts or models. A durable scheduler invokes real skills only for qualified events or user tasks. No continuous LLM polling or cross-agent conversation loops.

Claude Code's official unmodified CLI with owner-native login is the intended Enterprise Local AI route. An application adapter supervises the process; it does not extract credentials or impersonate an API using subscription tokens. The owner-local spike is a hard dependency for acceptance of this requirement. API/SDK provider adapters and local models remain explicit optional alternatives, not silent replacements or mandatory paid dependencies.

Loki retrieves versioned product knowledge and permitted actual runtime state. It explains and guides with citations and freshness, and submits typed task/command proposals. It cannot trade, change hard limits, promote models, retrieve secrets or grant privileges. Choose the smallest available subscription model that passes the task evaluations; do not hard-code a claim that Haiku is always available or cheapest under a flat plan.

## Consequences
Idle agents consume no inference. Qualified tasks consume plan allowance and can hit quotas. Deterministic risk, execution, accounting and protection remain operational through AI outages. No promised unlimited subscription automation, uptime or zero operating cost. Terms/model compatibility are checked at integration and upgrades.
