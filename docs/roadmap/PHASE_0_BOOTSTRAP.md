# P0 bootstrap and compatibility evidence

Status: PLANNED; documentation remediation is complete separately. Read STATUS and current ExecPlan before starting implementation. No live exchange credentials or live-order submission in cloud/development.

Implement T001–T006 from CODEX_TASK_GRAPH. Choose justified Python financial/research modules and TypeScript custom UI; use mature native dependencies before custom Rust. Create only directories with an immediate owner/implementation need; the service list is logical ownership, not mandatory empty microservices.

Deliver executable TradeIntent, RiskDecision, PolicyDecision, Reservation, SubmissionAuthorization, OrderAction/Attempt, Fill, ledger, AgentTask, certification and event schemas; formatting/type checks/unit/property/contract framework, CI, secret scan and reproducible setup. Include invalid-unit/NaN/malformed-state fixtures. Add minimal storage/engine/provider prototypes to resolve high-cost assumptions early.

Run Nautilus/SDK ownership/license spike, PostgreSQL atomicity/replay fixtures, independent formula/causal-label fixtures and the Claude adapter spike. Owner-native Claude login is a local external step; cloud fixtures must not be mislabeled as real subscription evidence.

Acceptance: exact build/check commands run; schemas and independent invariants pass; dependencies/licenses and selected state ownership recorded; no unsafe alternative sender; quantitative trial protocol frozen; provider uncertainty explicit; next P1 work can proceed without inventing semantics. No L1/L2 readiness yet.

In an explicitly launched Autonomous Program, update the ExecPlan, VALIDATION_LOG and STATUS, commit/push and advance to eligible work after true gate pass. A documentation-only request ends with its documentation deliverable and does not launch this program.
