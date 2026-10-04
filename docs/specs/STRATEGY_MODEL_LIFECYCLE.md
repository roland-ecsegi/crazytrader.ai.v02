# Strategy, model and learning lifecycle

Status: DEFINED; registry, experiments and promotion enforcement MISSING.

Lifecycle: DRAFT -> VERIFIED -> BACKTESTED -> OUT_OF_SAMPLE -> WALK_FORWARD -> ROBUSTNESS_PASSED -> PAPER -> SHADOW -> CANARY_ELIGIBLE -> LIVE. Any research stage can produce REJECTED or INSUFFICIENT_EVIDENCE. Post-live: LIVE -> DEGRADED -> SUSPENDED -> RETIRED. These artifact states are distinct from engine L0–L6 certification.

Promotions bind artifact/code hash, candidate/profile versions, dataset/feature/label/cost provenance, all trial records, split dates, metrics/uncertainty, scope, risk review, deterministic policy decision and evidence reviewer. Agents propose promotions; they do not authorize themselves. A previously validated version may be restored only if current scope, venue behavior and operational checks still hold.

## Learning without selection bias

Create DecisionObservation for every eligible evaluation including no-signal, abstention, rejected proposal, approved/unfilled order and executed trade. Record the information available at decision time, reason and eligibility; subsequent OutcomeObservation is linked only after its horizon matures. Keep counterfactual estimates distinct from actual fills/P&L; a rejected order's hypothetical return is not earned profit.

ExperienceRecord links observations, task/strategy/model versions, market/risk state, fills/costs and anomalies. Learning Agent can find patterns and propose new experiments; it cannot conclude from winners or executed trades alone. Changes to filters, features, costs, hyperparameters, training data or profiles create a new version and count as new trials.

Initial live artifacts are frozen between governed promotions. Online adaptive live learning is deferred until a bounded algorithm, safety envelope, reproducible state, independent validation and rollback are separately specified. No LLM may patch deployed code/weights/policy in response to losses.

Deterministic drift monitoring can reduce allocation, suspend a candidate or request review under preauthorized rules. It cannot loosen hard limits or promise that retraining restores profitability. Performance and calibration monitoring record observation dependence, effective sample size and missing regimes.
