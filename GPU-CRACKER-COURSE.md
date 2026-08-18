# GPU Password Cracker Course — Absolute-Beginner (hello world → YOUR OWN Metal→CUDA password cracker, gated)

Zero GPU-compute and zero hash knowledge assumed — but you have GPU-BEGINNER (U-units) under your belt or take U0–U4 alongside; and a Mac with Metal + a Linux VM with a CUDA-capable GPU (or a CUDA cloud instance; a small one suffices — even a 2-GB card proves the architecture). The course runs on the PROVEN stack: your own hashes, generated challenge sets, authorized lab targets ONLY (ATMs own vault: hashes you made or were given for testing); nothing else. ~3h/unit, 12 units + capstone ≈ 8-10 weeks. You will write kernels in MSL (Metal Shading Language) and CUDA C, engineer the same cracker twice for two GPU ecosystems, and bench your work honestly.

Compass (re-read when lost): password cracking = hash inversion economics: given a hash digest, find an input — the answer is sorting the search space: dictionary seeds → rule mutations → mask patterns → brute force — and pushing each guess through the hash function faster than cheap hardware can. GPU wins because hashing is embarrassingly parallel: thousands of cores, each testing its own guess. The skill ladder: (1) hash one string correctly (CPU), (2) hash 10⁶ strings in parallel (GPU kernel), (3) choose the right search strategy per target type (mask, dictionary, adaptive rules), (4) scale and measure honestly. Metal first (your machine, your compiler, tight feedback), then CUDA (the industry standard, the NV universe, the same brain).

Honesty clamp (the GPU-PATH rule, all units): benchmark numbers are YOURS: real kernels, real clock, reproducible methodology (same hash set, same N, warmed-up rounds, GPU boost stated); never "advertise" a rate you didn't measure; cracked-material is your own generated lab set; hashcat is the reference tool you may READ for strategy but never COPY kernels from (write them).

Safety: own-hashes-only; authorized targets (your lab, sanctioned CTF/learning servers with published flags); no real-world account recovery without written authorization; cracker is a lab tool, never ships as a service; password recovery of your own lost files is legal and fine — the line is the target, not the tool.

---

## GC0 — hello world: hashes and the economics of cracking
Concept: hash = one-way; cracking = search; GPU = search accelerator. Do: your first hash: implement (CPU, any language) a correct SHA-1/SHA-256 of a string, verify against published test vectors; print hash-to-plaintext for a 3-char brute force (mind the clock); write the economics note: why search space explodes (alphabet^n) and why GPU flattens per-guess cost; read the 10-08 gpu-password-cracker step + hashcat's docs (strategy READING).
Verify: SHA vector-passing + 3-char brute force round; economics note written.
**Lesson check:** why can't you "invert" a hash — and what is the actual computational question you're asking per second?

## GC1 — GPU compute in your head (Metal priming)
Concept: GPU ≠ CPU: SIMT warps, memory hierarchy (global/shared/registers), kernel=thousands of scalar threads; Metal basics. Do: GPU-BEGINNER-floor exercises: a Metal compute kernel (GPU-BEGINNER U3-style) that squares a big array; measure launch overhead + throughput; then a kernel that hashes 1M strings in parallel (SHA-256 kernel v1 — YOUR bytes) and verify a subset's correctness.
Verify: array kernel throughput measured; 1M-hash kernel correct matches vs CPU for sampled outputs.
**Lesson check:** what does "embarrassingly parallel" mean for a hash loop — and where does memory (not ALU) become the bottleneck first?

## GC2 — the mask attack, Metal
Concept: the workhorse: masks (fixed pattern with variable position) — the shape of most real passwords. Do: Metal kernel: candidate generation from a mask (e.g., `?u?l?l?l?d?d`), hash each, compare vs your target digest set (atomics or per-thread early-exit); full pipeline: command buffer → kernel → result collection (found flag + plaintext); brute a mask set you generated with a known answer; measure: hashes/sec, candidates/sec, utilization (%).
Verify: known-answer mask cracked; per-second table recorded; utilization % reported honestly.
**Lesson check:** why do masks beat pure brute force for human passwords — and how does your kernel avoid needless memory traffic per guess?

