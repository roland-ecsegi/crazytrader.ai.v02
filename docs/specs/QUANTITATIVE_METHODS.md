# Initial quantitative candidates and research protocol

Status: DEFINED research candidates, NOT validated strategies and NOT profitability claims. The original repository defined two modes, not two complete mathematical algorithms. This specification supplies one implementable baseline per mode so research can accept or reject concrete hypotheses. More strategies are optional after baseline evidence, not a condition for initial engineering work.

## Shared scope and convention

Initial study: Binance Spot BTCUSDT and ETHUSDT, hourly closed candles, long/cash only. Start with a fixed declared universe; do not infer validity for other instruments. All numeric values below are research protocol defaults, not owner live risk caps or empirically optimal parameters. Version them before running; post-result changes count as new trials.

Index candle t over [u_t,u_(t+1)). Features are computed only after candle t is finalized and available. To avoid pretending a close can be observed and filled at the simultaneous next open, the OHLC screening baseline schedules entry at open of candle t+2, one full bar later. A signal expires if it cannot be evaluated before that scheduled entry. Event simulation uses the first eligible tradable quote after the scheduled time and processing/network latency; it never fills before signal availability. If that quote is unavailable, skip/flag rather than fabricate a fill. OHLC opening prices are screening references, not promised executable prices.

Use decimal money at the order boundary; finite float64 is allowed for statistical computation with unit/NaN/Inf checks. Gross return, net return, confidence and estimated uncertainty are distinct quantities. No guessed “84% confidence” is used.

## Candidate MATH-RIDGE-01

### Inputs and transformation

For closed bar t, C_t is close. Features are x1=C_t/C_(t-1)-1, x2=C_t/C_(t-6)-1, x3=C_t/C_(t-24)-1 and x4=sqrt(mean(r_j^2)) over the last 24 one-bar simple returns. Require complete windows and positive prices. Each feature is centered/scaled using training-only mean and standard deviation; zero training variance rejects that feature/config. Per-symbol models avoid silently pooling different distributions.

The target is four-bar forward simple return y_t=O_(t+6)/O_(t+2)-1, where O is the screening opening reference. Its availability is at or after u_(t+6). The live execution horizon remains four hours after scheduled entry; actual fill-based P&L is separately measured. Purge any training label whose endpoint crosses a validation/test decision cutoff.

