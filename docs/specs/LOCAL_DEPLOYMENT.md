# Enterprise Local deployment and operations

Status: DEFINED target; no deployment/backup/SLO has been demonstrated. One owner-controlled Linux/private host, tested hardware profile, custom local UI. Compose/systemd or equivalent simple supervision; no Kubernetes requirement.

## Minimal deployment boundary

A modular deterministic financial core (including execution, reconciliation and ledger ownership), PostgreSQL, local OPA evaluation, controlled UI/API, isolated agent/Claude worker and isolated research worker. Modules may share safe transaction/process boundaries; no research or AI shell executes inside a live-credential process. Local artifact directories and DuckDB/Parquet support history/research. NATS/ClickHouse/MLflow/OpenBao servers are optional decisions under ADR-0007.

Bind data/policy/admin endpoints privately; UI defaults loopback with real authentication and CSRF/origin protection. Live credentials are injected only on the owner machine into the execution boundary, via OS keyring/vetted secret provision or OpenBao if adopted. No secret in image, source, env dump, agent context or cloud runner. Document key rotation/revocation and verify read/trade permissions with withdrawal disabled before live.

## Resource and observability budget

Benchmark target CPU/RAM/disk/network with representative symbols, feed rates, research jobs and all task queues. Set hard worker concurrency/memory/disk limits; pause research first under resource pressure. Bound market-book history, task backlog and transcripts. Initial hourly two-symbol research does not justify collecting every tick for all markets.

Structured logs carry trace/intent/order/task IDs and redact sensitive payloads. Metrics include freshness, gaps, event/outbox lag, account revisions, holds, UNKNOWN orders, reconciliation age, P&L/drawdown/exposure, fee/slippage error, policy denials, latches, provider quota, CPU/RAM/disk, backup age and alert delivery. OpenTelemetry instrumentation and local metrics endpoints are appropriate; Prometheus is optional packaging after resource measurement. Critical local alerts and UI indicators work without Claude or external messaging. External notification channels are opt-in and tested for failure.

Use TESTING_AND_CERTIFICATION's initial measured targets. No claimed sub-second strategy or “enterprise SLA” without workload/hardware evidence. Keep operator runbooks for UNKNOWN order, mismatch, data gap, quota outage, secret failure, disk full, power loss, restore and kill/clear.

## Startup and shutdown

Startup defaults new-risk disabled. Verify deployed build/policy/certification/config hashes, storage integrity, exclusive sender epoch and clock; restore state without sending; reconcile unresolved orders/fills/balances against venue; re-establish fresh data; then permit the explicitly activated mode. Restore from backup always requires owner review before reactivation. Old outbox requests are not resent as new orders.

Shutdown blocks new risk, records unresolved sends/holds, follows configured cancel/hold policy, flushes journal/outbox, reports residual positions/orders and terminates the sender under supervision. Stop is not an assumption that the account is flat. Power loss may leave positions at the venue; native stops only count if individually supported and confirmed.

## Backups, RPO/RTO and restoration

Encrypted backups on a physically separate destination under owner control; local same-disk copies are insufficient disaster recovery. Include PostgreSQL base backup/WAL as selected, immutable artifacts/manifests, config/policy bundles, audit, schema migrations and emergency journal. Separate documented recovery of secret store/OS credentials; never export raw secrets into routine research backups.

Target disaster RPO <=15 minutes and safe reconciled RTO <=30 minutes, measured with representative dataset/hardware. Database crash durability and backup disaster RPO are different guarantees. Rebuild reconstructable market caches from manifests; never treat financial journal as an expendable cache. Verify hashes and restore into isolated no-send mode, replay idempotently, compare balances with venue, recover post-backup fills/orders and open an incident for unrecoverable gaps. Do not overwrite exchange truth with old local state.

Exactly one host/process may hold execution authority. Before restoring/failing over to another host, fence the original host or revoke/rotate its venue credential; DB leases alone cannot fence Binance calls from a disconnected host. A local OS supervisor plus database epoch is sufficient only within the tested single-host boundary.

## Upgrade and rollback

Pin artifacts/dependencies and store SBOM/license record. Run migrations on a backup/test copy, verify compatibility, pause new risk and preserve position protection/reconciliation during planned maintenance. Back up before changes. Rollback code only when its schema/event interpretation remains compatible; otherwise roll forward or restore in no-send mode and reconcile. An old model/config cannot inherit current certification automatically. Upgrade CLI/provider adapters through their scoped compatibility tests.

Single host, home connectivity and power remain availability limits. UPS or secondary connectivity is optional after measured outage impact; neither replaces recovery tests. Global SaaS availability is a separate phase.
