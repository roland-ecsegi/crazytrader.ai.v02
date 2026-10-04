# Durable event-driven agent runtime

Status: DEFINED architecture; scheduler, worker and task schemas MISSING.

## Trigger path

Market events -> local deterministic features/quality/risk monitors -> qualified domain event -> policy-filtered task enqueue -> bounded agent worker -> typed skill/result validation -> durable result/audit -> optional owner explanation or research workflow. Financial protection proceeds independently and never waits for this path.

An hourly strategy opportunity does not require eleven conversations. Route only to the responsibility that benefits from reasoning. For example, a spread threshold blocks trading deterministically; repeated abnormal spreads can produce one Execution Supervisor incident task; Loki explains it when asked or as one bounded notification.

## AgentTask contract

Fields: task_id, agent_id, task_type/version, trigger_event_id, dedup_key, priority, input_refs/hashes, allowed_skill_versions, permission_snapshot, created_at, not_before, expires_at, deadline, budget_id, max_attempts, attempt_count, lease_owner, lease_epoch, lease_expires_at, checkpoint_ref, output_ref, error_class, trace_id and parent_task_id. Inputs are bounded and versioned, not the entire repository/account history on every call.

States: QUEUED -> CLAIMED -> RUNNING -> SUCCEEDED / FAILED / CANCELLED / EXPIRED; WAITING_FOR_DEPENDENCY, WAITING_FOR_PROVIDER and RETRY_SCHEDULED are explicit durable states. Claims are atomic; expired leases permit recovery of a task attempt, not repetition of completed side effects. Store skill invocation IDs and completion receipts. A crashed task resumes from a checkpoint or reruns only idempotent steps. Invalid/malicious outputs fail closed.

## Limits and admission

Initial conservative local defaults, tunable after measurement: one concurrent LLM invocation for the whole subscription profile; at most 100 queued LLM tasks; maximum two attempts for transient provider failure; 120-second task deadline unless an approved research job declares another bound; per-agent incident cooldown of 15 minutes and 60-second aggregation window for repeated identical noncritical events. These are operational starting values, not guaranteed plan capacity or market safety thresholds.

Critical protection never uses these delayed queues. Severe incidents open immediately in deterministic monitoring and local UI/alerting; the AI explanation may wait. Owner-interactive Loki tasks have priority over routine research without starving essential operator notifications. Expire stale research/opportunity tasks instead of replaying them hours later.

Dedup key includes event class, affected entity, regime/config version and incident window. Escalating severity or a changed incident must bypass coalescing suppression. Bound DAG depth and task fan-out; default max delegation depth 2 and no cycles. Dead-letter terminal failures with cause/evidence. Retries honor provider backoff and total task deadline; no repeated login or quota probing loop.

## Token and spend policy

Track per-task and per-agent calls, input/output/cached-token estimates when available, wall time, model, context size, retries, quota denials and actual billing evidence where available. Subscription tokens are allowance use, not automatically a dollar invoice. A local estimated budget stops admission before invoking; provider accounting remains authoritative. No exact remaining-plan-token count is fabricated if unavailable.

Subscription-only profile has external paid-inference budget 0 and no paid fallback credentials/routes. Disable provider extra-usage billing in the owner's supported settings and validate the actual auth/model route; local budgets alone cannot enforce billing inside a provider. If zero-extra-cost routing cannot be established, pause AI tasks and record the blocker. Non-AI CPU/storage/venue fees remain distinct costs.

## Outages and correctness

Provider unavailable/quota exhausted -> WAITING_FOR_PROVIDER or explicit terminal expiry, visible to owner. Deterministic certified trading/protection continues according to existing policy. No new financial permission is created to compensate for an unavailable reviewer. Restart restores tasks and agent identity; an idle state does not invoke Claude.

Audit task inputs by reference, source retrieval, skill versions, model/provider, validated output, tool receipts and decision outcomes. Redact private data; do not store raw secrets or unlimited transcripts. LLM private reasoning traces are not required for audit; inspect observable inputs, actions, rationale/evidence and outputs.

## Acceptance

Prove 24-hour idle synthetic-clock test yields zero inference calls; duplicate-event burst yields bounded one-per-dedup-group work; restart at each skill boundary does not duplicate side effects; quota/timeout tests stop retries; revoked permission blocks a leased task at invocation; incident escalation survives coalescing; trading safety works with runtime completely stopped. Real elapsed provider/market observation is recorded separately from accelerated tests.
