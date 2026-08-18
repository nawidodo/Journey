# 24-47 · Own JS interpreter — tokens → AST → eval, the language-runtime gap (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../46-own-mqtt-broker`](../46-own-mqtt-broker/README.md)

## Objective
Your 02-17 WASM runtime runs compiled code; here's the other half — a language runtime. Build a JS interpreter: tokenizer, parser (Pratt/precedence — your 24-09 compiler skills), AST, tree-walking eval with closures, prototypes, garbage collection (mark-sweep — pairs 24-16 allocator). The security tie: interpreter bugs are browser-exploit fuel (pairs 08 track); you'll understand V8's job by writing its baby cousin.

## Tasks
- [ ] Lexer: tokens (identifiers, numbers, strings, operators); parser: expressions (Pratt), statements, functions — error messages you'd actually want
- [ ] Eval: tree-walking with environments (scopes), closures (the capture semantics), prototypes + `this` (the JS-specific weirdness)
- [ ] GC: mark-sweep over your object graph (pairs 24-16); measure pause (your 24-30 profiler)
- [ ] Lab: run real JS programs (fib, closures, a tiny object system); benchmark vs Node (the honest number) — `labs/`
- [ ] Writeup: where JS engines diverge (JIT tiers, hidden classes — pairs 02-17, 08-01) — `notes/`

## Resources
- "Crafting Interpreters" (the manual — JS-flavored version); V8 blog posts (peer); your 24-09 + 02-17 notes

## Exit Criteria
- [ ] Interpreter runs closures/prototypes/GC'd programs — `labs/`
- [ ] Benchmark + engine-divergence writeup — `labs/` + `notes/`

## Links
- [Crafting Interpreters](https://craftinginterpreters.com/)
- [V8 blog](https://v8.dev/blog)
