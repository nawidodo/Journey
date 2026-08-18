# Dynamic Instrumentation Course — Absolute-Beginner (hello world → YOUR OWN Frida-class instrumentation framework, gated)

Zero DI knowledge assumed — you need your Linux VM (primary playground), a Windows VM + your Mac (mapped lanes), and the RE-floors from 15-07 (mini-debugger) / 15-08 (mini-disassembler) as companions (reading them enough to thread your hooks). The course rebuilds the Frida stack from the ground: process injection → a C agent → your own inline hook engine → a script layer with onEnter/onLeave callbacks → memory exploration → a usable CLI — targeting ONLY your own test binaries, course CTF binaries, and sanctioned labs. Frida is the reference ARCHITECTURE you read, never code you copy. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/diN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 9-11 weeks.

Compass (re-read when lost): instrumentation = making a running program REVEAL its internals: your code attaches to a process, plants code inside it (injection), reroutes function calls to yourself (hooking), and lets a script decide what to do at each entry/exit (the scripting layer). Frida's genius is separating those four concerns cleanly — a tiny native agent + a JS runtime speaking to a host CLI. You'll rebuild that same separation your own way: less JS, more your own protocol, same superpower: "what was the argument to open() at 14:03:22" and "what does this binary do with strings starting with X" become minutes of work.

