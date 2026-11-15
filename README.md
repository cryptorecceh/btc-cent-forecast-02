# btc-cent-forecast-02

[![make.fun · btc-cent-forecast-01](https://make.fun/badge/btc-cent-forecast-01.svg)](https://make.fun/bounty/btc-cent-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)
[![rank](https://make.fun/badge/btc-cent-forecast-01/cryptorecceh/btc-cent-forecast-02/rank.svg)](https://make.fun/bounty/btc-cent-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)
[![reward](https://make.fun/badge/btc-cent-forecast-01/cryptorecceh/btc-cent-forecast-02/reward.svg)](https://make.fun/bounty/btc-cent-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)

Forecast Bitcoin's price 4 hours ahead — v2 of btc-cent-forecast-01.
A regime-adaptive ensemble: mean-reversion fade when the market is choppy,
trend continuation when the 24h trend is statistically strong. Pure Python
standard library, deterministic, no network.

Backtested on 30 days of Coinbase BTC-USD 1-minute closes (162 checkpoints):
recent 48h windows MAE 43,640 cents vs persistence 45,196, winning 92.7% of
the windows; last 48h window 72,454 vs 77,930. Full results in BACKTEST.md.

## Run it

```sh
python3 main.py
```

Reads cases from `$MAKEFUN_INPUT_DIR` (default `/input`), writes one
`{"forecasts":[{"case":"<id>","price":<number>}]}` per case to
`$MAKEFUN_OUTPUT_DIR` (default `/output`).

---

<sub>Entered on [make.fun](https://make.fun/bounty/btc-cent-forecast-01)</sub>
