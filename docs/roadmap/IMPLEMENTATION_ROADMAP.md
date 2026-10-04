# Evidence-gated Enterprise Local implementation roadmap

Revision 2026-10-04. Current task completed only documentation remediation; application remains L0. A future explicit implementation launch follows CODEX_MASTER_PROMPT. Requirements live in PRODUCT_REQUIREMENTS; detailed dependencies in CODEX_TASK_GRAPH. No phase is passed by existing prose.

## P0 — Resolve architecture, mathematics and safety foundations

Tasks T001–T006 after T000 documentation checkpoint. Bootstrap reproducible Python/core and TypeScript/UI toolchains only as justified; executable versioned contracts and CI, secret scan and isolated environments. Implement independent math fixtures and preregister both baseline experiments. Run Nautilus/official SDK ownership/license spike, PostgreSQL transaction/outbox/ledger prototype and early Claude subscription adapter spike. Cloud mocks can proceed while owner-local native login is pending.

Exit: selected single order authority; executable money/state contracts and independent fixtures; accepted dependency versions/licenses; no ambiguity in capital reservation/retry/emergency scope; local provider requirement either demonstrated or explicitly blocked. No live credentials/order route in cloud. Documentation defines semantics but does not replace these prototypes/tests.

## P1 — Correct historical research and exchange simulation

Depends on relevant P0 contracts/spikes. Tasks T007–T015: point-in-time data/manifests, actual balanced ledger/holds, deterministic risk and OPA, selected engine adapter/fake venue, reconciliation, shared feature/signal code, candidate research, chronological/robustness tests and L1/L2 gates. Instrument and build security/recovery tests as components appear. Build minimal UI/control/health views early enough to exercise the real workflow.

Exit: reproducible datasets/trials, no leakage fixture failures, realistic cost/latency limitations explicit; all simulation money invariants pass; research can accept/reject candidates honestly. Both unvalidated algorithms are research inputs, not mandatory winners.

## P2 — Usable paper and shadow application

Depends on L1/L2 and relevant provider/UI foundations. Tasks T016–T023: real-time feeds/paper adapter, durable eleven-agent runtime/skills, Loki retrieval, controlled learning from all observations, complete UI journeys, local deployment/backups/alerts and real elapsed paper/shadow evidence. Claude integration is scoped to actual owner plan/model/config; quota outages degrade advisory tasks visibly.

Exit: L3/L4 measured gates, real agent skills/task recovery and Loki evaluations, owner-local subscription evidence for requested AI acceptance, no critical open safety findings. Paper/shadow observations do not validate actual fills or future profitability.

## P3 — Controlled real-capital trading

Tasks T024–T026 after all applicable engineering, economic and operations gates. Owner alone enters withdrawal-disabled keys locally, declares small canary capital/stop/ramp policy and explicitly activates L5. Run real canary under its separate certificate; reconcile actual fees/fills/slippage and investigate deviations. No forced orders to meet a timetable.

Exit: actual scoped L5 evidence, no unresolved BLOCKER/CRITICAL and passed L6 criteria; explicit owner activation for general LIVE within certified scope. Missing keys, owner action, eligible candidate or sufficient market evidence produces the specific blocker, not fabricated completion.

## P4 — Mature Enterprise Local acceptance

Tasks T027–T030 consolidate sustained operations, upgrade/rollback/restore drills, resource/retention tuning, all required agent/UI workflows, tested capital allocation/ramp and final adversarial review. These controls are developed earlier; P4 demonstrates the complete product, not the first time backups/security are considered.

Exit: complete PRODUCT_REQUIREMENTS acceptance, actual L6 scope/evidence, both modes usable with their validated eligible artifacts, required subscription/Loki workflows working, runbooks/supportable host and measured limitations. If one method has no defensible eligible strategy, retain that honest research result and keep full two-mode live acceptance pending.

## FUTURE — Global SaaS only

Real tenancy/tenant isolation, identity/organizations/RBAC/SSO, billing/metering, commercial distribution/legal terms, broader compliance, global reliability/support and justified scaling. Paid AI API infrastructure is a commercial option, not a missing local safety feature. No Kubernetes, Kafka, service mesh, global HA or large security operations project without measured SaaS requirements.

## Priority and change discipline

MUST: financial authority/accounting, scientific protocol, data causality, security, recovery, scoped certification and required user product behavior. SHOULD: mature component reuse, invariant/property testing and measured local observability. COULD: local inference, optional optimizers and extra analytics stores after evidence. DO NOT DO NOW: SaaS platform or unbounded agents/self-modifying live strategies.

Failure at a scientific gate triggers a new documented hypothesis, not silent threshold relaxation. Failure at a safety gate blocks the dependent phase. Independent engineering may continue while authentic elapsed-market or owner-login evidence accumulates.
