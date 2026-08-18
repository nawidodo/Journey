# Interpreter Course — Absolute-Beginner (hello world → your OWN embedded scripting language, gated)

Zero interpreter knowledge assumed. You need: a Mac/Linux box, C (or C++), and the PL-course's frontend skills recommended-but-optional (this course includes its own lexer/parser pass — slighter than PL's, because the star here is interpretation, embedding, and the script↔C bridge). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/iN-quiz.md`)** — no advance without both. ~2–3h/unit, 12 units + capstone ≈ 6–8 weeks.

Compass (re-read when lost): an interpreter **executes a language at runtime, from its tree, with data defined by tags** — no native compilation. The professional position (Lua, Python's reference impl, JS engines originally) is the *embedded scripting interpreter*: a host C program holds a state, feeds script text in, gets values back, and scripts call host functions out. You'll build exactly that — write-vs-compile isn't the question, "host + script live together" is. This is the interpreter shape that powers game engines, editors, and routers you actually use.

Safety: pure engineering, own machine; rules like every course — verification + own-words quiz gate each unit; copy only I0/I7 boilerplate (state scaffold, C-API shim); erase-and-retry once when stuck; 3h timebox; benchmark honestly.

---

## I0 — hello world: host + script, the two worlds
Concept: an embeddable interpreter is a library: `state *s = open(); dostring(s, "print(1+2)"); close(s)`. Do: in C: a minimal state struct + one eval function that handles literal integers only; a host program (your editor tool or a test harness) calls it; print the result host-side; extend to "print" builtin so script can talk out.
Verify: host embeds your interpreter; `dostring` returns a value the host prints; both directions work.
**Lesson check:** what does "embedding" require from host and script — and why is state (not globals) the professional interface?

## I1 — values: everything is a tagged box
Concept: dynamic languages store values as unions + type tags — number, string, bool, nil, table, function. Do: your `Value` union with tags; print/debug toString for each type; arithmetic ops switch on tags (mixed-type rules: number+string = error or coercion — your spec, documented); a small test suite over your box.
Verify: value VSU tests pass; toString prints every type; mixed-op policy documented + enforced.
**Lesson check:** what does a "tag" buy the interpreter — and where does dynamic typing show up physically (runtime checks, not compile time)?

## I2 — the script language "sscript": tokens → tree
Concept: a small dynamic language: statements, expressions, assignment, if/while, function defs — recursive descent is fine here. Do: lexer + parser for sscript (position-carrying tokens; if you've done PL units, port your grammar fast, tweaked for dynamic semantics); S-expression AST dump; 6 golden programs hand-checked.
Verify: AST dumps match hand-written trees; 3 malformed programs rejected with line/col errors.
**Lesson check:** why does the dynamic language's AST look the same as a compiled one — and what differs in *meaning* (types decided when?).

## I3 — the core evaluator: walking with environment
Concept: eval(AST, env) with dynamic dispatch everywhere; env = chain of scopes; errors carry line numbers (a stack trace needs them). Do: evaluator for expressions + statements; scopes (block, function, global); error/value objects with source positions; `factorial`, loops, nested scopes all working; a trace mode printing each step.
Verify: factorial + nested-scope programs pass; a thrown error prints line+message (not a segfault).
**Lesson check:** what's on the stack when an interpreter runs — and why does a good error *position* matter more here than in a compiler?

## I4 — tables: the universal data structure
Concept: dynamic scripts grow one structure that does dict+array+object: hash portion + array portion, with *metamethods* (operator overloading hooks). Do: your table type (open-addressing hash + contiguous array parts, growth policy), indexing reads/writes, length; then metamethods: `__index`, `__add` (script-defined behavior for `t + x`), documented + tested.
Verify: table stress (1M inserts) passes; metamethod demos (overloaded add) run; benchmark table access (notes/i4.md).
**Lesson check:** why do one-size tables win in script languages — and what does `__index` let a script author fake (inheritance, proxies)?

## I5 — closures and functions: higher order becomes real
Concept: functions as values, closures capturing environment, varargs, multiple returns. Do: function values in your Value box; closure env capture (upvalue sharing — the PL4 lesson, now in script form); vararg + multi-return call/return; a `map`/`filter` in sscript using closures.
Verify: make-counter closure counts across calls; varargs + multi-return tests pass; sscript map/filter work.
**Lesson check:** what does a closure physically contain — and how do multiple returns change the call convention you designed?

## I6 — the C barrier: strings, interned and exchanged
Concept: strings cross the host/script line constantly — interning (one pointer = equal strings), string library, and the first structured C-exported functions. Do: string interning table; string ops (len, sub, find, format-lite) in sscript; export 3 C functions to script (print, readfile, time) through a registration API; a script that reads a file and prints lines.
Verify: interned strings compare by pointer; exported C funcs callable from script; file-reading script works.
**Lesson check:** why intern strings — and what's the edge *interface* when script→C (values crossing must be marshalled by whom)?

## I7 — the embedding API: the product moment
Concept: a scripting language ships an API: create state, load file, call script functions with args, get results back, register C funcs — the Lua-shaped surface. Do: your host API: `open/close/load/call/register` over a value stack (push args, call, pop results); write a HOST program (a tiny adventure-game controller or your 24-38 text-editor) that loads a sscript config/behavior file and uses its functions.
Verify: embedding demo runs (host + script file + function calls both ways); C-API surface documented like a header (`include/sscript.h`).
**Lesson check:** what makes an embedding API usable — and why does a value *stack* beat ad-hoc param passing in a C API?

## I8 — the debugger you bake in
Concept: interpreters can trace themselves: execution hooks (line events), stack dumps, breakpoints — for free compared to native debuggers. Do: debug hooks (per-line callback), stack-trace snapshot function, breakpoint table (line → hook), a `dbg` REPL mode (`n` next, `p` print, `bt` backtrace); test by debugging your own fibonacci-with-bug.
Verify: breakpoints + n/p/bt work on a live script; you found a planted bug with only your debugger.
**Lesson check:** why is a line hook "free" for interpreters — and what does a full stack trace require your evaluator to maintain anyway?

## I9 — errors as citizens: protected calls
Concept: production scripts must fail softly: pcall-style protected invocation, error objects, recover at the REPL. Do: protected call (`pcall(f)` returns ok/err, no abort), error() raising objects, formatted traceback (function chain + lines); REPL catches script errors and continues; test a failing script under pcall.
Verify: pcall string + error object demos pass; tracing you wrote in I8 shows in the traceback; REPL survives errors.
**Lesson check:** why must errors be values, not crashes — and what does a traceback need from I3/I8 that must exist *before* it works?

## I10 — the heap police: GC for script data
Concept: script data is interpreter-owned; mark-sweep across your tagged heap (values, tables, closures) frees it; hosts must not hold stale pointers. Do: mark-sweep GC over your objects (root set = state globals + stack + pending calls); stress: 1M-table churn, heap flat (chart); host-ref lifetime rule (register C-held values) implemented + documented.
Verify: GC stress chart flat; register/unregister host refs correct (no UAF in a long host session).
**Lesson check:** why does an interpreter NEED GC where a compiler-written language might not — and what's the rule for pointers crossing host↔script?

## I11 — honest speed: where interpreters stand
Concept: interpreters are slower than compiled — dispatch + dynamic checks; the wins are avoids (interning, table access, tail-respecting design), and the next step is bytecode (PL7's lesson, yours to borrow). Do: benchmark: sscript vs your PL bytecode VM (if built) vs clang C — three columns of honestly timed fib/map; document dispatched-instead-of-compiled slowdown with a per-op estimate; write your bytecode roadmap paragraph.
Verify: benchmark table in `notes/i11.md`; slowdown per-op explained — numbers match intuition.
**Lesson check:** name the three recurring costs of interpreting — and which one does bytecode (not native) eliminate?

## I12 — CAPSTONE: sscript embeds, cold
Prereq: I0–I11. **Close all notes.** Re-create the core evaluator cold (values + eval + env + error in one sitting — the gauntlet discipline), then finish sscript v1: lexer/parser, tables+metamethods, closures, strings+intern, C-API (open/load/call/register), debugger, pcall, GC, REPL, stdlib — and EMBED it in a second program you own (the GAME-ENGINE-COURSE engine for game logic, or 24-38 text-editor for macros). Write `labs/interpreter-capstone.md` like a language maintainer: architecture, three proud decisions, regret, roadmap (bytecode then JIT — PL/LLVM courses' roads), plus the embedding case study.
**Pass = sscript drives a real host program, cold rebuild of the evaluator passes, writeup reads like a maintainer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in I0/I7 boilerplate (state scaffold, C-API shim) — evaluator, tables, GC, debugger written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Own machine; honest benchmarks; the debugger is a tool, not a shame (you wrote it).
5. Honest bar: Lua is 30 years of refinement; this course's bar = your own embedded, embeddable, debuggable, GC'd, C-bridged scripting language driving a real host program, proven cold at the capstone — the floor for interpreter engineering and every "why is this slow / why is this weird" question you'll ever ask about Python/JS/Lua.

## Where this lives
`steps/` unchanged (route: 24-47, 24-09). Sibling map: [`PROGRAMMING-LANGUAGE-COURSE.md`](PROGRAMMING-LANGUAGE-COURSE.md) (whole pipeline by hand, compiler-leaning) and [`LLVM-LANGUAGE-COURSE.md`](LLVM-LANGUAGE-COURSE.md) (native backend) — this course is the *embedded-interpreter* corner: script-into-C, the Lua position. Embed sscript into your [`GAME-ENGINE-COURSE.md`](GAME-ENGINE-COURSE.md) engine at the capstone — one pair of hands writes both the engine and its scripting language.