# Fuzzing Course — Absolute-Beginner (hello world → YOUR OWN coverage-guided fuzzer that finds real bugs in YOUR code, gated)

Zero fuzzing knowledge assumed — you bring a parser you wrote (JSON/PNG/text-loader from the Make-Your-Own steps — your FIELDS OF PLAY), C-family compile skills (clang/gcc), and CVE-vision from CVE-STUDY (why fuzzing matters: most modern vulns were fuzz-found). The course rebuilds the coverage-guided fuzzer (libFuzzer/AFL-class) from zero: harness → coverage collection → mutation engine → the feedback loop → crash triage + minimization — and then proves it: YOUR fuzzer finds a bug in YOUR OWN code (a planted one if you must, a real one if you can). Step 05-12 (own-coverage-fuzzer) is your charted coastline; this course is the whole voyage with the tooling depth. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/fzN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): coverage-guided fuzzing = a search: throw inputs at a program, watch WHICH code paths light up (coverage), and mutate the inputs that lit NEW paths harder. The loop: input → execute → coverage → keep-if-new → mutate → repeat. The skill is in the machinery: a tiny fast harness, a sensitive coverage bitmap, a mutant that explores (bit flips + structure), and triage that turns crashes into reports. You'll build each piece with your own bytes and measure the loop's value the way the industry does: bugs found per CPU-hour.

Safety/scope (unit FZ0): you fuzz YOUR OWN programs and sanctioned lab targets ONLY; fuzzing a third party's software (even "harmlessly") is out unless explicitly authorized; the course's bug-hunt runs against your OWN parsers; crash artifacts stay in your lab.

---

