# VM-Obfuscation Course — Absolute-Beginner (hello world → YOUR OWN VM-based code obfuscator/tamper-protector + the deobfuscator that defeats it, gated)

Zero VM-obfuscation knowledge assumed — but you have the PL-Course VM (your own bytecode interpreter) or INTERPRETER-course under your belt; plus INSTRUMENT-COURSE DI0–DI12 (your tracer will become the deobfuscator) and 15-06 symbolic-execution awareness as strength. The course builds the VMProtect-class pipeline: lift a function into your own virtual machine (static analysis sees a loop + garbage, not your logic), armor it (encoding, opaque dispatch, junk), package it (protected blob + loader), then — the honest twin — build the DE-OBFUSCATOR (trace + dump + recompile) that recovers your own function. **The word "evasion" is not in this course's contract: the pipeline protects YOUR OWN software and trains reverse engineering; evading antivirus/EDR detection is a different (refused) target.** Each unit: concept → do → runnable verification → **lesson check (own words, `notes/voN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 9-11 weeks.

Compass (re-read when lost): an obfuscation VM replaces real machine code with YOUR instruction set: the target program becomes a tiny interpreter-loop running encrypted bytecode — static disassembly shows the VM, not the logic. The craft has three gears: (1) the VM itself (the interpreter from PL-course), (2) the LIFT (compiling your own function down to bytecode — a mini-compiler in reverse), (3) the ARMOR (making the bytecode and dispatch resist reading). And because this entire art is an arms race, the course ends where it must: with the tools that UNDO it — tracing, dumping, and rebuilding the plain logic. You cannot honestly own the offense without owning the recovery.

Core honesty clause (unit VO0's constitution): obfuscation is not security (a determined analyst always wins eventually — that's a theorem, not a mood); the product of this course protects your own software and becomes a RE study target; the "evade the AV" framing is explicitly refused; every unit ships with its detection counterpart.

---

