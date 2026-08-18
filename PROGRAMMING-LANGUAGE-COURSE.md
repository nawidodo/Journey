# Programming-Language Course — Absolute-Beginner (hello world → your OWN working language, gated)

Zero compiler knowledge assumed. You need: a Mac/Linux box and one language you're fluent in for the toolchain (C/C++ or Rust recommended; Swift fine). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/plN-quiz.md`)** — no advance without both. ~3h/unit, 12 units + capstone ≈ 8–10 weeks. This is the "how does any language work" skeleton key: after this course, every parser error, every debugger, every "why is it slow" reads differently. The repo already owns the raw materials (24-47 JS interpreter, 24-09 C compiler, 24-87 linker) — this course builds the whole pipeline in order and then hands you those steps as second winds.

Compass (re-read when lost): a language is **text → tokens → tree → (meaning) → execution**: lexer splits chars into words, parser builds a tree, the evaluator either walks the tree (interpreter) or compiles it to instructions (VM/compiler). Exactly one idea per stage. Everything else — types, closures, GC, tooling — is a subsystem of one stage. You will build every stage; by the capstone you'll have the entire map.

Safety: pure engineering, no security surface beyond your own machine; rules like every course: verification + own-words quiz gate each unit; copy only PL0/PL10 boilerplate (REPL scaffold, test harness); erase-and-retry once when stuck; 3h timebox.

---

## PL0 — hello world: the pipeline in miniature
Concept: a language is a pipeline: source → tokens → tree → evaluation. Do: write a REPL: read a line, run it, print the result; make the first grammar "add/subtract numbers" by hand: parse `1+2*3` respecting precedence (even crudely), tree-print it, evaluate it.
Verify: your REPL evaluates `1+2*3` = 7 (correct precedence) and prints the interim tree.
**Lesson check:** name the four pipeline stages — and what would break if you swapped two of them?

