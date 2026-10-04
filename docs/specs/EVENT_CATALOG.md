# Event catalog and delivery semantics

Status: DEFINED v1 contract; executable schemas and transports MISSING. Events report facts; commands request actions. Receiving an event is not new financial authority.

## Envelope

event_id, event_type, schema_version, owner_id/tenant_id, aggregate_type/id/revision, source_module, occurred_at, recorded_at, available_at when relevant, trace_id, correlation_id, causation_id, actor_id if applicable, payload, payload_hash and producer build/config version. Market events also retain venue event/receive/sequence semantics from MARKET_DATA. Immutable; incompatible changes create a new version.

No global timestamp total order is assumed. Consumers enforce aggregate revision/sequence where required, detect gaps and deduplicate event_id/source fact. Financial mutation + inbox receipt + outbox entry share a database transaction. At-least-once delivery is the baseline; PostgreSQL outbox is sufficient initially. NATS may be added later without changing financial semantics.

## Catalog

All names below are `.v1` contracts to implement, not existing handlers.

| Area | Events |
|---|---|
| Market | MarketTickReceived, MarketTradeReceived, MarketBookUpdated, MarketCandleClosed, MarketDataStale, MarketDataRecovered, MarketSequenceGapDetected, DatasetVersionPublished |
| Signals/observations | MathSignalProduced, StrategySignalProduced, MarketRegimeChanged, DecisionObserved, DecisionAbstained, OutcomeMatured |
| Intent/authority | TradeIntentCreated, TradeIntentExpired, TradeIntentRiskApproved, TradeIntentRiskDenied, TradeIntentPolicyApproved, TradeIntentPolicyDenied, CapitalReserved, SubmissionAuthorized, AuthorizationInvalidated, TradeIntentExecutionRequested |
| Order facts | OrderSubmissionStarted, OrderSubmitted, OrderAcknowledged, OrderPartiallyFilled, OrderFilled, OrderCancelRequested, OrderCancelled, OrderRejected, OrderExpired, OrderStateUnknown, OrderRecoveryStarted, OrderRecovered |
| Ledger/portfolio | LedgerTransactionPosted, ReservationConsumed, ReservationReleased, CapitalAllocationProposed, CapitalAllocated, CapitalAllocationReduced, PositionOpened, PositionIncreased, PositionReduced, PositionClosed, ExternalAccountAdjustmentDetected |
| Risk/control | RiskWarningRaised, RiskDecisionDenied, DailyLossLimitReached, WeeklyLossLimitReached, DrawdownLimitReached, ExposureLimitReached, TradingBlocked, TradingUnblocked, GlobalKillActivated, GlobalKillCleared, OwnerActivationChanged |
| Reconciliation | ReconciliationStarted, ReconciliationCompleted, ReconciliationMismatchDetected, ReconciliationCriticalMismatch, ReconciliationRecovered |
| Agent/provider | AgentTaskCreated, AgentTaskClaimed, AgentTaskStarted, AgentTaskCheckpointed, AgentTaskCompleted, AgentTaskFailed, AgentTaskExpired, AgentTaskWaitingForProvider, AgentAvailabilityChanged, AgentPermissionChanged, AgentMemoryUpdated, ExperienceRecordCreated, AIProviderAvailabilityChanged, AIProviderUsageLimited, AgentBudgetExhausted |
| Research/lifecycle | HypothesisCreated, ExperimentStarted, ExperimentCompleted, StrategyCandidateCreated, StrategyPromoted, StrategyDegraded, StrategySuspended, StrategyRetired, ModelRegistered, ModelPromoted, ModelRolledBack, StrategyPromotionDenied, ModelPromotionDenied |
| Platform | SystemIncidentOpened, SystemIncidentResolved, CertificationScopeChanged, CertificationInvalidated, SecretRotationRequired, ServiceHealthChanged, BackupVerified, RestoreDrillCompleted |

OrderSubmitted means a known submission observation, not necessarily acknowledgment/fill. ReconciliationCompleted carries pass/mismatch/insufficient status; name alone is not healthy evidence. MarketRegimeChanged is produced by a deterministic versioned detector; agent interpretation is a separate task result.

## Replay and compatibility

Replay rebuilds state in no-send mode. Sending a recovered OrderSubmissionStarted event cannot resend an order. Consumers with irreversible side effects use explicit receipt/action identities and current authority checks. Record unknown event versions as errors/quarantine, never silently deserialize into permissive defaults. Test duplicate, reordered, missing and old-version events before certification.