## VO0 — hello world: what a protection VM is
Concept: VM-based protection: lift code, run it in an interpreter loop; what static analysis sees vs what's really there. Do: read one VMProtect-class paper (analysis writeups count); write the "what gets hidden / what remains visible" table (executable headers, imports, the VM loop itself stay; your function's logic disappears); run a hello: your PL-course VM interpreting a tiny program while `objdump`-style listing of the host binary shows only the interpreter (prove with your own eyes on your own build).
Verify: table written; your VM-binary's static listing shows no program logic.
**Lesson check:** what exactly remains visible after a VM-lift (what CAN'T be hidden) — and why does that matter for what this course refuses to promise?

## VO1 — the lift: your function becomes bytecode
Concept: lift = a mini-compiler: take a function (yours), translate to YOUR bytecode: constants, arithmetic, branches, calls. Do: hand-lift one real C function (a small state machine with a loop + branches, yours) into PL-VM bytecode, then generalize with a generator script (AST-walker-ish from your PL compiler) for arithmetic/branch/call patterns; bytecode runs and matches the C function's results on a test suite.
Verify: bytecode matches C results across a test suite (diff-clean); lift-generator documented.
**Lesson check:** what linguistic features make a function EASY vs HARD to lift (pointers? calls? float?) — and where does your lite version stop (honest list)?

## VO2 — the reshape: flattening and encoding
Concept: armor by structure: control-flow flattening (dispatch state machine), opaque predicates (always-true junk conditions), constant encoding (XOR/blocks) — each transforms the bytecode shape. Do: implement on YOUR lifted bytecode: flatten (translator from CFG to dispatcher-state machine — YAML/JSON IR), encode constants (your chosen scheme + decoder prologue), inject opaque predicates + junk blocks (dead code); verify: bytecode still runs to identical results; measure the size/delta growth.
Verify: re-shaped bytecode passes the same test suite; growth table written.
**Lesson check:** what does flattening do to a HUMAN reader that it doesn't do to a machine — and why are opaque predicates a "speed bump" and not a wall?

## VO3 — the packer: blob + loader
Concept: ship it: encrypted bytecode blob + native loader stub (decrypt + VM-entry). Do: your blob format (your magic, header, encrypted regions, key — lab-key discipline); loader: native stub decrypts + jumps into interpreter loop; self-check: lightweight checksum over own blob (integrity lite); a test: your protected program runs from the stub and behaves identically.
Verify: protected run identical on the test suite; loader/self-check behaviors demonstrated; blob format documented.
**Lesson check:** what does encryption add that flattening couldn't (and what does the KEY problem become — where must the key live)?

## VO4 — static resistance: what the disassembler sees
Concept: measure the hiding: static analysis of your protected build. Do: run your own protected binary through a static listing (objdump + your own parser from 15-08); inventory: what's plainly readable (headers, imports, VM loop), what's hidden (your logic), what leaks (strings, patterns, entropy spikes); write the "static forensics of my build" report.
Verify: report produced; three leaks you found and fixed (strings, patterns) with before/after proof.
**Lesson check:** why do strings and imported-API patterns leak more than code — and what does an analyst actually do FIRST on a suspected-VM binary (hint: not disassembly)?

## VO5 — deobfuscation half 1: trace and dump
Concept: the blind spot of static-resistance: execution — trace the VM loop and the plaintext logic walks out. Do: use YOUR OWN INSTRUMENT tool / tracer: instrument your protected binary at the interpreter loop, log executed bytecode + operand values; dump the de-encrypted bytecode stream mid-run; the "trace-to-recovery" principle written as your own method note.
Verify: tracer recovers the full executed instruction sequence + operand values for your test function.
**Lesson check:** what does tracing replace that static analysis can't give (values! concrete data) — and why is a SINGLE traced run often enough for small programs?

## VO6 — deobfuscation half 2: rebuild the function
Concept: lift back up: executed bytecode → pseudocode: peephole constant folding, opaque-predicate elimination (run them: they're constant!), dispatch-state collapsing → a C-shaped function. Do: build your auto-deobfuscator: bytecode→IR (your PL IR), constant-fold + predicate-eliminate, CFG rebuild via dispatch trace, emit pseudo-C; run it against YOUR OWN protected function; diff the recovered logic vs the original (structure + values — honest match report, including what you couldn't recover).
Verify: recovery output matches original behavior/structure (diff-grade report, honest misses).
**Lesson check:** which armor survives your recovery and which dies immediately (why are opaque predicates the easiest kill) — and what does that prove about obfuscation's ceiling?

## VO7 — armoring back (the arms race, kept honest)
Concept: defenders of obfuscation get smarter too: encoded immediates per-instance, opaque dispatch tables, split VMs — the never-ending loop. Do: implement TWO counter-measures to your own deobfuscator (per-instance key derivation; dispatcher indirection table); re-run VO6 — recovery takes longer / degrades (measure); the essay: "why this loop never ends and why obfuscation is defense-in-depth, not a lock."
Verify: both armorings implemented + your recovery degrades measurably; essay written.
**Lesson check:** what did the second round of armor cost (size? speed? complexity?) — and what does the perpetual loop say about the RIGHT place to put real security?

## VO8 — the real world, read-only: VMProtect/Themida and the malware-shaped cousins
Concept: commercial protectors and the malware that borrows their tricks; what defenders actually do about both. Do: reading-only: VMProtect/Themida analysis papers (mechanism mapping to YOUR units), anti-virus-industry writeups on VM-packed malware (Trickbot-class) and the AV-side techniques (emulation, behavioral); the "why protection vendors obey the arms race" note; NO code from them, mechanism-only mapping.
Verify: mapping table (their feature → your unit) + AV-side techniques summary written.
**Lesson check:** what separates commercial protection from malware-use legally and technically (target, purpose, disclosure) — and what AV technique defeats both (hint: it doesn't disassemble)?

## VO9 — the detection twin: flag your own build
Concept: the blue side: detect VM-protected binaries: entropy, section/header anomalies, import patterns, behavior. Do: from your YARA-engine step: write YARA rules + a heuristic scorerner that flags YOUR OWN protected binary (entropy spikes, flat imports, VM-loop hallmark); the honest metric: false-positive rate on a clean corpus (your normal builds); the round-trip proof: your detector flags your protector.
Verify: detector flags your protected builds; FP rate measured on clean corpus; rules written in your words.
**Lesson check:** what makes a detector's job easier here (what did the packer LEAVE that clean code doesn't have) — and why FP-rate discipline matters more than detection rate?

## VO10 — the truth page: obfuscation ≠ security
Concept: the professional's creed: obfuscation slows analysis; it authenticates nothing, resists nothing determined, and its legitimate use protects confidentiality of YOUR code while its abuse hides malware. Do: the 400-word essay (your position): what this course's craft is FOR (software IP protection, RE study, CTF design), the line (protecting your software vs evading defenders), the guardrails (never ship evasion, always disclose, lab-key discipline); sign it.
Verify: essay + guardrails written; you can defend both sides in notes.
**Lesson check:** what single goal, if attached to your pipeline, would turn it from protector into weapon — and what does your constitution say about that switch?

## VO11 — the product: vmp-lite CLI + the challenge
Concept: useable tooling: one CLI for the pipeline + the peer challenge (the ultimate test: someone else's eyes). Do: `vmp-lite`: `protect <bin>` (lift+armor+blob), `status <bin>` (static forensics report), `deob <bin>` (trace+rebuild); then THE CHALLENGE: exchange protected binaries with a peer (or future-you after a month) — you deobfuscate theirs, they deobfuscate yours; the challenge report (time-to-recovery, what helped/hindered).
Verify: CLI works end-to-end on your test suite; challenge exchange completed with reports both ways.
**Lesson check:** what did an OUTSIDE deobfuscation of YOUR build teach you that self-testing couldn't (where were your blind spots)?

## VO12 — CAPSTONE: the full circle, cold
Prereq: VO0–VO11. **Close all notes.** Cold: lift ONE function (fresh, unseen) → armor → protect → run; then deobfuscate it back with your tooling; final static+dynamic report; the detection rules re-run against your fresh build; the truth essay as cover letter. Write `labs/vmp-capstone.md`: pipeline architecture, the recovery report (what matched, what didn't), the detection FP table, three proud decisions, regret, roadmap (multi-VM, x86 backend, symbolic-execution recovery).
**Pass = protect→deobfuscate→detect all work cold on the fresh function; the truth page carries the file.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in VO0/VO3 boilerplate (VM scaffold from PL-course, blob loader skeleton) — lifts, armors, deobfuscator, detector written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Line-hardline: protects YOUR software; no AV/EDR evasion targeting ever; deobfuscation twin required per unit; lab keys; commercial/malware code = reading only.**
5. Honest bar: VMProtect/Themida are commercial products and so is their deobfuscation research (an entire academic+industry field); this course's bar = a working protect→recover→detect cycle on your own code, arms-race understood, truth page signed, proven cold — the floor for software-protection engineering and the exact anatomy of every "why can't I read this binary" question.

## Where this lives
Your PL/interpreter VM is the engine; INSTRUMENT-COURSE's tracer is the deobfuscator's eye; 15-06 symbolic execution is the future roadmap; 24-10 own-packer is the simpler cousin; 21-09 YARA supplies the detection twin — and every step of this course doubles as a RE training ground, which is the point.