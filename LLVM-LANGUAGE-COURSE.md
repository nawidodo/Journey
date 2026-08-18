# LLVM-Language Course — Absolute-Beginner (hello world → your OWN language compiled by your OWN frontend onto LLVM, gated)

Zero compiler knowledge assumed. You need: a Mac/Linux box with LLVM installed (`brew install llvm`; use `llvm-config --cxxflags --ldflags` to build), plus C++ (or the language you'll write the frontend in — C++ is the standard path and what this course assumes; the PL course's lexer/parser units are a recommended-but-optional deeper floor). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/llN-quiz.md`)** — no advance without both. ~3h/unit, 12 units + capstone ≈ 8–10 weeks.

Compass (re-read when lost): a compiler is a frontend (your text → your tree) and a backend (anything → machine code). **You write the frontend — the language — and borrow the backend: LLVM accepts "LLVM IR", optimizes it, and emits native code (or JITs it).** Most production languages do exactly this (Rust, Swift, Clang itself). The whole course is: learn IR, then emit it from your own lexer/parser/type-checker, then ride LLVM's optimizer, then JIT a REPL. By the capstone you've written a language with register allocation, optimization, and native codegen *given to you for free by a library* — which is the professional move, not the cheat.

Safety: pure engineering, own machine; rules like every course — verification + own-words quiz gate each unit; copy only LL0/LL10 boilerplate (CMake scaffold, test harness); erase-and-retry once when stuck; 3h timebox; benchmark honestly.

---

## LL0 — hello world: meet the backend
Concept: LLVM is a library of compiler backends; its input is a text IR you can read. Do: install LLVM; compile `hello.c` (and hello in C++) with `clang -S -emit-llvm`; read the `.ll`: module, function, basic blocks, `load/store/call/ret`; edit it by hand (change a constant) and re-run with `lli` (interpreter) and `llc` (native object); toolchain verified.
Verify: your edited `.ll` runs and prints something you changed; `lli` and `llc` both work end-to-end.
**Lesson check:** where is the "compiler" in an LLVM toolchain — what does your future compiler contribute vs what does LLVM contribute?

## LL1 — IR literacy: types, SSA, control flow
Concept: LLVM IR = strongly typed, SSA-form (each value assigned once), explicit control flow with `br` and `phi`. Do: write `fib.ll` BY HAND (recursion + branch + phi for loop version); run via `lli`; dump with `opt -S` after running passes to see how it changes; hand-write an if/else with phi merging two paths.
Verify: hand-written fib prints correct values; phi merges values correctly in your if/else IR.
**Lesson check:** what does SSA guarantee — and why must a `phi` exist where two control paths meet a value?

## LL2 — your frontend: lexer + parser
Concept: a language starts as text → tokens → AST; recursive descent is the readable grammar. Do: lexer (identifiers, numbers, keywords, positions) + recursive-descent parser (expressions with precedence, assignments, if/while, function defs) for tiny language **"lango"**; S-expression AST dump; precise errors with line/col. (If you've done PL units, this is a fast re-run in C++; if not, take the time here — it's the only hand-written part.)
Verify: AST dumps match hand-written trees for 6 programs; malformed input → position-accurate errors.
**Lesson check:** what is the frontend's output contract — what must the AST contain for a backend to become possible?

## LL3 — codegen 1: expressions become IR
Concept: codegen walks the AST and emits IR via LLVM's C++ API (IRBuilder): values flow as SSA registers. Do: build the pipeline lango→IR for expressions: literals, arithmetic, comparisons; `extern printf` + a `main` calling it; emit IR, `llc` to native, link, RUN the binary.
Verify: your binary prints computed expression results matching expectations; emitted IR is readable and valid (`opt -verify`).
**Lesson check:** what is IRBuilder doing that you don't have to — and why does expression codegen come out SSA naturally?

## LL4 — codegen 2: variables and functions
Concept: variables are `alloca` slots with load/store (the IR-friendly lie); functions are declarations with argument slots. Do: variable declarations + assignment + read; user-defined functions with params, calls, returns; run multi-function programs (gcd, factorial); inspect IR — see the allocas and why they're "unoptimized".
Verify: factorial/gcd programs compile and run correctly; `opt -O2 -S` visibly changes the IR (mem2reg promoting your allocas).
**Lesson check:** why do variables live in memory (alloca) instead of registers — and what does mem2reg do to that choice?

## LL5 — control flow: if, while, and the phi
Concept: branches + merges + phis: the real language shape. Do: codegen if/else and while/for from your AST to basic-block IR with phi nodes for merged values; benchmark fib-compiled vs fib-interpreted (if you did PL) — expect the "wow" — then vs `clang` C fib, honestly.
Verify: control-flow programs correct; benchmark table (`notes/ll5.md`) compiled vs interpretive vs clang.
**Lesson check:** walk one loop through IR — where does the back-edge go and what does phi reconcile at the loop head?

## LL6 — types: check before you emit
Concept: garbage in → garbage IR; types are a frontend pass that protects the backend. Do: type checker (int/float/bool/string) over your AST before codegen: infer, verify, error-with-position; then emit IR respecting types (int vs float ops differ); reject the classic misuse set (string+int, calling non-function).
Verify: 5 planted misuse programs rejected before IR; a valid mixed program compiles with correct typed IR.
**Lesson check:** why catch type errors in the frontend rather than let the backend fail — and what's the difference between checking and *inferring*?

## LL7 — the optimizer tour: you get this for free
Concept: opt runs passes over IR: mem2reg, instcombine, loop unrolling, vectorization — LLVM's decades of research, one flag away. Do: pipeline `opt -O2` on your fib/main IR: dump before/after; time the -O0 vs -O2 runtimes; write the *pass pipeline notes* — which pass did the most visible work on your program?
Verify: -O2 binary measurably faster (runtime table); your pass-notes identify the dominant transform.
**Lesson check:** name one pass and what invariant it restores — and why is "your program got faster" sometimes free (semantics preserved)?

## LL8 — functions as values: closures on a real backend
Concept: closures need captured state — the classic indirection trick: a closure struct (fn ptr + captured env). Do: first-class function references (function pointers in IR — easy); then closures: compile `make-counter` → create a struct {fn ptr, env}, float env captures, emit indirect calls through the struct; document the "trampoline-free" `lango` design.
Verify: make-counter counts across calls through compiled closures; IR dump shows the struct + indirect call.
**Lesson check:** what does a closure capture (value vs env) and why does a struct of {fn, env} solve it in a native backend?

## LL9 — strings and a runtime: calling out
Concept: strings are data + length (`i8*` + size or i32 pool), and your language links against libc/runtime code for what the frontend shouldn't rebuild (printf, strlen) plus your own C helpers. Do: string literals pooled in IR (`@.str` globals), length-carrying string value, string concat via a `lang_concat` C helper YOU write and link; format/print via printf.
Verify: string concat/print/len programs run; linker story (your helper + LLVM obj) documented in Makefile.
**Lesson check:** why does a language with a backend still write *some* runtime C — and where's the line between frontend, runtime, and backend?

## LL10 — debugging and the toolchain wars
Concept: errors, verifier, flags: professional compilers differ by DX. Do: `llvm::verifyModule` in your pipeline; IR-dump flag (`-emit-ir`), -O0/-O2 flags, error messages with source context; golden-test suite (program → expected output, `lango test`); compare your -O2 vs `clang -O2` on the same program, honest table.
Verify: `lango test` green; your error messages carry line/col; benchmark table fair.
**Lesson check:** which DX pieces cost nothing but make a language feel real — and why is a golden-test suite the backbone of a compiler?

## LL11 — JIT: the REPL compiles
Concept: LLVM's JIT (ORC) compiles IR to native at runtime: your language becomes interactive *and fast*. Do: ORCJIT path: read source line → frontend → IR → JIT → call the function → print result; build the lango REPL on the JIT; define-and-call functions interactively; watch the same REPL run fib lazily-fast.
Verify: REPL evaluates lines + function defs through the JIT; timing shows JIT-compiled fib ≈ native (table).
**Lesson check:** what does JIT compile *when* — and why must your IR be valid before JIT (errors surface where)?

## LL12 — CAPSTONE: lango ships, cold
Prereq: LL0–LL11. **Close all notes.** Re-create the codegen core cold (yur AST → IR for expressions + control flow + functions in one sitting — the gauntlet discipline), then finish lango v1: lexer, parser, type check, codegen, -O0/-O2, verifier+IR dump, JIT REPL, golden tests, runtime helpers — and write TWO real programs in lango (a raytracer-lite that prints an image + a fib/closures demo). Write `labs/llvm-capstone.md` like a compiler engineer's post: architecture, the three design decisions you're proudest of, regret, roadmap (self-hosting compilers is the stretch — from PL/PROGRAMMING-LANGUAGE-COURSE work, know why).
**Pass = lango compiles programs you wrote to native binaries + JIT REPL works; IR valid under -O2; writeup reads like an engineer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in LL0/LL10 boilerplate (CMake+harness scaffold) — lexer, parser, type checker, IR emission written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Own machine; honesty about benchmarks (LLVM's optimizer will beat naive IR — the lesson is understanding *why*, not being embarrassed).
5. Honest bar: LLVM is decades of team engineering — the professional move is *not* writing a backend; this course's bar = your own end-to-end language (frontend by you, IR by you, optimization+native+JIT by your usage of LLVM), proven cold at the capstone — the floor for frontend/compiler engineering and the exact architecture of Rust, Swift, and every modern compiler you'll read.

## Where this lives
`steps/` unchanged (route: 24-47, 24-09, 24-87, 02-17). The sibling [`PROGRAMMING-LANGUAGE-COURSE.md`](PROGRAMMING-LANGUAGE-COURSE.md) builds the whole pipeline by hand (bytecode VM, GC, JIT sketches) — run it before or after; together they give you both views: the backend WHOLE-cloth and the backend BORROWED. Feeds everything: every interpreter you read, every language you adopt.