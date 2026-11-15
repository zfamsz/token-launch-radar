# token-launch-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/zfamsz/token-launch-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/zfamsz/token-launch-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Launch Radar: a dashboard that ranks pre-token protocols by implied value. It reads a sealed snapshot of every protocol DefiLlama tracks, works out which ones still record no token and are not a wrapped or staked product of one that trades, and ranks them by what their launch would be worth — priced from the market itself, with each category's median market-cap-to-TVL ratio among the protocols that already trade. Each row carries a seven-step farm plan with costs and times.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` runs on Node 22 with the standard library only. It writes `board.json` (the ranked board), `data.js` (the dashboard's data), `dashboard/` and `screenshots/` to the output directory, and `dossier.json` (the summary the judge reads). Run it twice: both runs write the same bytes.

## The model (radar-2)

```
implied = round(tvl_usd x multiple(category) x growth x maturity)
```

- `multiple(category)` — the median market-cap-to-TVL ratio of the live-token protocols in the row's category, computed from the same snapshot; a category with fewer than 3 live comparables uses the global median, and every multiple is clamped to [0.05, 1.50].
- `growth` — `1 + clamp(change_7d, -60, 120) / 100 x 0.5`.
- `maturity` — `0.6 + min(age_days, 1095) / 1095 x 0.4`; an unlisted age uses 365 days.

The board covers the snapshot's whole watchlist (40 protocols at or above the $50M floor with no token on record) plus every rankable pre-token protocol below the floor — 178 rows in all. A protocol the watchlist leaves out above the floor is left out too: the house's rule kept it out for a parent protocol the snapshot does not name. Live-token protocols are set aside with their market caps shown, and their ratios are the comparables the board is priced with.

Implied value is the entry's own claim about what a token would be worth if the protocol's record stayed as it is. It is not a prediction of whether or when a token is issued, and never a price.

---

<sub>Entered on [make.fun](https://make.fun/?ref=pdwrq5m4&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
