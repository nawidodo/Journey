# 24-93 · Own Sokoban solver — A* with deadlock pruning, the fun search capstone (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../92-own-fft-visualizer`](../92-own-fft-visualizer/README.md) · **Pairs:** 24-41, 24-37, 24-14

## Objective
The search algorithms from 24-41 meet a real puzzle: solve Sokoban. Build a solver — state space (positions + box states), A* with a good heuristic (box-to-goal Manhattan distance, the admissible design — pairs 24-37 evaluation thinking), deadlock detection (corner/corridor pruning — the part that separates solvers from crawlers), and IDA* for memory. Then the honest test: a solver that beats you on a board you design; and the metagame — generate your own levels (reverse push — pull boxes out of solved positions, the classic generator trick). TUI play via 24-14 if you want to watch it think.

## Tasks
- [ ] State: player + box coords, move generation (push/walk), canonical hashing (24-42-style)
- [ ] A*: heuristic (sum of box-to-goal distances, admissible), open set + priority queue (24-16/24-42)
- [ ] Deadlocks: static (corner/edge traps) + dynamic pruning — measure nodes saved (24-30 sampling)
- [ ] IDA*: iterative deepening for deep boards; the memory-vs-time table
- [ ] Generator: reverse-pull levels from solved states (the fun trick); tune difficulty by box count
- [ ] Lab: solve 3 published boards, report nodes/frontier size vs death-first heuristic (the comparison table) — `labs/`

## Resources
- The Sokoban solvers' lore (YASS/boxpusher docs — the manual); your 24-41/24-37 code

## Exit Criteria
- [ ] Solver finishes 3 boards; deadlock pruning measured — `labs/` + `code/`
- [ ] Solver-design writeup (heuristics, pruning, IDA*) — `notes/`

## Links
- [Sokoban](https://www.sokoban-online.de/)
- [Box storage / solver references](https://github.com/robinhouston/sokoban)