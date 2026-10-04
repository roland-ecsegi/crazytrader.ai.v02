# Capital allocation and controlled growth

Status: DEFINED target; no allocation optimizer or profitable strategy demonstrated. The purpose is controlled allocation, not forced compounding.

Owner supplies absolute capital caps, per-mode budgets and explicit reserve. Initial implementation uses fixed capped allocations and deterministic reduction/freeze rules; a sophisticated optimizer is not required to reach a usable local product. Portfolio Agent proposes changes; Hard Risk and ledger enforce them independently.

Include realized/unrealized P&L, fees, cash flows, holds/unknown orders, liquidity, concentration, correlated exposure, drawdown, operational health, artifact eligibility and effective sample size. Sums of allocations plus reserve cannot exceed available authorized capital. Internal transfer is not a return.

Capital can increase only within owner-preauthorized limits after stage-appropriate forward evidence and after the previous step's observation criteria pass. Define step amount, maximum cumulative allocation, observation interval/sample rule and automatic stop/rollback before canary/live activation. Expanding account, instrument, profile or execution scope requires certification review. Degradation can freeze/reduce allocation immediately under deterministic policy; restoration requires evidence and applicable owner acknowledgment.

No martingale, loss chasing, minimum trade count, fixed monthly income or assumed revenue funding timetable. Kelly/fractional Kelly, dynamic covariance optimization and learned allocation are optional later research requiring estimator uncertainty, leverage prohibition, stress tests and caps. Initial production correctness must not depend on them.
