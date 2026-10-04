# Evidence-based reuse and dependency decisions

Revision 2026-10-04, based on the independent audit's official documentation/original-repository screening. These are adoption directions, not installed/approved versions. Recheck release/license/security facts when pinning. No candidate was benchmarked or dependency installed during documentation remediation.

## Decision rule

For each selected dependency record exact problem, version/hash, release/maintenance evidence, license and transitive obligations, local hardware benchmark, integration/upgrade burden, attack surface, paid-service requirement, replacement interface and acceptance tests. Stars are not a quality gate. Reject overlapping financial state authority. A mature project does not certify our integration.

| Candidate / primary source | Recommendation and exact value | Cost, burden and decision condition |
|---|---|---|
| [NautilusTrader](https://github.com/nautechsystems/nautilus_trader) | ADAPT / SHOULD: event simulation and execution mechanics before custom engine | LGPL-3.0 obligations; Python/native Rust build and domain mapping are substantial. Audit observed active 2.0 RC releases; do not pin an RC merely as newest. One state owner and outer-risk non-bypass are mandatory |
| [Binance Python connector](https://github.com/binance/binance-connector-python) | ADOPT / MUST for official endpoint/reference contract tests; evaluate modular Spot SDK | MIT; modest local overhead; venue fees/rate limits remain. No second uncontrolled sender |
| [PostgreSQL](https://www.postgresql.org/docs/current/transaction-iso.html) | ADOPT / MUST: transactions, reservations, journal, outbox/inbox and task leases | PostgreSQL License; mature SQL boundary; admin/backups needed, no paid server service required |
| [DuckDB](https://github.com/duckdb/duckdb) and [Parquet](https://parquet.apache.org/) | ADOPT / SHOULD: portable versioned history and local analytical queries | DuckDB MIT; RAM/SSD workload-dependent; never authoritative money ledger |
| [statsmodels](https://github.com/statsmodels/statsmodels), [scikit-learn](https://github.com/scikit-learn/scikit-learn), [arch](https://github.com/bashtage/arch) | ADOPT / SHOULD: transparent baselines, diagnostics, ridge and dependence-aware uncertainty | Scientific Python/BSD-family licenses to verify at pin; local numeric dependencies; tests do not establish stationarity/edge |
| [Hypothesis](https://github.com/HypothesisWorks/hypothesis) | ADOPT / MUST: stateful invariant tests and minimized failing interleavings | MPL-2.0 Python package; development-only; independent model/oracle required |
| [Toxiproxy](https://github.com/Shopify/toxiproxy) | ADOPT / SHOULD: network delay/disconnect drills | MIT; local dev/test service; supplement with deterministic fake-venue ambiguity cases |
| [OPA](https://github.com/open-policy-agent/opa) | ADOPT / MUST under existing authority invariant: versioned authorization | Apache-2.0; local integration cost; test outage/emergency semantics, not just policy syntax |
| [OpenTelemetry](https://github.com/open-telemetry) / [Prometheus](https://github.com/prometheus/prometheus) | ADOPT / SHOULD: narrow trace/metric instrumentation; choose local collector packaging after measurement | Apache-2.0 project terms to verify; bound labels/retention and redact; no hosted observability requirement |
| [Optuna](https://github.com/optuna/optuna) | DEFER / COULD until full trial registry and fixed baseline work | MIT; no paid cloud needed; search multiplies selection bias and compute |
| [OpenBao](https://github.com/openbao/openbao) | ADAPT / COULD if OS secret facility cannot meet tested needs | MPL-2.0; unseal/backup/rotation add operator work; secret isolation is required, this server is not |
| [NATS](https://github.com/nats-io/nats-server), [ClickHouse](https://github.com/ClickHouse/ClickHouse), [MLflow](https://github.com/mlflow/mlflow) | DEFER / SHOULD until measured fan-out, analytics volume or experiment-registry need | Apache-2.0 roots; extra stores/processes/recovery. Local DB/files suffice initially |
| [Freqtrade](https://github.com/freqtrade/freqtrade), [Hummingbot](https://github.com/hummingbot/hummingbot) | USE AS REFERENCE / SHOULD: backtest leakage, dry-run and exchange-failure patterns | GPL-3.0 Freqtrade; Apache-2.0 Hummingbot root, transitive review required. Full adoption overlaps UI/governance/engine |
| [CCXT](https://github.com/ccxt/ccxt) | DEFER / COULD to actual multi-exchange research | MIT; duplicates first venue adapter; normalized API can hide important venue differences |
| [cryptofeed](https://github.com/bmoscon/cryptofeed), [vectorbt](https://github.com/polakowo/vectorbt) | USE AS REFERENCE / COULD | Audit observed AGPL-plus-attribution / Apache-plus-Commons-Clause respectively; exact-version license review before embedding; not default core dependencies |
| [llama.cpp](https://github.com/ggml-org/llama.cpp) | ADAPT / COULD for explicitly selected local advisory inference | MIT engine, separate weight licenses; hardware/quality benchmark required; not silently equivalent to Claude acceptance |
| General agent frameworks / hosted workflow platforms | DEFER / COULD until a measured missing capability | PostgreSQL tasks, typed tools and provider adapter meet initial design; framework names do not create durable agents |
| Unofficial subscription proxies / raw exchange MCP for LLMs | REJECT / DO NOT DO NOW | Unsupported credential routing or uncontrolled trading authority defeats requirements |

## Mandatory engine spike

Prove Binance Spot supported filters/order subset; authorization cannot be bypassed by engine strategy callbacks; sole state/sender ownership; fees/partial/cancel/late fills; UNKNOWN recovery; restart persistence; account/ledger mapping; replay no-send; Python/native build and redistribution license fit. Compare a bounded custom adapter only if the candidate cannot satisfy these cases. Record rejection evidence; do not maintain two live engines as a hedge.

## Useful development capabilities

GitHub connector: repository/PR/commit/evidence management, already useful; runtime does not depend on it. Official documentation access: verify venue/provider behavior at pin and upgrade. Local test/benchmark tooling: contract, property, replay and fault injection suites. Versioned project skills can standardize quant gate review and financial invariant review after executable interfaces exist; prompts alone are not verification.

Codex Security is a future development-only optional code review candidate after entitlement/cost and permissions are checked; no scan or installation occurred here. Generic chat/task plugins are not added simply because available. Any external agent tool is allowlisted, versioned and prohibited from live order authority. No plugin is required for the financial runtime merely to manage the repository.
