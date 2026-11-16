# tiny-chess

A 4 KB chess engine in a single Python file (`main.py`, 3,217 bytes as packed
with the kit's `makefun compress`), written for the make.fun tiny-chess-01
competition: strongest engine in 4 KB of Python.

## Practice results

Three 6-game practice trials against the kit's house gauntlet (kit env server
+ official scorer): trial-0 Elo 1279, trial-1 Elo 1099, trial-2 Elo 1099,
mean 1159. Below the Sunfish house baseline (~1325), so a prize is unlikely;
the entry is a tested, deterministic first entry, not a contender claim.

The largest leak investigated was repetition draws in winning positions.
Three variants were tried and rejected (demote-repeat, re-run root without
the repeat, 120k nodes): at depth 4-5 the search cannot tell a convertible
+285 from a dead +285, so the draws are the best result this engine reaches
there, and a bigger node budget only plays differently, no better.

## Design

- 0x88 board, negamax with alpha-beta and iterative deepening to a fixed
  80,000-node budget. Deterministic: no wall clock, no randomness.
- Quiescence search (captures; all evasions when in check), transposition
  table, killer moves, MVV-LVA capture ordering.
- Null-move pruning (R=2, only with non-pawn material and static eval at or
  above beta) and a check extension.
- Evaluation: material + piece-square tables, bishop-pair bonus, and a
  phase-based endgame king table.

## Files

- `main.py` — the whole engine. It is an LZMA payload plus a small unpacker;
  compressed payloads are explicitly allowed by the competition rules.
- `LICENSE` — MIT.
- `.makefun/owner.json` — payout wallet.

No other files are read, imported, opened, or executed. Standard library only.