Scope discipline (the course's standing rule): instrument only programs you own, course-provided CTF/learning binaries, and your dev builds. Instrumentation is also used by malware (API tracing, evasion detection) — you learn the CRAFT here, and the ethics unit (DI10) owns that line explicitly. Anti-instrumentation (tamper resistance) is studied as an adversary-facing design question, not built.

---

## DI0 — hello world: attach, inject, hook, script
Concept: DI in one loop: find process → inject code → hook a call → act on it; Frida's four-layer anatomy (agent/runtime/host/script). Do: read Frida's architecture overview (docs); `/proc` walks: list your VM's processes + maps of one of YOUR binaries; ptrace: attach/detach your own process cleanly (PTRACE_ATTACH + waitpid + DETACH), read /proc/self/status TracerPid before/after; write the four-layer diagram for what you'll build.
Verify: process/map listing works; ptrace attach/detach proven with TracerPid evidence.
**Lesson check:** what four distinct problems does Frida separate — and which of them is REALLY the hard one (injection? hooking? scripting?) — argue from what you saw today.

## DI1 — injection, Linux: your own code into another process
Concept: injection = planting a payload: ptrace + /proc/pid/mem write (classic Linux), or a shared-library loader trick; you'll do the honest classic. Do: own a small target program (yours); write the injector: ptrace attach → save regs → map/place a shellcode or dlopen stub → set RIP → continue → restore; payload prints "injected" into the target's stdout/fd; the full writeup of the technique + why it's deprecated-ish in real tools (that reading counts).
Verify: your target prints the injected message from a separate injector process; regs restored cleanly (target keeps running normally after).
**Lesson check:** what does ptrace give you that lets you hijack execution — and why do production tools prefer library-loading over raw shellcode (maintenance, portability)?

## DI2 — the agent: a C payload with a contract
Concept: agent = code living INSIDE the target: handshake, command loop, shared ABI. Do: write the agent (C shared object): load via your DI1 injector (or LD_PRELOAD for dev); agent speaks YOUR protocol over a pipe/socket: commands you define (READ_MEM, WRITE_MEM, CALL, GET_REGS, HOOK_ADD); host-side client (Python) issuing commands; target's agent responds; a functional demo: host reads a chosen buffer from the target, writes a byte back, calls a target function.
Verify: full host↔agent round-trip works (read/write/call proven with evidence from your target).
**Lesson check:** what does the agent contract have to get right once and forever — and why is a SOCKET better than a file for agent transport (what breaks on a file)?

## DI3 — hooking: OWN inline trampolines
Concept: hook = redirect a function to your code: PLT/GOT interposition (cheap, easy) vs inline hooks (trampolines, powerful, risky): 5-byte jmp on x86-64. Do: first LD_PRELOAD interposition (hook your own libc call in your binary — trivially easy path, still educational); then the real thing: write the inline hook engine: read bytes at target → save original + build 5-byte jmp → your handler (saves regs, calls original via your saved trampoline) → restore on uninstall; test on YOUR binary's own functions; crash discipline: snapshots + a known-bad test case you document.
Verify: both hook styles proven on your binary (log lines with args); inline engine uninstalls cleanly (binary still works); one documented crash resolved.
**Lesson check:** what makes inline hooking fragile (length disassembly, PIE, concurrency) — and why does instrumentation STILL choose it over LD_PRELOAD for real work?

## DI4 — the script layer: your own onEnter/onLeave
Concept: scripting = policy above mechanics: hooks attach to callback protocol, not hardcoded logic. Do: design YOUR script format: a tiny JSON/expression command ("onEnter: log arg0; onLeave: log ret") OR a micro-script interpreter (tokenizer+exec of 5 statement types — impressive, still tractable); the glue: hook events → your script engine → host output; implement Interceptor.attach-style: your own `attach(addr, {onEnter, onLeave})` with register save/restore + arg reads (SysV AMD64 calling convention: rdi, rsi...); trace `strcmp`/`open` in your test binary with scripts.
Verify: script-driven trace works (onEnter+onLeave logs with args + ret); your attach API used in a demo script.
**Lesson check:** why does Frida put a SCRIPTING language (not just C callbacks) above its hooks — what does a policy layer buy over hardcoded logic?

## DI5 — the interpreter inside: registers, ABI, and the call-arguments problem
Concept: intercepting means speaking the ABI: save ALL caller regs, read convention args, capture return, restore exactly. Do: the deep unit: your hook handler's full register save/restore (assembly), argument extraction per AMD64 ABI (by integer/float class), return-value capture on leave; the floating-point + vector-arg cases (SSE) handled or honestly documented; a torture test: hook a function with 6+ mixed args (int, float, string, struct ptr) in your binary — all logged correctly.
Verify: mixed-arg torture test passes; register save/restore proven (function still returns correct values).
**Lesson check:** what breaks when a hook clobbers one callee-saved register — and why do float-vs-int arg classes need different capture paths?

## DI6 — memory exploration: maps, scans, dumps, backtraces
Concept: the analyst's eyes: parse /proc maps (your own parser), scan for patterns, read/write memory, enumerate modules + exports via YOUR ELF parser, walk backtraces. Do: your own /proc-maps parser + a memory scanner (pattern search over readable regions, slow-but-YOURS); your own ELF64 parser (sections, symbols — reuse synth from 15-08) exposing module+export lists; a backtrace lite (frame-pointer walk on your test binary, or libunwind READING — pick, justify); demo: find a known string in the target's heap, dump it, backtrace the function that touched it.
Verify: scanner finds the planted string; dumps match file contents; backtrace names appear in your report.
**Lesson check:** which memory artifacts make scanning fast (regions, permissions, alignment) — and why is a BACKTRACE the highest-value single artifact when you hit a breakpoint?

## DI7 — self-defense awareness (theirs, not yours)
Concept: targets fight back: ptrace checks (TracerPid), /proc/self/maps inspection, timing, integrity; instrumentation must read those signals to know when IT is being detected. Do: implement a detector module (in YOUR tool) that checks a target for anti-debug (TracerPid, breakpoint-byte scan, timing deltas) — the honest direction: YOUR tool detecting the target's defenses, not building new evasion; write the "why anti-instrumentation exists (DRM, cheat-proofing) and its cost" essay.
Verify: detector reports a target that checks TracerPid; essay written (with a real product example).
**Lesson check:** what does a target's anti-instrumentation reveal about ITS threat model — and why is "detect the detector" the correct engineering direction for an analysis tool?

## DI8 — Windows lane: DLL injection and MinHook-style hooks
Concept: Windows injection: CreateRemoteThread/APC-techniques (mapped), your DLL agent; inline x64 hooks (same trampoline math, different calling env). Do: Windows VM: your agent as a DLL; injector using CreateRemoteThread + LoadLibrary (the documented classic; VirtualAllocEx + WriteProcessMemory + CreateRemoteThread); write YOUR x64 inline hook (MinHook-style reading for the technique, your own bytes) hooking a test-program function; both verified in the VM; the modern-alternatives reading (NtCreateThreadEx, hollower-class) mapped.
Verify: Windows injector + DLL agent run (your test DLL logs); x64 inline hook proves arg capture.
**Lesson check:** what does Windows change about injection vs Linux (APIs, handle model, thread) — and why is CreateRemoteThread the "old but teachable" path?

## DI9 — macOS lane: reading the guarded house
Concept: macOS restricts: task_for_pid requires privileges, DYLD interpose is the sanctioned hook point, codesigning gates everything. Do: reading unit: DYLD interpose (the legit hooking route — map it to your DI3), task_for_pid + its privilege reality, codesigner/entitlement walls for injection; your notebook: "if I had to instrument on macOS, I would use X — and here's why"; a tiny interpose demo (if your Mac allows unsigned dev builds) OR a documented skip with the exact wall quoted.
Verify: interpose reading mapped to your architecture; either demo or wall-definition written.
**Lesson check:** what does macOS's hostility to injection tell you about ITS trust model — and which of YOUR layers does DYLD interpose replace (and not)?

## DI10 — real Frida, read at depth + the ethics standing
Concept: Frida = gum (core) + runtime (JS) + agent + host; instrumentation's dual nature. Do: deep-read Frida's architecture (gum's backends, interceptor implementation, script runtime choices); map THREE of its engineering calls vs yours (hook backend, script runtime, transport) with notes on why theirs scales; the ethics page: instrumentation's legit empire (RE, malware analysis, debugging, CTF, research) vs its abuse (API-patch malware, cheat tools) — 300 words, your position, the guardrail (target-ownership) you'll keep.
Verify: architecture map done; three-vs-three comparison written; ethics page in `notes/di10-ethics.md`.
**Lesson check:** which of Frida's choices did YOUR rebuild validate (and which made you say "of course") — and what single fact separates a tool from a weapon here?

## DI11 — the product: your frida-lite CLI
Concept: the deliverable shape: spawn/attach, script load, trace, memory, dump, backtrace from ONE CLI; good UX for analysis. Do: integrate everything into `frida-lite` (your name, your CLI): `attach <pid>`, `script <file>`, `trace <symbol>`, `scan <pattern>`, `dump <addr> <len>`, `bt`; run it against your own test binaries AND one authorized CTF/learning binary (a real "find the flag logic" trace); the UX pass: your own usage notes (what's slow, what's missing).
Verify: CLI session handles all commands; CTF binary solved-trace logged; usage notes written.
**Lesson check:** why does a CLI matter beyond the engine (what does a tool become without UX) — and which THREE commands got the most use in real tracing?

## DI12 — CAPSTONE: rebuild the core, cold
Prereq: DI0–DI11. **Close all notes.** Cold: rebuild injector + inline hook + script attach in one sitting (no notes), then live: spawn/trace/dump/backtrace a session against a fresh test binary with a scripted onEnter/onLeave trace; one Windows-lane hook demo from memory. Write `labs/di-capstone.md` like a tool author's: architecture, protocol doc, hook-engine doc, the ethics page (DI10), three proud decisions, regret, roadmap (JS runtime, multi-arch, live UI).
**Pass = the full frida-lite loop works cold (inject → hook → script → explore) and the ethics page reads like a maintainer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in DI0/DI2 boilerplate (ptrace scaffold, socket handler) — injector, engine, scripts, parser written by you; erase-and-retry once when stuck; Frida = architecture reading, not code.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. Scope discipline: own binaries + course CTF only; no unauthorized targets, ever; anti-instrumentation studied, not weaponized; readings mapped, not copied.
5. Honest bar: Frida is a decade of engineering (multi-arch backends, JS runtime, corporate-grade); this course's bar = a working injector→hook→script→explore stack on Linux (Windows/mac mapped), real traces on real binaries, an argued ethics position, proven cold at the capstone — the floor for dynamic-analysis tooling and the complete answer to "how do tools like Frida actually work".

## Where this lives
Feeds from 15-07 mini-debugger / 15-08 mini-disassembler (your CPU-floor), and the reverse side of MALWARE-country: every EDR/malware-analysis shop runs instrumentation daily; ROOTKIT-DEFENSE's hook-audit is this course's engine seen from the blue side; the CTF lane wires to your 09-real-chains step.