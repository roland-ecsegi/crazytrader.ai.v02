# AI routing and zero-extra-paid-inference profile

Status: DEFINED intended route; owner-local compatibility UNVERIFIED. ADR-0008 and CLAUDE_SUBSCRIPTION_SPIKE govern acceptance.

Agent identity -> durable task admission -> AI Gateway -> official owner-local Claude Code CLI -> schema/tool/result validation -> durable outcome. AI Gateway supervises approved transports; it is not a subscription-token proxy. There is no automatic paid fallback.

## Provider classification

| Dependency | Classification | Current decision |
|---|---|---|
| Official Claude Code CLI with owner's subscription | REQUIRED NOW for requested AI product acceptance | Implement controlled adapter and verify native auth/plan/model/quotas locally |
| Claude/ChatGPT coding tools | DEVELOPMENT-ONLY | Existing entitlements can assist development; no runtime API entitlement inferred |
| Claude paid API | OPTIONAL | Disabled by default; explicit owner-paid opt-in only |
| Python/TypeScript Agent SDK product adapter | OPTIONAL / DEFER TO SAAS PHASE | Do not assume consumer OAuth can power a product API; use supported auth if selected |
| Local open-source inference | REPLACEABLE BY LOCAL/OPEN SOURCE for advisory tasks | Optional evaluated adapter; not a silent substitute for requested Claude acceptance |
| Paid embedding, hosted vector store, remote agent cloud | OPTIONAL | No current requirement; local retrieval/tasks suffice |
| Exchange public/signed APIs | REQUIRED NOW for market/account/execution integration | API quotas and trading fees differ from LLM token billing; current venue terms apply |

## Official-source compatibility note

Checked 2026-10-04: Anthropic documents native subscription sign-in to an unmodified Claude Code binary, including permitted hosted use under applicable terms; it prohibits collecting/intermediating subscription credentials as a third-party application login. Product/SDK API authentication is a separate route. Individual-plan limits are not unlimited automation rights. Verify applicability for this owner-local integration; unresolved terms/capability questions block that acceptance claim.

Official programmatic CLI supports `claude -p`. Current `--bare` skips subscription OAuth/keychain, so it is not the subscription-profile isolation solution. Non-bare invocations may load local hooks/plugins/MCP settings; run only from a controlled workspace/config with OS-level isolation and verify loaded capabilities.

Sources: [legal/auth boundaries](https://code.claude.com/docs/en/legal-and-compliance), [programmatic CLI](https://code.claude.com/docs/en/headless), [model configuration](https://code.claude.com/docs/en/model-config). Recheck at integration/upgrades; these are dated observations, not permanent contractual guarantees.

## Process and tool isolation

Pin/test an official unmodified binary version and capture supported flags/capabilities. Owner completes native login on the owner machine; application never extracts, copies or stores OAuth tokens. Separate OS identity/config and controlled working directory from untrusted repositories, user shell secrets, production volumes and exchange process. Remove API/provider billing credentials from this profile's environment; do not print the environment during diagnosis.

Constrain tool access with verified CLI controls plus server-side allowlists and OS/network/filesystem boundaries. No arbitrary Bash, production writes or live exchange MCP. Process argv is constructed as an argument array, not interpolated shell command. Bound stdout/stderr/input size, subprocess lifetime/process tree, cancellation and tool calls; validate both structured schema and semantics. Invalid flags, missing authentication or malformed output are explicit failures, not successful empty responses.

Allowed models are discovered and verified for the actual plan. Check model resolution and any extra-usage/extended-context charges; fallback is an explicit allowlisted route under the same cost/permission constraints, otherwise fail visible. Loki chooses the least resource-consuming model that meets its evaluation, not a hard-coded price label. Provider estimates do not establish actual billing.

Claude outage never disables Hard Risk, OPA, execution reconciliation, ledger, Kill or deterministic position protection. Production deployment accepts the quota/availability limit explicitly and exposes it in UI.
