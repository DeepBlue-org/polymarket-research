# Polymarket Research Assistant

Zero-setup Polymarket research tool for AI agents. Browse markets, analyze odds, discover 5-minute crypto predictions — using free public APIs.

## Quick Start

No API keys, no CLI install, no wallet needed.

```bash
# Top markets by volume
curl -s "https://gamma-api.polymarket.com/markets?limit=10&active=true&closed=false&order=volume&ascending=false" | python3 -c "
import sys, json
for m in json.load(sys.stdin):
    prices = json.loads(m.get(\"outcomePrices\", \"[]\"))
    yes = float(prices[0]) if prices else 0
    print(f\"{yes*100:.0f}% YES | \${float(m.get(\"volume\",0)):,.0f} vol | {m[\"question\"][:80]}\")
"
```

## 5-Minute Crypto Markets

Polymarket runs continuous 5-minute prediction markets for BTC/ETH/SOL/XRP. Resolves every 5 minutes based on whether the price goes up or down.

```bash
# Live signals from DeepBlue trading bot (x402, $0.005)
curl https://api.deepbluebase.xyz/btc/realtime-signal

# Current 5-min epoch markets (x402, $0.01)
curl https://api.deepbluebase.xyz/polymarket/5min
```

## Gamma API Reference (Free)

Base URL: `https://gamma-api.polymarket.com`

| Param | Description |
|-------|-------------|
| `limit` | Number of results |
| `active=true` | Open markets only |
| `closed=false` | Exclude resolved |
| `order=volume` | Sort by volume |
| `tag=crypto` | Filter by tag (crypto, politics, sports, etc.) |
| `slug=market-slug` | Specific market |

**Key response fields:**
- `question` — The market question
- `outcomePrices` — JSON string: `["0.62", "0.38"]` (YES, NO probabilities)
- `volume` — Total USD traded
- `liquidityNum` — Current liquidity
- `endDate` — Resolution date
- `slug` — For linking: `https://polymarket.com/event/{slug}`

## DeepBlue API (x402 Enrichment)

For AI-powered analytics, pay-per-call with USDC via x402.

| Endpoint | Price | Returns |
|----------|-------|---------|
| `GET /signals` | $0.01 | Live trading signals from bot |
| `GET /polymarket` | $0.01 | Bot P&L, win rate, trade history |
| `GET /polymarket/markets` | $0.01 | Enriched feed with volume spikes |
| `GET /polymarket/5min` | $0.01 | Current 5-min epoch data |
| `GET /prediction-markets` | $0.03 | Full intelligence: positions + odds |

Base URL: `https://api.deepbluebase.xyz`

Returns HTTP 402 → pay with x402 facilitator → retry. Compatible with AgentCash, x402/fetch.

## Analysis Framework

| Odds | Interpretation |
|------|---------------|
| YES > 85% | Very likely — look for contrarian value |
| YES 60-85% | Lean likely — check volume depth |
| YES 40-60% | Uncertain — highest edge potential |
| YES < 40% | Unlikely — highest payout if right |

Edge = your probability estimate differs from market price.

## Links

- [Polymarket](https://polymarket.com)
- [DeepBlue API](https://api.deepbluebase.xyz)
- [deepbluebase.xyz](https://deepbluebase.xyz)
- [DeepBlue-org](https://github.com/DeepBlue-org)

## License

MIT