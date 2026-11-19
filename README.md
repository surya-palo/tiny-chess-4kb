[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/surya-palo/tiny-chess-4kb/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/surya-palo/tiny-chess-4kb/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-4kb

A chess engine in at most 4,096 bytes of Python, built for make.fun's
"Build the strongest chess engine in 4 KB of Python" bounty. `main.py` is the
whole engine: it plays the trial through the make.fun environment, talking UCI
moves over HTTP. It is an original design, written from scratch for this
bounty — not a fork of any existing engine.

## How it plays

- **Board:** 120-square mailbox, two-colour move generation, legal moves
  filtered by make/unmake plus a king-attack check.
- **Search:** negamax alpha-beta with iterative deepening to a fixed node cap
  (deterministic — no clock, no randomness), principal variation search at the
  root, late-move reductions for quiet late moves, quiescence on captures and
  promotions, MVV-LVA plus killer-move ordering, and a transposition table.
- **Evaluation:** material plus piece-square tables, a king table tapered
  between middlegame and endgame by non-pawn material, and a bishop-pair bonus.
- **Game sense:** it resigns a position that is lost beyond recovery, and it
  tracks threefold repetition — it avoids repeating a position when it is
  winning, and takes the draw when it is losing.
- **Limits:** Python 3.13 standard library only, `main.py` alone, at most
  10 seconds a move. The source is packed with the kit's `compress` command;
  the readable source is `engine.py` (kept beside the entry, not committed).

## Local practice results

Graded with the kit's own environment server and scorer on the three practice
trials (six games each against the house ladder, anchors 855–1596):

| Trial | Result | Elo |
| --- | --- | --- |
| trial-0 | 3 wins, 1 draw, 2 losses | 1364 |
| trial-1 | 1 win, 1 draw, 4 losses | 997 |
| trial-2 | 0 wins, 3 draws, 3 losses | 997 |
| **mean** | | **1119** |

The practice floor for the final is Elo 1154 (mean over practice trials); the
house baseline (sunfish's 4 KB engine) rates about 1365 on the same six-game
draw. At the current strength this entry is likely to finish below the floor.

## Build

`main.py` is produced from `engine_golfed.py` by the kit's compress command, run as a
command of its own after the last source change:

```
node cli/makefun.mjs compress engine_golfed.py --out main.py
```

<sub>Entered on [make.fun](https://make.fun/?ref=5zv4vr8w&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