## GC3 — dictionary + rules: the 90% trick
Concept: humans reuse words: dictionary seeds → mangling rules (leet, append digits, doubled suffixes) explode coverage cheaply. Do: Metal kernel: dictionary load (global memory), rule engine (a compact rule set YOU design: capitalization, substitutions, suffixes), hash each mutation; crack a generated set with a mix of "weak word + suffix" answers; compare coverage: dictionary+rules vs mask vs pure brute on your test set — THE honest comparison table.
Verify: mixed set cracked where plain brute alone would miss; comparison table written (coverage × rate).
**Lesson check:** why does rule-mangling out-crack raw brute force on human inputs — and where does it FAIL (what kind of password defeats it)?

## GC4 — hash families: the resistance ladder
Concept: hashes fight back: MD5/NTLM fast by design; bcrypt/scrypt/argon2 = memory-hard + iterated, built to kill GPUs. Do: implement (CPU, vector-checked) bcrypt OR scrypt (your choice — reading the spec, running official test vectors); TIME it on CPU, then reason + measure the GPU story: bcrypt's circuit (Blowfish state) resists naive parallelism; write the "why these hashes cost 10⁴×" essay with YOUR measured CPU-time numbers.
Verify: bcrypt/scrypt vectors pass; CPU-time table vs MD5/SHA recorded; essay written.
**Lesson check:** what specifically makes a hash GPU-resistant — and why can't you just "add threads" to fix memory-hardness?

## GC5 — CUDA, the port: same brain, NV dialect
Concept: CUDA = industry standard; kernels port structure, not style: grids/host-device/streams vs Metal's buffers/threadgroups. Do: Linux VM + CUDA: vectorize (device-side) your SHA-256 kernel from GC1 — SAME algorithm, CUDA C; the compile→profile loop (nvcc, nvprof/Nsight): measure occupancy, occupancy-limited vs memory-bound; verify correctness vs your CPU reference.
Verify: CUDA SHA-256 kernel matches CPU; occupancy measurements with the WHY.
**Lesson check:** what ports cleanly Metal→CUDA and what doesn't — and which metric do you optimize FIRST (occupancy? memory?) for a hash kernel?

## GC6 — the CUDA cracker core: candidate + compare, NV speed
Concept: the full cracker on CUDA: mask + dictionary+rule engines from GC2/GC3 ported, batched streams (overlap kernel + memcpy), pinned memory, the compare path. Do: CUDA mask kernel + CUDA dict+rule kernel; double-buffered streams; crack your GC3-compatible test set ON THE NV CARD; the port-table: same algorithms, two ecosystems, performance deltas explained (clock, memory, compiler).
Verify: CUDA cracker cracks your sets; stream overlap measured (+X%); port-table written.
**Lesson check:** where did your CUDA build beat Metal and lose — and what did BOTH tell you about the SAME bottleneck?

## GC7 — the memory-hard frontier (bcrypt/scrypt on GPU: engineering, not magic)
Concept: memory-hard on GPU = workable via batching + shared/registers, with hard memory ceilings — the honest engineering. Do: design (and implement as far as the card allows) a batched bcrypt/scrypt CUDA kernel: their memory phases scheduled across the block; measure the real ceiling (threads that fit vs memory per thread); the verdict essay: why GPUs still lose to ASICs/CPUs here, where hashcat-style tools put the effort anyway.
Verify: kernel design doc + ceiling measurements + verdict essay (honest limits).
**Lesson check:** what is the fundamental resource fight in memory-hard hashing on GPU — and what would you buy (HW) to win it?

## GC8 — speed, honestly: the bench discipline
Concept: numbers mean nothing without methodology: clock control, input normalization, per-candidate math, warm-up, boost state. Do: write the bench harness (same hash set, same N, warmed-up, N runs: min/median/max, GPU state recorded); bench every kernel you've built (Metal list + CUDA list); the "hashcat baseline" — run hashcat on the SAME sets/cards for reference (reading its numbers against yours); write your Bench Report in the GPU-PATH format (methodology + tables + what each delta means).
Verify: full bench report reproducible end-to-end; hashcat comparison included + explained.
**Lesson check:** what three lies do you filter in GPU benchmarking — and why is a methodologically-sound slower number more useful than a fast honest-looking one?

