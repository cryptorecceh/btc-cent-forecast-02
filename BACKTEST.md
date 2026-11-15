# Backtest — btc-cent-forecast-02 (regime-adaptive ensemble)

Model: `tilt = +0.05*r240 + 0.10*r1440` when the 24h trend is strong
(`|r1440| / (v240*sqrt(1440)) > 1.0`), else `tilt = -0.10*r15 - 0.05*r60
- 0.05*r240`; guess = last * exp(tilt). All MAE in cents, 240 minutes ahead.

## Data
- **Coinbase BTC-USD 1-minute closes**: 43,341 candles, 16 Oct 2026 04:35 UTC
  to 15 Nov 2026 04:35 UTC (43,341 calls of 300 candles via the public
  exchange API). BTC fell 81,959.11 → 64,904.99 over the window.
- **Kit practice series** (real Orca cbBTC data from the kit): 5,520 closes,
  7 Nov 06:01 → 11 Nov 02:00 UTC, 75,940.07 → 73,545.88.

## Results (162 checkpoints, every 4h, 30 days)
| model | MAE all | MAE fit (Oct 16-Nov 8) | MAE val (Nov 8-15) |
|---|---|---|---|
| persistence | 46,799.9 | 43,494.3 | 56,555.2 |
| v1 fade (submitted entry) | 46,779.6 | 43,762.3 | 55,684.5 |
| fade 0.20/0.10/0.05 | 46,616.5 | 43,660.9 | 55,339.0 |
| **regime (this entry)** | 47,668.6 | 45,755.5 | **53,314.6** |
| momentum 0.10*r240 | 46,876.9 | 43,001.6 | 58,314.0 |

## 48h windows (12 cases each, like the final, sliding every 4h)
| model | 48h MAE all | win% vs persistence | 48h MAE recent 7d | win% recent |
|---|---|---|---|---|
| persistence | 45,714.0 | — | 45,196.1 | — |
| v1 fade | 45,825.0 | 48.3% | 44,627.2 | 72.1% |
| fade 0.20/0.10/0.05 | 45,767.0 | 53.6% | 44,470.7 | 72.1% |
| **regime (this entry)** | 46,613.8 | 33.1% | **43,640.0** | **92.7%** |

## Most recent regime (last 3 days, Nov 12-15; trigger fires 71% of cases)
| model | MAE | wins vs persistence (of 17) |
|---|---|---|
| persistence | 73,960.3 | — |
| v1 fade | 73,361.9 | 8 |
| **regime (this entry)** | **66,895.4** | **11** |

Last 48h window (12 cases, Nov 13-15, closest analog to the final):
**regime 72,454 vs persistence 77,930 (WIN, ~7% better)**.

## Kit practice cases (Orca, Nov 7-11 downtrend)
| model | MAE (5 cases) |
|---|---|
| persistence | 47,054.7 |
| v1 fade | 48,952.7 |
| momentum 0.10*r240 | 46,469.3 (best) |
| **regime (this entry)** | 48,952.7 (momentum branch never fired — vol too high) |

## Why the regime model, given the worse overall MAE
The overall 30-day MAE is worse than persistence because in choppy weeks the
momentum branch fires into reversals. But the final is a single 48h window
starting 17 Nov, two days after this downtrend, and the model is chosen for
the regime the final starts in, not the average: in the recent 7 days it wins
92.7% of 48h windows with a 3.5% MAE edge, and in the last 48h it beats
persistence by ~7%. The fade-only models cap at ~1% — not enough to reach
the paid ranks. This is a deliberate regime bet, documented with the numbers
above, including the cases where it loses.
