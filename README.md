# fra-public-feed

Public funding-rate arbitrage signal feed. Updated every 15 minutes during US market hours (09:30–16:00 ET Mon–Fri), hourly off-hours.

## Feed URL

```
https://raw.githubusercontent.com/jaimenbell/fra-public-feed/main/feed.jsonl
```

## Schema (schema_version=1)

Each line is a JSON object with these fields:

| Field | Type | Description |
|---|---|---|
| `schema_version` | int | Always `1` for this format |
| `ts` | ISO-8601 string | UTC timestamp when signal was emitted |
| `signal_id` | UUID string | Unique ID for deduplication |
| `venue` | string | Exchange identifier (e.g. `binanceusdm`, `okx`) |
| `symbol` | string | CCXT-style symbol (e.g. `BTC/USDT:USDT`) |
| `side` | string | `"long"` or `"short"` |
| `funding_rate_bps` | float | Current funding rate in basis points |
| `entry_price` | float | Reference entry price at signal time |
| `est_apr_pct` | float | Estimated annualised return in percent |

## Example

```json
{"schema_version":1,"ts":"2026-05-23T20:35:01.213248+00:00","signal_id":"bfa9b361-faa1-5179-9df6-303a99d72538","venue":"binanceusdm","symbol":"BTC/USDT:USDT","side":"short","funding_rate_bps":3.5,"entry_price":67500.0,"est_apr_pct":38.32}
```

## Update cadence

- **Market hours** (09:30–16:00 ET Mon–Fri): every 15 minutes
- **Off-hours / weekends**: every 60 minutes

## Disclaimer

Signals are informational only. Not financial advice. Past funding rates do not guarantee future returns. Always do your own due diligence.

## Feed integrity

Every push is gated by an automated sanitization check. The script aborts if any of the following tokens appear in the feed: `api_key`, `secret`, `balance`, `account_id`, `internal_*`. This ensures zero private data leaks into this public repository.