Fit ridge regression with unpenalized intercept:

    minimize over a,b: (1/n) sum_t (y_t - a - b'x_t)^2 + lambda * ||b||_2^2

Training objective and scaling are fixed. Candidate lambda grid = {0.1, 1, 10}; record all three results. Initial protocol uses 180 calendar days training, 30 validation, then 30 untouched forward test, rolling in 30-day steps when sufficient history exists. A fold without adequate complete samples is inadmissible, not padded. Freeze coefficients/scaler for the full forward fold; production artifacts do not refit themselves.

### Assumptions and limitations

This is a conditional mean approximation, not a proof of predictable returns. Stable conditional relationships over each fold are an empirical hypothesis. Returns are dependent/heavy-tailed, labels overlap and variance changes; iid Gaussian standard errors and nominal candle counts are inappropriate. Ridge shrinks estimates and can be biased; standardization/regularization do not cure leakage or non-stationarity. A zero/negative coefficient model is valid. Compare against zero forecast, buy-and-hold net of costs, and the strategy baseline under comparable capital/exposure.

### Signal, uncertainty and decision

Prediction m_t = a + b'x_t estimates gross simple return in the target convention. Cost estimate c_t includes round-trip fees, half-spread on both legs, adverse selection/slippage, latency allowance and size-dependent impact. Avoid double-counting spread when actual bid/ask fills already embed it. Spot has no perpetual funding; financing is excluded because borrowing is unsupported.

Compute a conservative quantity bound first, estimate c_t for that quantity, and recheck net edge after venue rounding or any Hard Risk size reduction; fixed fees/size effects can worsen unit cost for a smaller order. This resolves the apparent decision/sizing circularity. Net estimated edge e_t=m_t-c_t. Initial research decision buffer delta=0.001 (10 bps), fixed before testing; trade candidate only when e_t>delta, data/model scope healthy and no existing position/pending entry for this candidate-symbol. Delta is a candidate parameter, not statistically proven protection. Include stress runs at higher fees/slippage and nearby delta values without selecting the best retrospectively as untouched evidence.

No per-trade calibrated success probability is claimed. Report forecast residuals, conditional bias, empirical interval coverage and aggregate uncertainty from out-of-sample errors; an interval is not an expected-profit guarantee. If a future probability model is added, define its event/horizon and validate reliability/Brier/log loss and calibration on separate data before populating confidence fields.

### Sizing, risk and exit

Compute ATR20 from complete closed bars using true range max(H-L, abs(H-C_previous), abs(L-C_previous)); ATR is the arithmetic mean of 20 true ranges in this baseline. Planned stop distance d=2*ATR20, requiring d>0. Quantity proposal is min(per_trade_loss_budget/(d+per_unit_cost_stress), per_symbol_notional_cap/reference_price, available_mode_quote/(reference_price+per_unit_fee_buffer)). Down-round by venue step then recheck minimums. Zero/admissible-below-minimum quantity means NO_TRADE. Hard Risk applies account/portfolio caps and atomic reservations independently.

Entry uses the certified bounded marketable limit IOC adapter at scheduled time. Partial fills form the actual position; canceled remainder is not retried by the strategy automatically. Exit at the four-hour horizon or predeclared protective stop, whichever eligible event occurs first. No new overlapping candidate position per symbol, no averaging down, no martingale. Stop threshold is based on actual average fill and frozen entry ATR. A stop is a trigger for controlled reduction, not a guaranteed fill/loss bound.

## Candidate STRATEGY-DONCHIAN-01

### Inputs and model

Use hourly closed candles and complete warmup. Upper channel U_t=max(H_(t-20),...,H_(t-1)); exit channel L_t=min(L_(t-10),...,L_(t-1)). Exclude candle t from both channels. Trend filter SMA60_t is the arithmetic mean of C_(t-59)...C_t. ATR20 follows the same formula as above.

Entry signal: C_t>U_t and C_t>SMA60_t with no position/pending entry for the candidate-symbol. This is a deterministic breakout hypothesis, not a probability estimate. A deterministic regime label TREND_ELIGIBLE describes this predicate; the Market Regime Agent's prose cannot change it. Scheduled entry follows the shared t+2 convention and certified execution adapter.

### Confidence, decision and sizing

There is no per-signal success probability and no arbitrary composite “strategy score”. Eligibility depends on the immutable version's validation scope and current signal/data/risk checks. Expected edge is an empirical strategy-level out-of-sample estimate with uncertainty, not a free-form forecast attached to every trade. Reject/defer promotion when evidence is insufficient after costs.

Use the same capped stop-distance sizing rule with initial d=2*ATR20. LOW/MEDIUM/HIGH select predeclared numeric risk-budget and selectivity configurations before research/live activation; a profile change cannot invent alpha or bypass risk. Baseline screening uses one fixed profile, with other profiles treated as separate tested configurations and included in trial counts.

### Exit and execution

At each closed candle, exit if C_t<L_t; schedule the channel exit using the same declared delay. Independently, an intrabar protective stop triggers at entry_fill-2*entry_ATR when certified live quote events cross it. Maximum holding horizon is 72 hours after entry; do not hold indefinitely to improve results. First eligible event wins; contradictory simultaneous events follow a fixed priority (protective exit, timed exit, channel exit, then new entry). One position per candidate-symbol; no short and no pyramiding in this baseline.

OHLC screening cannot determine intrabar path. If a bar crosses multiple relevant levels, use a conservative fill/path assumption and label uncertainty; promotion requires event-level simulation for supported protection. Do not select favorable intrabar ordering.

## Complete validation protocol for both candidates

Register hypothesis, symbols, horizons, feature formulas, trials/grid, data dates/splits, costs, benchmark, effect-size threshold, test statistic, risk caps and rejection criteria before testing. Preserve all runs, rejected ideas and parameter changes. Hyperparameter selection uses training/validation only; a final holdout is touched once for the preregistered choice. After inspection it is no longer untouched.

Use chronological walk-forward splits, purge forward-label overlap and apply an embargo at least covering label/execution overlap, with exact endpoints recorded. Test sensitivity of this rule and ensure no fitting on future data. Stationarity/regime tests are diagnostics; failing to reject a null does not prove a market stable. Explicitly analyze trending, range, shock/high-volatility, low-liquidity and missing-data segments when observed; missing regimes remain INSUFFICIENT EVIDENCE.

Estimate uncertainty with dependence-aware moving/stationary block bootstrap over out-of-sample strategy returns, block length justified by dependence and sensitivity. Do not shuffle iid trades as a universal Monte Carlo model. Resample/stress fees, latency, fills, spreads, gaps and loss clustering; add deterministic historical shock replays. Report sample size and effective independent observations, drawdown/tail distributions, turnover/exposure and confidence intervals. A Sharpe ratio without dependence/sample-size caveats is insufficient.

Record all hypothesis/model/profile trials; use a preregistered multiple-testing adjustment (for the small initial grid, Holm-adjusted tests at family alpha 0.05) and a dependence-aware test whose null and implementation are verified. Broader adaptive search requires an updated selection-bias protocol. A lower confidence bound on net performance > 0 may be required for economic promotion but is never a future profit guarantee. State uncertainty when reliable testing is not possible.

Promotion requires robustness under adverse costs, parameter perturbation, out-of-sample/walk-forward evidence and relevant forward-stage observations. The specific minimum effect/effective sample size comes from a documented power/precision analysis for that candidate, not a universal “100 trades” rule. See TESTING_AND_CERTIFICATION for engineering gates and separate economic eligibility.

## Research tooling

Use NumPy/SciPy/statsmodels/scikit-learn or equivalent maintained libraries behind reproducible configs; verify the ridge scaling convention against a small independent hand-computed fixture. Bootstrap/time-series tooling must preserve dependence. Optuna is optional after the small fixed search is understood. Reinforcement learning, online self-modifying live weights and a large indicator zoo are outside initial scope.
