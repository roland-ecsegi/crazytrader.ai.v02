# Documentation remediation ExecPlan

## Goal
Reconcile the complete documentation baseline with the independent 2026-10-04 audit and owner requirements. Deliver reviewable financial, quantitative, agent, deployment and validation contracts, a dependency-ordered implementation plan, and a before/after report. Documentation completion does not grant trading certification.

## Non-goals
No application implementation, executable schemas, deployment, credentials, trading, profit claim, SaaS build, dependency installation or repository-visibility change.

## Architecture context
AGENTS.md invariants, ADR-0001 through ADR-0006 and the original 40-file baseline are inputs. New accepted ADRs refine local deployment, AI runtime and financial semantics. Historical audit/provenance files remain intact.

## Current state
Baseline main commit: `6a437ac2d745e2c901e8c46ec483211721cb47cf`. Autonomous branch: `c45f1f01cd69ef946c656b43d52db900f3377420`. All 40 files are Markdown; no executable implementation or validation evidence. Certification L0.

## Proposed design
Preserve both modes and the deterministic financial boundary. Specify atomic authorization/accounting, event-driven durable agents including Loki, subscription-only Claude CLI acceptance, initial research candidates, point-in-time data, scoped certification and local recovery. Logical ownership does not require a microservice per responsibility.

## Files and ownership
Root entry points and instructions; architecture/ADR; financial, data, quant, agent and operations specs; roadmap and program state. Add focused specifications only where a distinct authoritative contract is needed. Final comparison lives in docs/audit.

## Dependencies and licensing
No dependency adopted or installed in this documentation change. OPEN_SOURCE_ADOPTION defines evidence required before pinning. Claude authentication/model availability and vendor terms require owner-local compatibility evidence; documentation cannot certify them.

## Failure modes
Conflicting duplicated rules, superficial closure of audit findings, accidental implementation claims, paid fallback, overpromised emergency flattening, look-ahead assumptions and overwriting concurrent Git changes. Prevent through cross-file checks and fresh branch verification before publishing.

## Security and financial-risk impact
Design changes concern both sides of the financial boundary, but have no runtime effect. No secrets or exchange authority are requested. Preserve immutable history and explicit L5/L6 owner activation.

## Data/event migrations
No deployed data exists. Contract additions are proposed v1 implementation requirements; future deployed incompatible changes require versioned migrations and replay tests.

## Implementation steps
- [x] Verify baseline, audit and owner requirements.
- [x] Revise specifications and ADRs together.
- [x] Update roadmap, entry prompts and status.
- [x] Run documentation integrity and adversarial consistency checks.
- [x] Publish an atomic documentation commit after fresh branch checks.
- [x] Deliver detailed comparison and remaining-work report.

## Test plan
Run the documentation validation script recorded in VALIDATION_LOG against original and revised snapshots: unchanged inventory preservation, relative-link resolution, agent count, canonical contract references, stage sequence, prohibited readiness claims and semantic conflict review. These are documentation checks, not application tests. Exact executed commands/results are recorded before publication.

## Acceptance criteria
All original paths retained; historical evidence preserved; every F01–F24 finding mapped to an authoritative correction and pending implementation proof; Loki and subscription-only runtime specified; no readiness above L0 claimed; no code or credentials changed; comparison report complete; remote tree verified.

## Rollback/recovery
Revert the documentation commit through Git if required. Do not force-reset either branch or erase concurrent work. No live data migration occurs.

## Resume checkpoint
Working branch: documentation preparation for main and codex/enterprise-local-autonomous, subject to fresh head checks. Last durable baseline commit: `6a437ac2d745e2c901e8c46ec483211721cb47cf`. Current step: completed documentation checkpoint, published by the containing commit. Next action: owner may launch implementation T001 separately. Uncommitted work in the delivered tree: none. Blockers: none for documentation; implementation proof remains pending. Usage state: documentation complete. Last verification: the documented checker passes 54 files, 11 agents, 31 task rows and 24 finding mappings. The containing commit identifies the delivered revision; do not invent a self-referential commit hash.

## Progress log
- 2026-10-04: Confirmed unchanged repository and separate original snapshot; started documentation-only remediation under explicit owner authorization.

- 2026-10-04: Finished specification/roadmap alignment and adversarial consistency review; automated documentation check PASS. All 40 original paths retained, 54 total Markdown files. Comparison report prepared; publishing this checkpoint atomically.

## Decisions
Use eleven permanent identities on a shared durable runtime. Preserve OPA authorization while simplifying deployment. Introduce explicitly unvalidated candidate methods rather than pretending original mode names were mathematical algorithms. All runtime claims remain pending evidence.

## Completion summary
Delivered: revised specifications/ADRs/roadmap, retained original audit, reproducible documentation checks and detailed Romanian comparison report. Publication is represented by the containing Git commit; remote tree verification and the companion Word report are reported in the delivery message. No executable application tests or trading performed. Application remains L0; next phase is separately launched T001.
