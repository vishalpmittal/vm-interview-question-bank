# Estimation Facts

Quick-reference numbers for back-of-the-envelope math during system design interviews. Memorize the magnitudes; the exact figures matter less than knowing the right order.

## Time

| Unit | Seconds |
|---|---|
| Day | 86,400 |

## Population

| Group | Count |
|---|---|
| World | 8.26 billion |
| US | 350 million |

## Daily Active Users (DAU)

| Platform | DAU |
|---|---|
| YouTube | 122 million |
| Spotify | 15 million |
| Twitter | 20 million |
| Amazon | _TBD_ |

## Short-URL / ID Combinatorics

Character set `a-z`, `A-Z`, `0-9` = **62 characters**.

| Length | Combinations |
|---|---|
| 1 | 62 |
| 2 | 62² = 3,844 |
| 6 | 62⁶ ≈ 56 billion |
| 7 | 62⁷ ≈ 3.5 trillion |

## Read / Write Ratios

- **Typical read:write ratio = 100 : 1**
- Use this when sizing read replicas, caches, and QPS budgets unless the problem says otherwise.

Throughput shorthand:
- **Writes per second (WPS)** — sustained insert/update throughput
- **Queries per second (QPS)** — sustained read throughput

## Magnitudes

| Prefix | Bytes / Count |
|---|---|
| KB | 10³ |
| MB | 10⁶ |
| GB | 10⁹ |
| TB | 10¹² |
| PB | 10¹⁵ |
| 1 million | 10⁶ |
| 1 billion | 10⁹ |

## Encoding Sizes

- 1 character ≈ **1 byte** (ASCII / UTF-8 for the ASCII range)
- 64-bit integer = **8 bytes** — required to store values up to ~9.2 × 10¹⁸

### Worked example: storing 8 billion numbers

- 8 billion entries × 8 bytes/entry = **64 billion bytes = 64 GB**

## Geospatial

### Latitude
- Range: **-90° to +90°**
- 0° → Equator, +90° → North Pole, -90° → South Pole

### Longitude
- Range: **-180° to +180°**
- 0° → Prime Meridian, positive → East (E), negative → West (W)

### Grid sizing

- Earth divided into 1° × 1° blocks → **180 × 360 = 64,800 blocks**
- Each block ≈ **69 miles × 69 miles** (≈ **111 km × 111 km**) at the equator

## Financial Markets (US Equities)

Useful when designing trading systems, market data feeds, tickers, or portfolio services.

### Listing & Index Sizes

| Index / Exchange | Count |
|---|---|
| NASDAQ-listed companies | ~3,300 |
| NYSE-listed companies | ~2,300 |
| Total US-listed stocks | ~5,500–6,000 |
| S&P 500 | 500 companies (~503 stocks; multi-class shares) |
| Dow Jones Industrial Average (DJIA) | 30 |
| Russell 2000 | 2,000 small-caps |
| Russell 3000 | ~3,000 (covers ~98% of US public equity market cap) |

### Market Hours (Eastern Time)

| Session | Hours |
|---|---|
| Pre-market | 4:00 AM – 9:30 AM ET |
| Regular | 9:30 AM – 4:00 PM ET (6.5 hrs = **23,400 sec**) |
| After-hours | 4:00 PM – 8:00 PM ET |

- Trading days per year: **~252**
- Market holidays: ~9–10 per year (full closures); a few early-close half-days

### Volume & Throughput

| Metric | Value |
|---|---|
| US equity volume (all exchanges) | ~10–15 billion shares/day |
| NASDAQ daily share volume | ~4–6 billion |
| NYSE daily share volume | ~3–4 billion |
| US options volume | ~40–50 million contracts/day |
| Trades/day across US equities | hundreds of millions |
| Market data peak rate (NASDAQ ITCH) | **millions of messages/sec** at open/close |
| Quote-to-trade ratio | **~100 : 1** (quotes vastly outnumber actual trades) |

### Pricing

- Minimum tick size: **$0.01** for stocks ≥ $1.00; **$0.0001** sub-penny for stocks < $1.00
- Prices commonly stored as **integer cents** to avoid floating-point error
- Latency targets: **microseconds** for HFT / co-located feeds; **milliseconds** acceptable for retail dashboards

### Sizing implications

- Budget for **peak ≠ average** — open and close produce large spikes (often 5–10× midday rate)
- Most messages are quote updates, not executed trades — size the feed handler accordingly
- Per-symbol fan-out can be huge (one quote update → millions of subscribers for popular tickers like AAPL/TSLA)
