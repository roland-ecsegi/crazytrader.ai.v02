# Point-in-time market data and feature contract

Status: DEFINED; ingestion/datasets/quality tests MISSING. Applies to both modes and every backtest, paper and live adapter.

## Raw and normalized records

Preserve immutable raw records where practical and a normalized record containing venue/account environment, instrument, event kind, venue ID/sequence, event_time, receive_time, available_at, ingestion_time, source/API version, payload hash and quality flags. event_time describes the market; available_at is the earliest time the strategy could actually use the validated record. Historical datasets lacking receive/availability metadata must declare that limitation and apply an explicit conservative delay model. They cannot establish latency/execution realism.

UTC timestamps have declared units/precision. Keep monotonic process time for deadlines/durations and measured exchange/host clock offset; wall-clock corrections cannot extend authorizations or reorder logical state arbitrarily. Record time-zone/candle-boundary conventions.

## Quality and continuity

Deduplicate by source identity and payload rules; conflicting same-ID content is quarantined. Buffer/reorder only within a bounded lateness policy. Rebuild books from snapshot plus correctly sequenced deltas using venue-specific rules; a gap invalidates the book until a verified rebuild. No L2 book reconstruction from OHLC bars. WebSocket reconnect requires backfill, not an assumption of continuity.

Validate nonnegative volume, positive prices, candle OHLC inequalities, duplicate/gap/overlap intervals, symbol lifecycle, missing periods and outliers. Quarantine suspicious data with provenance; do not silently delete extreme moves as “bad ticks” to improve results. Outliers can represent genuine market shocks.

Closed candles carry interval_start, interval_end, finality and revision. Live features cannot use an unfinished candle unless separately specified/certified. A missing candle is missing, not automatically a zero-volume flat bar. Forward filling can support a labeled stale display but must not create an eligible signal. Late corrections create new dataset revisions; live decisions retain the version actually seen.

## Point-in-time joins and features

Feature snapshot records computation code/version, input record IDs, available_at cutoff, window completeness, scaler/model/config hash and quality verdict. A strategy at decision time d can use only records available_at <= d. Join historical universe, fees, instrument filters and corporate/token changes as of d, not today's universe. Include delistings and inactive instruments when the research question selects across a universe; a fixed BTC/ETH study must not claim broad-market selection validity.

Rolling windows include only the stated closed intervals; normalization, winsorization, imputation, feature selection and hyperparameter fitting use training data only. Labels record their full forward interval so purging can remove overlap at splits. Clock uncertainty, stale quote, missing required feature or incomplete warmup yields NO_SIGNAL/DATA_INVALID, not a numerical guess.

## Dataset manifest

Manifest includes source/license, acquisition time, content hashes, symbols and historical eligibility, interval range, raw/normalized schema, corrections, missingness/outliers, available-at model, fee/filter provenance, split definition and generation command/seed. Training, calibration, validation and untouched test partitions are immutable and independently addressable. Keep rejected/unused trials and dataset revisions.

## Same logic across stages

Use the same pure feature/signal/sizing code with injected clock/data/order adapters in backtest, replay, paper and live. Run identical captured events through each deterministic stage and compare outputs/evidence hashes. Differences in data availability, execution adapter, costs, timing and fills are explicitly modeled and reported; code reuse alone does not guarantee live/backtest equivalence.

Parquet stores versioned history; DuckDB performs reproducible queries; PostgreSQL stores manifests, current ingestion watermarks and operational health. Benchmark disk, memory, ingestion backlog and dataset footprint before introducing ClickHouse. Bound book retention rather than collecting all crypto data indefinitely.
