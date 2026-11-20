# tiny-chess-01 — a chess engine in 4,093 bytes of Python

Entry for the make.fun bounty **tiny-chess-01** ("strongest chess engine in
4 KB of Python"). The whole engine is `main.py` (4,093 bytes, cap 4,096): a
single line that unpacks a compressed payload (base85 + lzma, both standard
library) and executes it. `main.py` reads no other file, per the rules.

## Design

- Negamax alpha-beta with iterative deepening under a fixed 80,000-node
  budget — no wall clock anywhere, fully deterministic (contender trials are
  replayed and must match move for move).
- Transposition table (depth / value / flag / best move) with PV ordering.
- Null-move pruning (R=2), late move reductions, check extensions.
- Quiescence search with check evasions.
- Tapered PeSTO evaluation — midgame/endgame piece-square tables,
  file-mirrored and quantized to fit the byte budget — with incremental
  scoring and Zobrist hashing.
- Repetition avoidance from the game's own position history at the root.

## Local practice results (official kit, local machine)

3-trial mean Elo **1,393** (trials 1,451 / 1,364 / 1,364) on the kit's
6-game practice trials. The house baseline (sunfish 4K, itself an lzma
payload of 3,649 bytes) rates a 1,365 mean on the same trials. Local scores
are indicative only; the staged final uses unseen openings.