## FZ0 — hello world: the loop, seen whole
Concept: the CGF loop diagram (input → execute → coverage → keep-if-new → mutate); why coverage beats random (path discovery = more program reached = more bugs observed). Do: run a REFERENCE fuzzer once (libFuzzer or AFL, documented install) against a toy parser (yours) — observe its loop in logs (input count, coverage growth, crashes); write the "what the reference did that I'll rebuild" note (architecture, not code); your first harness: a trivial parser (one function that parses YOUR buffer format) compiled with a sanitizer.
Verify: reference run observed + note written; your harness compiles with ASan and runs one input.
**Lesson check:** what exactly does COVERAGE add over random bytes (what's the mechanism of discovery) — and why is the HARNESS the fuzzer's real sink (what's best to fuzz)?

## FZ1 — coverage collection: seeing the light
Concept: coverage = which edges executed: compiler instrumentation (SanitizerCoverage / gcov) or your own collector; edge vs line coverage. Do: instrument your harness with clang's `-fsanitize-coverage=trace-pc-guard` (reading the callback contract), your own coverage bitmap (a shared-memory/global array the callbacks poke — YOUR wiring), the "edge ID" logic (hash of caller+callee — why edges beat basic blocks); demo: two inputs, different paths, your bitmap differs.
Verify: bitmap differs across two crafted inputs (direct proof); edge-ID scheme documented.
**Lesson check:** why EDGES (not lines) — and what does a COARSE bitmap cost (what happens when two paths collide on one bit)?

## FZ2 — the mutator: the explorer's dice
Concept: mutation: bit flip, byte splice, insert/delete, dictionary tokens, arithmetic; deterministic first (each seed explored in order) then havoc (random). Do: your mutator module (the operation set + a deterministic pass + a havoc pass), seed corpus (your own valid inputs), a dictionary (magic bytes, keywords — YOUR list for your parser); the mutation test: given one seed, generate N variants, verify they mostly parse (validity rate %) vs are diverse (bytes differ) — the tradeoff table.
Verify: mutation stats table honest (validity vs diversity vs count); operations all implemented.
**Lesson check:** why does mutation need BOTH structure-touching and byte-touching ops — and what does VALIDITY-rate predict about your fuzzer's reach (too valid = no new edge; too invalid = how deep does it get)?

## FZ3 — the loop: my fuzzer, first blood
Concept: your main loop: spawn/call harness per input, coverage compare, new-coverage → queue, feedback (energy per queue item), crash collection. Do: the loop (in-process or subprocess — your choice, documented tradeoff), queue management (rounds, energy, max-items per input — AFL-class discipline), coverage-replay check (an input re-run yields the same bitmap — the sanity test); run: your fuzzer vs your harness for 10 minutes → coverage growth curve plotted.
Verify: curve grows then plateaus (the classic shape); reproducibility check green; crashes (if any) collected with reproducer saved.
**Lesson check:** what makes the loop FAST (what's the per-iteration tax — and what would you cut to double exec/s) — and why does the CURVE plateau (what does plateau mean: done or stuck)?

## FZ4 — triage: from crash to report
Concept: crashes need dedup + minimization + report: stack-based dedup, delta-debugging minimization (implement your own — the divide-and-conquer shrink), ASan report capture. Do: your triage module: crash signature (return address stack) → dedup key, `minimize` (your delta-debug: shrink input while crash persists), report file (input, cmdline, signature, minimized input); a planted crash (a bug YOU put in your harness — e.g., missing bounds check) found, deduped, minimized.
Verify: planted crash → triage pipeline (dedup key correct, minimized input tiny but still crashes, report complete).
**Lesson check:** why MINIMIZE (what does a researcher actually file — the 12-byte repro, not the 4KB original) — and what makes a dedup key honest (what does stack-dedup miss)?

## FZ5 — structure-aware: the leap from dumb to smart
Concept: dumb byte mutation dies on format validation; grammar/token-aware mutation walks INTO the structure. Do: your structure model for YOUR parser (tokenization pass + mutation at token level: length fields, counts, nested elements), custom mutators (your own ops: duplicate-element, swap-fields, boundary-values), interleave with dumb pass; the comparison run: structure-aware vs dumb on the SAME target/hours — coverage + bugs (expect structure wins on validation-heavy parsers — measure it).
Verify: comparison table (coverage/bugs per mode) honest; structure pass demonstrably reaches deeper.
**Lesson check:** why do length-fields and counts kill dumb fuzzing (what invariant breaks) — and what is the TRADEOFF when you invest structure (what do you lose, and when is dumb still right)?

## FZ6 — dictionaries, seeds, and the corpus mind
Concept: good seeds + good dictionaries = head start: tokens (magics, operators, keywords), corpus from real usage (your own files), corpus minimization (keep only coverage-adding files). Do: build the corpus mind: seed collection from your own tests, dictionary curation, and CORPUS MINIMIZATION (your implementation: greedy coverage-preserving subset); the A/B: fuzz with bare vs seeds+dictionary — the curve comparison (how much wall-clock the head start buys).
Verify: A/B curves recorded (head-start quantified); corpus minimizer keeps coverage, cuts files (numbers).
**Lesson check:** what does a SEED give a fuzzer that mutation alone can't (the grammar of reality!) — and why minimize the corpus (what does redundancy cost per iteration)?

## FZ7 — sanitizers: the bug detectors on the hook
Concept: sanitizers turn "silent wrongness" into crashes: ASan (memory), UBSan (undefined), TSan (races); fuzzing without them misses most bugs. Do: your harness compiled under ASan+UBSan, fuzz again on the same target — the before/after bug count (planted subtle bug that ONLY shows under sanitizer — e.g., signed overflow or OOB-by-one); the sanitizer matrix note (what each catches, speed cost, false-positive kinds — documented honestly).
Verify: before/after table (sanitizer-only bug found); matrix note written.
**Lesson check:** why are MOST fuzzer-found CVEs sanitizer-dependent (what does "silent wrongness" do without one) — and what does ASan's speed cost buy (when do you fuzz without it)?

## FZ8 — the real hunt: find and fix your own
Concept: the whole point: a bug in code YOU wrote, found by YOUR fuzzer, fixed, regression-tested. Do: take a real parser you built earlier (Make-Your-Own), write its harness, run your full stack (coverage + structure + sanitizers), find a REAL bug (if your code is clean, fix that by planting? NO — hunt honestly first, plant only if truly clean, and disclose the plant); produce the full artifact set: reproducer, minimized input, report, FIX, regression test added to your harness suite. 
Verify: real artifact set complete; regression test fails before fix / passes after.
**Lesson check:** what did finding a bug in your OWN code teach that planted crashes can't (the honesty of the hunt) — and why is the REGRESSION TEST the fuzzer's gift that keeps giving?

## FZ9 — scale-lite: parallel and the honest ceiling
Concept: fuzzers parallelize: workers share a corpus; diminishing returns are real; benchmark honesty (exec/s, coverage/hour). Do: multi-worker support (N processes, shared corpus dir), the scaling curve (1, 2, 4 workers — coverage vs wall-clock — the sub-linear truth), benchmarking discipline (same target, same seeds, warm-up), the honesty section: what your fuzzer will NEVER do (what libFuzzer's decades of tuning add).
Verify: scaling curve plotted + bottleneck analysis; honesty section written.
**Lesson check:** why is scaling sub-linear (what's the contention — coverage map? CPU? target?) — and what does "diminishing returns" tell you about when to STOP fuzzing?

## FZ10 — the pros: libFuzzer, AFL, and OSS-Fuzz, read
Concept: the reference implementations: AFL's forkserver design, libFuzzer's in-process speed, OSS-Fuzz's CI fuzzing — and the case studies. Do: reading unit: AFL architecture (forkserver, shm coverage — map to yours), libFuzzer's speed tradeoffs, OSS-Fuzz's model (fuzzing as infrastructure); case studies: pick 2 fuzz-found CVEs from CVE-STUDY's tables, read the bug + fix, map which technique found them; the design-review: three ideas to adopt, three to reject (argued, with reasons).
Verify: reading notes + case-study maps + adopt/reject table complete.
**Lesson check:** what is the ONE design idea that separates pro fuzzers from yours (argue: speed? coverage fidelity? structure?) — and why did OSS-Fuzz change the industry (what does fuzz-in-CI mean for bug timelines)?

## FZ11 — the professional shape: fuzzing as your floor
Concept: a researcher's day: harness hygiene, continuous fuzzing, triage queues, regression suites — the discipline wrapping the tool. Do: build the wrap: your fuzzing README (harness list, how to fuzz, how to report), a CI-lite script (fuzz 5 min on new commits — run it), a crash-queue review run (from your earlier sessions: dedup + triage all collected crashes into a table); the 300-word essay: "why I trust my fuzzer's negatives (and don't)" — the coverage-blind-spot honesty.
Verify: wrap artifacts done; crash queue table complete; essay written.
**Lesson check:** what does CONTINUOUS fuzzing change vs one-off (who sees the bug first) — and why must you distrust "no crashes" (what does coverage NOT cover)?

## FZ12 — CAPSTONE: the fresh hunt, cold
Prereq: FZ0–FZ11. **Close all notes.** Cold: rebuild the CGF core (coverage collector + mutator + loop + triage) in one sitting (no notes) against a FRESH parser (unseen, with a REAL or planted-but-disclosed bug), find it, minimize it, report it, fix it, add the regression test. Write `labs/fuzz-capstone.md`: architecture, loop design, curve, the artifact set (input, minimized, report, fix), the FZ11 essay, three proud decisions, regret, roadmap (structure-aware deep, grammar fuzzing, fuzz-in-CI).
**Pass = the cold rebuild finds the fresh bug end-to-end; the artifact set and essay carry the file.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in FZ0/FZ1 boilerplate (reference-tool run, sanitizer flags) — collector wiring, mutator, loop, triage, minimizer written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Scope: your own programs + sanctioned lab targets only; no third-party fuzzing without explicit authorization; crash artifacts stay in the lab.**
5. Honest bar: libFuzzer/AFL/OSS-Fuzz are decade-tuned systems backed by volunteers and companies; this course's bar = a working coverage-guided fuzzer that finds, minimizes, and fixes a real bug in your own code, proven cold at the capstone — the floor for vulnerability-discovery engineering and the complete answer to "is my code actually robust".

## Where this lives
The discovery engine of the vuln-research family: 05-12 (own-coverage-fuzzer) is the mapped step; KERNEL-EXPLOIT's feeds start here (fuzz → bug → exploit-worthiness); BROWSER-EXPLOIT's fuzzing unit is the browser-scale version; CVE-STUDY's cases show the payoff — one loop, three altitudes, your own hands.