## PL1 — the lexer: words from text
Concept: lexing = characters → tokens (identifiers, numbers, strings, operators, keywords) with positions for good errors. Do: hand-written lexer (no regex): tokenize a tiny source file, each token tagged (type, text, line/col); unknown char → your own error with position; test over 10 tricky inputs (nested quotes, weird spacing).
Verify: lexer output matches a hand-annotated golden for your test file; errors point at the right line/col.
**Lesson check:** why does a lexer track line/col at all — and what is the token's job boundary compared to the parser's (what's still unresolved at token stage)?

## PL2 — the parser: tree from words
Concept: parsing = tokens → AST via grammar rules; recursive descent is the readable one. Do: recursive-descent parser for your tiny language: expressions (add/mul/parens, precedence climbing), statements (assign, if, while, print); dump the AST in S-expression form (`(+ 1 (* 2 3))`); malformed input → precise error with position.
Verify: AST dumps match hand-written trees for 6 programs; your parser rejects 3 malformed ones with position-accurate errors.
**Lesson check:** what does "precedence" mean *inside* the parser — and where does grammar ambiguity get resolved (hint: the recursion structure, not magic)?

## PL3 — the interpreter: walking the tree
Concept: evaluation = walk the AST carrying an environment (variable bindings); branches and loops flow through it. Do: tree-walking evaluator: numbers/variables/if/while/block scopes; implement `factorial(10)` in your language and run it; add a trace mode (each step printed) to debug your own interpreter.
Verify: your interpreted `factorial` and a loop-sum match expected values; scope test (nested blocks) passes.
**Lesson check:** what is an "environment" and where does it live during a nested loop — and why does that become the closure problem next unit?

## PL4 — functions and closures: the hard truth
Concept: functions need frames (arg locals) and closures need *captured environments* — environments outlive their call. Do: function definitions + calls with argument frames; closures: `make-counter` returns a function that increments; recursion (fib); print the "upvalue" behavior (captured variable shared across calls).
Verify: fib(10) correct; make-counter counts across calls (1,2,3); recursion depth tested.
**Lesson check:** what does a closure capture — a value or an environment — and why does that difference make or break shared state?

## PL5 — types: the checker that catches your bugs
Concept: types are a static pass over the AST saying what fits where; dyn vs static is a design choice. Do: add a type checker pass: infer/annotate int/float/bool/string on expressions, verify statements, report type errors with positions; choose dynamic-with-runtime-checks OR static — but make the pass explicit and document why you chose it.
Verify: type checker flags 5 planted misuse programs (string+int etc.); your language's own type rules documented in `notes/pl5.md`.
**Lesson check:** what does a type checker prevent that runtime checks don't — and what does it cost you (expressiveness, compile time)?

## PL6 — memory 1: your own garbage collector
Concept: objects must be freed; reference counting is simple-and-cycles-problem; mark-sweep is classic-and-pausing. Do: implement an object heap with mark-sweep GC across your interpreter (or reference counting with a cycle-weakref note); stress: allocate a million tiny objects in a loop, watch heap stay flat; exercise the cycle case (self-referential) and document behavior.
Verify: heap-profiling chart shows GC keeping memory flat; cycle test documented honestly.
**Lesson check:** mark-sweep vs refcounting — what does each miss, and why does "pause" matter for games specifically?

## PL7 — the bytecode VM: your language gets fast
Concept: compile the AST to a stack-based instruction set; a tight loop executes it — the classic interpreter-speed leap. Do: bytecode compiler (AST → opcodes: push, add, call, jump, load/store) + stack VM loop (switch dispatch) + a disassembler for debugging; run fib on the VM vs the tree-walker — benchmark table.
Verify: VM fib correct AND faster than tree-walk (honest numbers in `notes/pl7.md`); disassembler output legible.
**Lesson check:** why is a fixed instruction stream faster than tree-walking — and what's the trade for that speed (compile time, complexity)?

## PL8 — deeper execution (reading the frontier)
Concept: the next leaps are JIT (compile hot code to native at runtime) and direct-to-native compilation — both build on your VM. Do: read-only/stretch: watch the WASM-runtime step (02-17) for the interpreter→JIT transition; implement ONE JIT-lite if time (compile a tiny expression to x86/arm64 shellcode via 24-110's playground — optional); otherwise write the JIT design sketch with the cache-invalidation problem named.
Verify: `notes/pl8.md` — JIT design sketch (or working JIT-lite) + the invalidation problem explained.
**Lesson check:** what does a JIT compile — and why must it invalidate code when the world changes (re-GC, self-modification)?

## PL9 — the language grows up: strings, errors, modules
Concept: languages ship batteries: strings, error handling, multi-file programs, a stdlib. Do: string type + concatenation/len (own builder); error handling (your choice: try/catch or result-return — implement + document); module system: import another file (search path, one-time load); stdlib start: print, len, range, parseInt.
Verify: a 3-file program imports, uses strings, throws and catches, runs correctly.
**Lesson check:** why do languages pick between exceptions and results — and what does "one-time module load" protect against?

## PL10 — tooling: REPLs, tests, and the developer loop
Concept: a language without a REPL and tests is a toy; a language with them is a tool. Do: REPL with history + multi-line input; golden-test harness (program → expected output files, run all, diff); formatter-lite (canonical spacing of your syntax); a `mylang` runner binary with flags.
Verify: `mylang run program.ml` and `mylang test` work; golden suite green; REPL usable.
**Lesson check:** which three tooling pieces changed your own debugging experience most — and why is the golden-test harness the backbone?

## PL11 — the transpiler afternoon: your AST → C
Concept: the final leap of the map: your language → C (gcc handles the rest) — you've built a real compiler now. Do: emit C from your AST for a subset (fib, loops, functions (non-closure)); compile with gcc, run, benchmark vs your VM (C wins — that's the lesson, not a failure); document which features blocked emission (closures/GC) and why.
Verify: C-emitted fib matches results; benchmark table VM vs C-ified; blocker notes written.
**Lesson check:** what was the hardest AST construct to emit to C — and what does that reveal about why languages have VMs before compilers?

## PL12 — CAPSTONE: your language ships a program
Prereq: PL0–PL11. **Close all notes.** Re-create the bytecode VM cold (the gauntlet discipline — if you can, do lexer→parser→VM for a *tiny* subset in one sitting), then finish: mylang v1 with lexer, parser, type check, VM, GC, stdlib, REPL, golden tests — and **write a real program in it** (your choice: a text game, a calculator suite, an interpreter-of-a-subset — prove your language is a language). Write `labs/pl-capstone.md` like a language designer's post: the map, the three design decisions you're proudest of, your three regrets, and the roadmap (JIT? better GC? ownership?).
**Pass = mylang runs programs you wrote in it, the VM rebuilt cold, the writeup reads like a designer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in PL0/PL10 boilerplate (REPL/harness scaffold) — lexers, parsers, VMs, GCs written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Own machine; honesty about benchmarks (the compiler community lives on fair numbers).
5. Honest bar: real languages are years by teams (Rust/Go/TypeScript); this course's bar = a working pipeline (lexer→parser→typecheck→VM→GC→stdlib→tooling) of YOUR code running programs YOU wrote in YOUR language, proven cold at the capstone — the floor for compiler engineering, and the fastest route to understanding every other language you'll ever read. The repo's own 24-47 (JS interpreter) and 24-09 (C compiler) are your second-half sprint goals.

## Where this lives
`steps/` unchanged (route: 24-47, 24-09, 24-87, 02-17). Pairs the EMULATOR course (a language and a CPU are the same idea — decode + act — once you've built both) and the LLM-ML work (every transformer is a program; now you know what programs are made of).
Backend-borrowed twin — the same frontend skills, LLVM doing optimization+native+JIT: [`LLVM-LANGUAGE-COURSE.md`](LLVM-LANGUAGE-COURSE.md) (LL0–LL12). Together: build the whole pipeline by hand, then build the same language on a real backend.