# Token Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/rayarvan77/token-launch-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/rayarvan77/token-launch-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Which pre-token protocol is worth farming next — and what do you do about it?

Token Radar reads a sealed snapshot of the protocol universe (DefiLlama, 2026-11-14),
finds every genuine pre-token launch candidate, ranks them by an implied value, and
shows a farm plan for each one. Built for the [make.fun](https://make.fun) bounty
**token-radar-01** ("Build the best token launch radar").

## The model (radar-2)

```
implied = round( tvl_usd x 0.25
  x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
  x (0.6 + min(age_days, 1095) / 1095 x 0.4)
  x (1 + 0.04 x min(chains - 1, 5)) )
```

- **TVL** is the size of the prize a token would divide.
- **Seven-day growth** is the momentum the market is already paying for.
- **Age with no token** is how long the protocol has run without issuing one.
- **Chain breadth** adds up to 20% for protocols live on more chains.

A protocol is a launch candidate when it records no token **and** is not a wrapped,
staked or bridged product of a token that already trades, not a versioned pool, and
not an exchange. The house watchlist (40 protocols) is the floor: the board covers
all of it, plus every other protocol that passes the same rule — 198 rows in all.
Live-token protocols are set aside with their market caps shown. No protocol name
is ever special-cased.

## Run it

```bash
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` (Node 22, standard library only) writes `board.json`, `data.js`,
`dashboard/`, `screenshots/` and `dossier.json` into the output directory.
Open `out/dashboard/index.html` in a browser — the page renders with no network.

## The dashboard

- A board of 198 pre-token protocols ranked by implied value, sortable and
  filterable by search, category, chain and TVL.
- A detail view per protocol: the model's breakdown, why it ranks where it does,
  and a six-step farm plan with cost and time.
- A model sandbox: move the base-multiple and growth-weight sliders and the
  board re-ranks live from the same figures.
- A live-token view, kept apart from the ranking, because a market cap is a
  fact, not a model.

## Honesty note

A protocol on this board records no token in DefiLlama — that is not proof it
has none, and not a promise it will issue one. Implied value is this radar's
own claim from the snapshot's figures, never a price.
