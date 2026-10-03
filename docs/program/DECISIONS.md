# Program Decisions

## 2026-10-02 — Full architecture audit

- Enterprise Local completion means **L6 LIVE CERTIFIED**, not merely code-complete.
- Codex Cloud never receives live Binance credentials or owner-local Claude authentication material.
- Math Mode authoritative live decisions remain quantitative/statistical.
- Risk controls distinguish risk-increasing from risk-reducing actions.
- No fixed monthly return/minimum trade count is a system success requirement.
- Autonomous phase gates use mandatory adversarial review but do not require routine human approval.
- An early open-source compatibility/license spike is mandatory before rebuilding mature infrastructure.
- Current Codex Goals auto-continue only while active/within budget; budget-limit auto-resume cannot be guaranteed by repository code, so the project uses durable checkpoint/resume protocol.
- Final live canary runs on the owner-controlled local deployment, not Codex Cloud.


## 2026-10-03 — v02 baseline integrity and L5/L6 authority correction

- v02 was seeded from the exact pre-Codex v01 snapshot at `c1239585e1d9405dfc083bf9fb0bb7a4e46ab71e`.
- Before v02-specific adaptations, all 38 baseline files matched the source snapshot by path and blob SHA.
- Repository-local operational references were adapted to v02 without importing later Codex implementation work.
- The baseline contradiction between “no real money before L6” and required L5 canary evidence is resolved by ADR-0006: general LIVE requires L6, while a tightly bounded owner-local L5 canary is the sole pre-L6 real-money exception.
