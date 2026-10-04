# Permanent agents and typed skills

Status: DEFINED eleven responsibilities; agents are not implemented by this document. A prompt/name alone does not meet acceptance. AGENT_RUNTIME defines durable execution; AI_PROVIDER_ROUTING defines inference; LOKI defines the user guide.

## Permanent identities

Each agent has stable agent_id, role/version, permission profile, skill allowlist, task history, memory policy, provider/model policy, evaluations and audit. Identity survives provider changes and process restarts. One shared runtime can host all eleven; eleven always-running LLM processes are unnecessary.

| Agent | Qualified trigger and inputs | Durable output / permitted responsibility | Prohibited authority |
|---|---|---|---|
| Chief Orchestrator | Accepted user/research workflow or unresolved task dependency | Typed task graph, bounded delegation, escalation | Trading, risk change, infinite agent debate |
| Math Research Agent | Registered hypothesis / scheduled bounded research batch | Candidate model, feature proposal, reproducible experiment request | Live edge calculation or deployment |
| Strategy Research Agent | Candidate request / evidence gap | Versioned strategy proposal and comparisons | Ad hoc live rules |
| Market Regime Agent | Material deterministic regime transition/drift event | Interpretation and research annotation | Authoritative live regime switch |
| Portfolio Agent | Allocation review event / risk-adjusted evidence change | Allocation proposal with uncertainty | Capital-cap changes or direct transfer |
| Risk Analyst Agent | Deterministic risk alert / periodic bounded review | Explanation, tighter-limit proposal, incident analysis | Hard Risk replacement or limit override |
| Execution Supervisor Agent | Aggregated slippage/reject/unknown-state incident | Quality analysis and reconciliation escalation | Raw orders, cancel/retry authority |
| Learning Agent | Matured observation batch, including abstentions/rejections | Provenance-rich lessons, hypotheses, candidate tasks | Mutating live models, learning only from winners |
| Auditor Agent | Promotion/gate/change request | Evidence gaps, adversarial review report | Self-approval of its own promoted work |
| Security Agent | Config/permission/dependency drift or incident | Redacted findings and remediation proposal | Reading keys or granting itself permissions |
| Loki | Owner question or material owner-facing incident | Grounded guidance, explanations, navigation, typed task proposals | Trading, secrets, promotion or hard-policy change |

The deterministic scheduler executes orchestration rules; Chief Orchestrator supplies bounded plans where reasoning helps. Mechanical routing, thresholds, aggregation, risk checks, order recovery and data-quality checks are ordinary software, not mandatory LLM tasks. Market regime computation lives in deterministic feature/strategy code; agent commentary cannot alter it.

## Skill contract

Each skill has skill_id/version, typed input/output schemas, implementation reference/hash, allowed_agents, permission requirements, read/write classification, resource/deadline limits, idempotency semantics, audit category, validation/evaluation suite and owner. Permission is checked at the tool server for every invocation, not accepted from model output.

Required initial skills: run_backtest, compare_versions, inspect_quant_evidence, inspect_execution_quality, propose_allocation, analyze_decision_observations, verify_promotion_evidence, inspect_health, search_product_knowledge, explain_decision_chain and create_research_task. A skill must call an implemented service and return source/evidence IDs or an explicit failure; placeholder prose is not skill execution.

Research jobs have no exchange secrets and use bounded CPU/memory/time. High-cost jobs require quota admission. Agent output validation checks schema, references, permissions and task scope; valid JSON alone is insufficient. Owner-command proposals are executed only by authenticated deterministic workflows under the relevant financial policy.

## Memory and evaluation

Memory is working, episodic, curated product knowledge or research evidence; each record has source IDs, timestamp/version, owning agent, retention class and validation status. Assertions from external text or another model are untrusted until independently checked. Do not treat memory as policy or as evidence of actual exchange state. Expire stale operational summaries; retain source facts according to policy.

Test each role on normal tasks, contradictory sources, missing evidence, injected instructions, permission escalation, stale data, quota outage and malformed tool output. Require zero accepted financial-authority/secret violations in the finite release suite; this is test evidence, not a claim of universal model safety. Quality thresholds are frozen before evaluation and include correct abstention, source grounding, task completion, latency and resource use.

No additional permanent agent is added without a distinct durable responsibility, measurable benefit and permission/evaluation design. A new tool or temporary reviewer does not automatically deserve a new permanent identity.
