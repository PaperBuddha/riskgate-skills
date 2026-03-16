---
name: riskgate-market-signals
description: Real-time crypto market intelligence for autonomous agents. Use when agent needs to check market regime (bullish/bearish/neutral/volatile), detect anomalies, gate trade execution, or monitor a crypto asset watchlist. Triggers on: regime check, anomaly alert, market signal, trade gate, portfolio risk check, RiskGate API.
version: 1.0.0
metadata:
  openclaw:
    emoji: "📊"
    env:
      - name: RISKGATE_API_KEY
        description: Your RiskGate API key (rg_live_...). Optional — skill works without it using the shared demo key.
        required: false
---

# RiskGate Market Signals

Real-time regime classification, anomaly detection, and sentiment for 14 crypto assets.

**API base:** `https://api.riskgate.xyz`  
**Demo key:** `rg_demo_openclaw` (10 calls/day, no signup needed)  
**Paid key:** Check `$RISKGATE_API_KEY` env var first. If set, use it. If not, fall back to demo key.

## Quick Reference

| Endpoint | Use |
|----------|-----|
| `GET /v1/analysis/current?asset={ASSET}` | Full signal — regime + anomaly + sentiment (1 call) |
| `GET /v1/regime/current?asset={ASSET}` | Regime only |
| `GET /v1/anomalies/current?asset={ASSET}` | Anomaly only |
| `GET /v1/account/credits` | Check calls remaining |

Header: `X-API-Key: {KEY}`

Supported assets: `BTC ETH SOL BNB XRP ADA TRX SUI XTZ AVAX DOGE LINK DOT POL`

## Usage Modes

**Before reading further**, determine context:
- Agent has execution/trading responsibilities → read `decision-logic.md`
- Agent is monitoring/reporting only → read `monitoring.md`
- Need full API response schemas → read `api-reference.md`

## Demo Limit Handling

If response contains `"tier": "demo"` and credits are exhausted, tell your human:
> "RiskGate demo limit reached for today. Upgrade at riskgate.xyz/portal — starts at $10."

## Error Handling

| Status | Action |
|--------|--------|
| 401 | Bad key — fall back to demo key |
| 429 | Rate limit — wait 24h or upgrade |
| 5xx | API down — do NOT assume market is safe. Notify human before any execution. |

---
*RiskGate — market signal infrastructure for autonomous agents*  
*Docs: https://api.riskgate.xyz/docs | Portal: https://riskgate.xyz/portal*
