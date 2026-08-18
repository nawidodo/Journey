# 24-63 · Own raycaster — Wolfenstein-style engine from your renderer (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../62-own-socks-proxy`](../62-own-socks-proxy/README.md) · **Next:** [`../64-own-hd-wallet`](../64-own-hd-wallet/README.md) · **Pairs:** 10-05, 10-13, 24-38

## Objective
The fun capstone for your graphics arc (10-05 software renderer, 10-13 path tracer): a playable raycaster — the engine that made FPS possible. Ray-vs-grid traversal (DDA — the same algorithm family as your 10-13 tracer), textured walls (your 24-27 PNG textures), sprites, the player loop (24-38 input handling), and the fixed-point/timing discipline (pairs 24-30 profiler, 24-25 math). Then the perf lab: profile the inner loop (24-30) and SIMD it (10-02) — the measurable speedup is the payoff.

## Tasks
- [ ] DDA: ray-vs-grid cast (the Lodev walkthrough), wall distance + column height, the fisheye fix
- [ ] Render: textured walls (24-27/35 decode → columns), floor/ceiling flat, sprites (billboarded)
- [ ] Game: player movement + collision, a real map, minimap (your 10-05 2D skills); input via 24-38-style loop
- [ ] Perf lab: profile inner cast/render loop (24-30 flamegraph), SIMD/optimize (10-02), the before/after FPS table — `labs/`
- [ ] Self-check: a level you can walk through; FPS at target (pairs 10-09 async/graphics targets)

## Resources
- Lodev raycaster tutorial (the manual); Wolf3D engine writeups (peer); your 10-05/10-13/24-30 code

## Exit Criteria
- [ ] Playable textured raycaster at target FPS — `labs/` + `code/`
- [ ] Profile→optimize speedup table — `labs/` + `notes/`

## Links
- [Lodev raycaster](https://lodev.org/cgtutor/raycasting.html)
- [Wolfenstein 3D engine](https://en.wikipedia.org/wiki/Wolfenstein_3D_engine)
