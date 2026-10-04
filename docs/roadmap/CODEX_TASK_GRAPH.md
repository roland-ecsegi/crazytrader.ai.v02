# Dependency graph and implementation acceptance

Revision 2026-10-04. Task IDs below replace the unimplemented earlier sequence; no completed implementation is renumbered or claimed. All tasks except T000 documentation are pending. Tasks may overlap only when dependencies/contracts are stable and no shared financial files/migrations conflict.

| ID / priority | Deliverable | Depends on | Measurable acceptance |
|---|---|---|---|
| T000 / P0 | Audited documentation and owner requirements | Baseline audit/discussion | Cross-file integrity and F01–F24 mapping; remains L0 |
| T001 / P0 | Toolchain, CI, env isolation, secret scan, ExecPlan | T000 | Reproducible local build/check commands; no live route |
| T002 / P0 | Executable domain/event/authority schemas | T001 | Invalid units/fields/versions rejected; authority identities distinct |
| T003 / P0 | Engine/SDK ownership and license spike | T002 | Single sender/state owner, required ambiguous-order cases demonstrated |
| T004 / P0 | PostgreSQL journal/reservation/outbox prototype | T002 | Concurrent-mode holds and crash/duplicate posting invariants pass |
| T005 / P0 | Claude CLI adapter compatibility spike | T001 | Cloud fixtures plus actual owner-local native-auth/cost/isolation evidence; external portion may remain blocked |
| T006 / P0 | Math fixtures and preregistered candidate protocols | T002 | Independent formula/label/timing/sizing checks; trials/splits frozen |
| T007 / P1 | Historical/live normalization and dataset manifests | T002, T006 | Point-in-time/gap/revision/universe tests pass |
| T008 / P1 | Production ledger, attribution and reservations | T004, T007 contracts | Per-asset balance, fee/external-trade/hold/replay tests pass |
| T009 / P1 | Hard Risk, OPA, latches and emergency evaluator | T002, T008 | Negative authorization/concurrency/dependency-matrix tests; adversarial review |
| T010 / P1 | Engine execution adapter and deterministic fake venue | T003, T008, T009 | Full order-state/UNKNOWN/filter/rate/fencing matrix; independent review |
| T011 / P1 | Reconciliation/restart and emergency journal recovery | T010 | Crash at all boundaries, no duplicate effects, account ownership restored; review |
| T012 / P1 | Shared Math/Strategy feature/decision/sizing code | T006, T007, T009 | Same captured inputs yield declared deterministic outputs across adapters |
| T013 / P1 | Research runner, trial registry and robustness | T007, T010, T012 | Reproduce all candidate results; leakage/cost/uncertainty/selection controls |
| T014 / P1 | Early authenticated control/UI and telemetry | T001, T002, T008 | End-to-end simulated intent/rejection/ledger/Kill evidence |
| T015 / P1 | L1/L2 gate package | T011, T013, T014 | Engineering criteria pass; separate economic accept/reject/insufficient result |
| T016 / P2 | Real-time paper/shadow adapters | T015 | Data freshness, no-send shadow, same risk/accounting paths |
| T017 / P2 | Durable agent runtime and eleven manifests | T002, T004, T005 adapter fixtures | Leases/dedup/budgets/restart/permissions tested; no live secrets |
| T018 / P2 | Real skills, memory and all-decision learning | T013, T017 | Typed service-backed skills, abstention/rejection provenance, no self-promotion |
| T019 / P2 | Loki retrieval/tools and evaluations | T014, T017, T018 | Grounded guidance, protected-action routing and held-out evaluation pass |
| T020 / P2 | Complete Command Center journeys | T016, T019 | Both modes, agents, research, evidence, quota and safe owner controls usable |
| T021 / P2 | Local security, backups, fencing and operations drills | T009, T011, T014 | Real target-host restore/kill/redaction/auth/resource tests meet registered targets |
| T022 / P2 | Actual Claude subscription acceptance | T005, T017, T019 | Owner-local plan/model/cost-route/isolation/evaluation evidence, no automatic paid fallback |
| T023 / P2 | L3/L4 elapsed-market evidence | T016, T020, T021 | Registered paper/shadow floors and case coverage, no unresolved critical defects |
| T024 / P3 | Scoped L5 canary authorization | T023, T022, eligible T013 candidate | Owner-local key/caps/stop policy, explicit activation, security/architecture review |
| T025 / P3 | Bounded actual canary and evidence | T024 | Real fills/fees/reconciliation/stop criteria; no fabricated time or forced signal |
| T026 / P3 | L6 scope and owner activation | T025 | Predeclared gates pass, no unresolved BLOCKER/CRITICAL, explicit owner action |
| T027 / P4 | Sustained resource/retention and recovery verification | T021, T026 | Long-run declared workload, upgrade/restore/rollback evidence |
| T028 / P4 | Controlled allocation/ramp acceptance | T008, T013, T026 | Versioned owner caps, uncertainty/stress, reversible steps; fixed allocation sufficient initially |
| T029 / P4 | Complete two-mode/eleven-agent product acceptance | T020, T022, T026, T027, T028 | Every PRODUCT_REQUIREMENTS row demonstrated; missing eligible mode remains pending |
| T030 / P4 | Final independent adversarial release review | T029 | Evidence-backed Enterprise Local acceptance and honest residual limits |

Security, observability, tests and recovery are cross-cutting from T001, not postponed to T021. T021 is consolidated target-host demonstration. T005's owner-local dependency does not block independent deterministic work, but cannot be waived for T022/full product acceptance. No routine human approval is needed for ordinary engineering gates; credentials, paid spend and live capital remain explicit external actions.
