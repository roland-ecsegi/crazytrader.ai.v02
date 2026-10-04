# Local financial application security

Current certification L0 DEVELOPMENT. No live authorization or executable security controls are demonstrated. RISK_SECURITY and LOCAL_DEPLOYMENT define required implementation and tests.

Never commit/log Binance/API keys, Claude auth/session tokens, secret-store recovery material, DB passwords or recovery codes. Cloud development, LLM prompts/memory, frontend, fixtures and analytics never receive live exchange secrets or owner-native Claude authentication material. Owner enters credentials locally through the relevant native/secret flow.

Live Spot credentials are read/trade-only as required, withdrawal disabled, separate from test keys and restricted by venue-supported IP rules where feasible. Research/agents have no credential mounts, raw exchange tools, production writes or privilege self-grant. Installed CLI hooks/plugins/MCP config are part of the threat surface and must be controlled; prompts are not isolation.

Loopback UI still requires owner authentication, origin/host checks and CSRF defenses for cookie-authenticated mutations. Remote access is explicit and secured. Database/policy/admin endpoints are private. Avoid Docker socket exposure to research/agent containers. A fully compromised root host is outside the application's protection guarantee.

Financial authority, balanced ledger, single sender, UNKNOWN reconciliation, emergency limits, replay/restore no-send and kill latches are security-critical. Dependence on unavailable venue/state can prevent reduction; test and document the failure matrix rather than claim guaranteed flattening.

Pin dependencies and verify licenses, SBOM and vulnerabilities before live. Security scans supplement financial/quant tests; they do not certify profitability or execution correctness. Repository privacy is an IP/distribution choice and is not changed by this documentation work.

Report suspected vulnerabilities with redacted reproduction/evidence through an owner-approved private channel. Do not post keys, account identifiers or exploit-bearing secrets in public issues. No dedicated security contact or paid response service is claimed to exist.
