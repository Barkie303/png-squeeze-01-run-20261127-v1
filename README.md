[![make.fun · png-squeeze-01-run-20261127](https://make.fun/badge/png-squeeze-01-run-20261127.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)
[![rank](https://make.fun/badge/png-squeeze-01-run-20261127/Barkie303/png-squeeze-01-run-20261127-v1/rank.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127/leaderboard)
[![reward](https://make.fun/badge/png-squeeze-01-run-20261127/Barkie303/png-squeeze-01-run-20261127-v1/reward.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)

# Squeeze screenshots into the smallest PNG

A lossless PNG re-encoder for 1280×720 screenshots, written in Node 22 with
the standard library only. It decodes the stored input PNG to RGBA, then:

- **Colour-type reduction** — palette (≤256 colours, ordered by count or
  luminance, transparent entries first), grey / grey+alpha, RGB / RGBA, with a
  tRNS key when a single fully-transparent value exists. This is the biggest
  win on flat and grey screenshots.
- **Per-row filter choice** — fixed filters, a minimum-sum heuristic, a
  fewest-pairs heuristic, or brute force (each row's filter chosen by its real
  deflate cost against the previous 32 KiB), screened per candidate.
- **DEFLATE** — every candidate is screened at zlib level 6, the best one or
  two are re-deflated at level 9 with the default and filtered strategies,
  and the smallest file wins.

Everything is deterministic: fixed work, no clock, no randomness, so two runs
on the same set write byte-identical outputs. One run answers all 8 instances
in about 15 seconds on this machine, well inside the 60-second budget.

Practice-set score: **703,094** (house baseline fixed-paeth: 420,399).