## GC9 — scaling: multi-stream, multi-GPU, the ceiling
Concept: one kernel → pipeline: streams overlap; multiple GPUs split the search space; the real ceiling = memory bus + PCIe, not cores. Do: CUDA: multi-stream version (2-4 overlapping pipelines) with the measured gain (expect less than linear — document why); split search space across TWO GPUs (or one NV + your Mac Metal card as a distributed pair — your own IPC shim if you dare); the scaling table.
Verify: streams + multi-GPU measured deltas; scaling table written with bottleneck analysis.
**Lesson check:** why is scaling sub-linear here — and what does PCIe/memory bandwidth cap that cores alone can't fix?

## GC10 — the toolbox: phishing-resistant practice = authorized only
Concept: the tool's market is authorized recovery and defense: pentest scope, DFIR password recovery, password-policy auditing — and its abuse is credential theft. Do: the usage playbook (written): authorized-target checklist (scope doc, written consent, lab flags, engagement rules), the same checks automated in your CLI (a `--require-scope` flag refusing targets without a markers file); a real authorized mini-drive: your lab VM's own password policy audit (crack generated policy-test hashes, report weakness pattern); the 300-word target-ethics essay.
Verify: playbook + scope-gated CLI + policy-audit run + essay all done.
**Lesson check:** what single file or fact makes a run authorized — and what does your autogate do when it's absent?

## GC11 — the reference wall: read hashcat's brain
Concept: hashcat is the summit (CPU/GPU, thousands of modes, rule engine, kernels); reading its design = the graduate course. Do: deep-read hashcat's docs/architecture (kernel source READING allowed now — no copying, it's the reference); map ITS strategy choices vs YOURS across: kernel engineering, rule engine, mode coverage, mask charset engine, performance techniques; write the design-review: three things hashcat does you'd steal (with reasons), three you wouldn't.
Verify: design-review paper (mapped to your kernels) + three-steal/three-not list argued.
**Lesson check:** what did the summit do that your course kernels skipped — and what ratio is "engineering excellence" vs "clever crypto"? (answer with examples)

## GC12 — CAPSTONE: gpcrack v1, cold
Prereq: GC0–GC11. **Close all notes.** Cold: rebuild the cracker core (SHA-256 kernel + mask + dict/rule + compare + bench harness) in ONE sitting on the platform of your choosing (Metal or CUDA — your call, your justification), then run a live authorized drive: a generated 4-password set you've never pre-seen, cracked in-session with the bench report written. Write `labs/gpu-crack-capstone.md`: architecture diagram (host↔device flow), the bench methodology + tables, the ethics page (target-gating, authorized-own), three proud decisions, regret, roadmap (multi-hash format support, distributed cluster, ASIC-when-to-give-up).
**Pass = gpcrack v1 cracks the live set cold with an honest bench report and the scope-gate armed.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in GC0/GC5 boilerplate (kernel scaffold, bench harness) — hashes, kernels, engines, rules, tests written by you; erase-and-retry once when stuck; hashcat = read-only reference.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Target-line: own hashes/authorized labs only, scope-gate armed, no real-world recovery without written authorization, cracker never ships as a service.
5. Honest bar: hashcat is decades of engineering — this course's bar is YOUR OWN two-ecosystem cracker (Metal + CUDA), real kernels, honest bench methodology, scope discipline, proven cold at the capstone — the floor for GPU-accelerated security tooling and the complete brain behind every "is this password weak" question.

## Where this lives
Your GPU-BEGINNER compute floor (U-units) lifts in; GPU-PATH S-units carry deeper kernels; 10-GPU 08-gpu-password-cracker step + 21-09 own-YARA engine (hash-first thinking) feed the detection side; the N/windows-lane password material (AUTH-TOOLING H-hashes, NTLM) ports straight into your kernel list.