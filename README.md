# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Baroosb/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Baroosb/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in 4,093 bytes of Python. The whole engine is `main.py`
(4,093 bytes, cap 4,096): two lines that unpack a compressed payload
(base85 + lzma, both standard library) and execute it. `main.py` reads no
other file, per the rules.

## Design

- Negamax alpha-beta with iterative deepening under a fixed 60,000-node
  budget — no wall clock anywhere, fully deterministic (contender trials
  are replayed and must match move for move). The budget is sized so a
  whole 8-game trial stays inside the 1,800-second wall limit.
- Transposition table (depth / value / flag / best move) with PV ordering.
- Null-move pruning (R=2), late move reductions, check extensions.
- Quiescence search with check evasions.
- Tapered PeSTO evaluation — midgame/endgame piece-square tables,
  file-mirrored and quantized to fit the byte budget — with incremental
  scoring and Zobrist hashing.
- Repetition avoidance from the game's own position history at the root.

## Local practice results (official kit, local machine)

3-trial mean Elo **1,394** (trials 1,279 / 1,451 / 1,451) on the kit's
6-game practice trials. The house baseline (sunfish 4K, itself an lzma
payload of 3,649 bytes) rates a 1,365 mean on the same trials. Local scores
are indicative only; the staged final uses unseen openings.

## Run it

```sh
python3 main.py
```

The engine is driven by the competition kit's environment: it reads
`MAKEFUN_ENV_URL` and plays each game over HTTP (`POST /game`,
`POST /move`) until every game is over.

---
<sub>Entered on [make.fun](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
