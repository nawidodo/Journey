# 24-37 · Own chess engine — search + eval, the classic AI build (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../36-own-bittorrent-client`](../36-own-bittorrent-client/README.md) · **Next:** [`../38-own-text-editor`](../38-own-text-editor/README.md)

## Objective
The most fun way to learn search + evaluation: a chess engine. Board representation (bitboards — a classic data structure), move generation, minimax + alpha-beta pruning, evaluation (material, mobility, PSTs), UCI protocol to play against real GUIs. The AI tie-in: this is the same search/eval pattern your 10-15 NN and 24-28 Grover use — "explore a space, score leaves, prune".

## Tasks
- [ ] Board: bitboards (the 64-bit trick — pairs 10-02 SIMD thinking), legal move generation + incremental zobrist hashing
- [ ] Search: negamax + alpha-beta, move ordering, quiescence; iterative deepening
- [ ] Eval: material/PSTs/mobility; tune a couple of weights (your 24-30 profiler finds the hot path)
- [ ] Play: UCI protocol → play against your engine in a GUI; beat it, then understand why it beat you
- [ ] Writeup: search-eval tradeoffs, where modern engines (NNUE — pairs 10-15) changed the game — `notes/`

## Resources
- "Chess Programming Wiki" (the manual); Sunfish source (peer — a tiny strong engine); your 10-15/24-30 notes

## Exit Criteria
- [ ] Engine plays legal chess, UCI-compatible, beats you at depth 4+ — `labs/`
- [ ] Search/eval writeup — `notes/`

## Links
- [Chess Programming Wiki](https://www.chessprogramming.org/)
- [Sunfish](https://github.com/thomasahle/sunfish)
