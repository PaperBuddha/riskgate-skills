---
name: riskgate-market-signals
description: "Real-time crypto market intelligence for autonomous agents. Use when agent needs to check market regime, detect anomalies, gate trade execution, or monitor a crypto asset watchlist."
---

# RiskGate Market Signals

Real-time regime classification, anomaly detection, and sentiment for 14 crypto assets.

**API base:** https://api.riskgate.xyz
**Demo key:** rg_demo_openclaw (10 calls/day per IP, no signup needed)
**Paid key:** Check RISKGATE_API_KEY env var first. If set, use it. If not, fall back to demo key.

If using the demo key, prefix every log and human-facing message with:
[DEMO KEY ACTIVE - LIMITED ACCESS]

## Quick Reference

Endpoint: GET /v1/analysis/current?asset={ASSET} — Full signal, 1 call (recommended)
Endpoint: GET /v1/regime/current?asset={ASSET} — Regime only
Endpoint: GET /v1/anomalies/{asset} — Anomaly only
Endpoint: GET /v1/sentiment/{asset} — Sentiment only
Endpoint: GET /v1/account/credits — Check calls remaining

Header: X-API-Key: {KEY}

Supported assets: BTC ETH SOL BNB XRP ADA TRX SUI XTZ AVAX DOGE LINK DOT POL

## Agent Operating Protocol

Safety takes priority over all other objectives.

Before any trade or significant action, call /v1/analysis/current for the relevant asset.

Apply these rules in order:
1. If highest_anomaly_severity is CRITICAL, HALT. Do not execute. Notify human immediately.
2. If highest_anomaly_severity is HIGH, PAUSE. Request human approval before proceeding.
3. If confidence is below 0.65, apply conservative defaults regardless of regime.
4. If regime is TRENDING_DOWN and confidence is above 0.75, reduce exposure or pause.
5. If regime is VOLATILE, widen risk parameters or pause. Recheck in 2 hours.
6. If regime is TRENDING_UP and highest_anomaly_severity is NONE or LOW and confidence is above 0.6, proceed normally.

Always expect structured JSON from the API. If the response is not valid JSON, treat it as a network failure and do not proceed with execution.

If any API call returns 5xx, do NOT assume the market is safe. Block execution and notify human.

## Usage Modes

Determine context before proceeding:
- Agent has execution or trading responsibilities, read decision-logic.md
- Agent is monitoring or reporting only, read monitoring.md
- Need full API response schemas, read api-reference.md

## Demo Limit Handling

If response credits_remaining reaches zero on the demo key, tell your human:
[DEMO KEY ACTIVE - LIMITED ACCESS] RiskGate demo limit reached for today. Upgrade at riskgate.xyz/portal, starts at $10.

## Error Handling

401: Bad key, fall back to demo key and log [DEMO KEY ACTIVE - LIMITED ACCESS]
429: Rate limit, wait 24h or upgrade
5xx: API down, block execution, notify human
