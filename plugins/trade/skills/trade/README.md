# Trade

Multi-leg options trading assistant — concrete strikes, IV-aware structures, probability-weighted scenarios. Backed by a curated library of 24 pitfalls, a gamma framework, a price-action framework, and 8 closed/in-progress case studies (INTC, Mag-7, APP, NOK, TSEM, CBRS, SNOW, MDB).

## Triggers

- Trade analysis requests, options strategy recommendations, post-mortems
- Mentions of multi-leg structures: Jade Lizard, bull put / bear call spread, iron condor, diagonal, calendar
- Earnings positioning, IV / IV crush, channel checks, AH price action
- Dealer GEX / gamma exposure / max pain / options-chain analysis
- LEAPS / stock-replacement theses
- Any single-stock options play in a US-equity context

See the full trigger list in the `description` field of `SKILL.md`.

## Data Access

**Primary market-data source is the local Bloomberg Terminal via `xbbg`** (Desktop API on `localhost`; Terminal must be running and logged in). Secondary: TradingView desktop reader. Do **not** use Funda AI, yfinance, or web-scraped values when Bloomberg can answer.

Two surfaces:

1. **Raw Bloomberg pulls** (`xbbg`) — quotes, fundamentals, historical series, option chains, Greeks, estimates. See `C:\blp\data\xbbgapiskill.md` for field mnemonics and recipes. ⚠ On this machine (pandas 3.x) xbbg returns **narwhals long-format** — normalize through the `wide()` shim.
2. **Derived trade metrics** — `python C:\blp\data\bbg_trade.py snapshot <TICKER>` returns spot, GEX, gamma-flip strike, max pain, IV Rank, and a net-flow proxy directly from the equity options chain.

**Coverage gaps** (state as unavailable, never fabricate): social/Reddit/X sentiment, Polymarket, congressional trades, earnings-call transcripts, curated smart-money flow. Bloomberg `opt_chain` returns **monthlies + LEAPS only, no weeklies** — short-dated gamma/pinning is limited to the nearest monthly. The net-flow number is a **volume/OI proxy**, not trade-classified smart-money flow.

## Reference Files

| File | Description |
|---|---|
| `references/strategies.md` | Structure-to-regime matching (3 axes), LEAPS stock replacement, setup checklist, position management |
| `references/gamma-framework.md` | Dealer GEX + chain + IV term + flow → multi-factor probability map (expiry, squeezes, pinning) |
| `references/price-action-framework.md` | Orderbook microstructure — buy/sell imbalance, vacuum zones, consensus shifts, float composition |
| `references/pitfalls/README.md` | Index of 24 trading pitfalls (severity-tagged, lookup-by-trade-type) |
| `references/pitfalls/NN-*.md` | One file per pitfall — read only when relevant |
| `references/ticker/README.md` | Index of closed/in-progress trade case studies |
| `references/ticker/<name>.md` | One file per case study (INTC, Mag-7, APP, NOK, TSEM, CBRS, SNOW, MDB) |

## Coverage

- **24 analytical pitfalls** covering consensus anchoring, flow misreading, IV crush traps, T+1 reverse drift, LEAPS vega tax, manipulator-tape recognition, channel-check sample bias, AH order-book fades, dealer-flow vs retail, hazard-rate exit discounting, and the three-axis structure check (direction / vega / asymmetry).
- **8 detailed case studies** showing thesis evolution, structure selection, and post-mortem lessons — including the SNOW (asymmetry-rule failure) ↔ MDB (asymmetry-rule applied correctly) A/B one day apart.
- **Structure-to-regime quick reference** covering high/low IV regimes paired with directional / neutral / manipulator-tape views, plus the bull-conviction count that forbids capped-upside structures (Jade Lizard / IC / calendar / diagonal) when conviction ≥ 4.
