[![make.fun](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/memelljk/gta6-twitch-forecast/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/memelljk/gta6-twitch-forecast/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01)

# GTA VI launch-day Twitch viewers, hour by hour

Entry for [make.fun — Forecast GTA VI's launch-day Twitch viewers hour by hour](https://make.fun/bounty/gta6-twitch-ccu-forecast-01) (`gta6-twitch-ccu-forecast-01`).

`main.py` (Python 3.13, standard library only) reads one case file
(`/input/<id>.in`: hour 1's print plus the launch curves of past big games
the case carries as comparables) and writes `/output/<id>.out`: for each of
hours 2–24 of release day, five quantiles (5%, 25%, 50%, 75%, 95%) of that
hour's Twitch concurrent viewers in the Grand Theft Auto VI category.

## Method

Launch days are not flat. A console game that unlocks at rolling midnight
local time starts its UTC day with only the earliest timezones live, then
climbs as each region unlocks toward a prime-time peak — and how steep that
climb is depends on the launch's size: the bigger hour 1 already is, the
flatter the rest of the day. So for each hour we regress, across comparable
rolling-midnight launches, `log(viewers_h / viewers_1)` on `log(viewers_1)`,
clamp the target's size to the range the pool covers (no launch in the pool
is as big as GTA VI will be, and extrapolating the fit past the data would
predict a falling day, which no rolling launch ever shows), and take the
spread from the regression's residuals. Simultaneous-global-unlock games and
early-access-contaminated launches are left out of the pool. See the
docstring at the top of `main.py` for the full reasoning.

## Local validation (practice cases, scored exactly as the rules define)

| case | this entry | house baseline |
| --- | ---: | ---: |
| Marvel's Wolverine | 0.198 | 0.744 |
| Red Dead Redemption 2 | 0.110 | 0.287 |
| God of War Ragnarök | 0.125 | 0.215 |
| Marvel's Spider-Man 2 | 0.227 | 0.303 |
| **mean pinball loss** | **0.165** | **0.387** |

Deterministic (two runs byte-identical), ~0.3 s per case (limit 8 s).

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

## License

MIT — see [LICENSE](LICENSE).
