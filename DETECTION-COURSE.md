# Hook & Debugger Detection Course — Absolute-Beginner (hello world → YOUR OWN cross-OS debugger/hook detection suite, gated)

Zero detection knowledge assumed — but you carry the weapons: INSTRUMENT-course hook-vision, INTEGRITY-course runtime guards, ROOTKIT-DEFENSE hook audits, EDR-course checks. This course fuses them into ONE discipline: detect that your own app is being debugged, hooked, or instrumented — across all four major apps-hosts (Linux, macOS, Windows, Android; iOS skeleton'd with its wall). Detection is BLUE work: it protects your software, your game, your product. Every unit builds a detector and proves it against REAL tools you run in your own lab (gdb, lldb, x64dbg, Frida, dtrace-class). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/dtN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): debuggers and hooks are detectable because they MUST leave tracks: a ptrace tag on the process, breakpoint bytes in code, a modified import table, a foreign thread or library, timing deltas. Detection = knowing where the OS keeps state and reading it against a baseline. The ladder: process-state signals (who's tracing me) → memory signals (was my code patched, my imports redirected) → kernel signals (what's loaded, what logged) → mobile signals (debug flags, hook frameworks, emulation). Each OS hides these in different places; the course maps ALL of them and builds one uniform detection suite.

**Honesty clause (unit DT0): all detection is dual-use** — the exact checks you build against debuggers are the ones malware uses against sandboxes. This course's line: you build detectors to PROTECT YOUR OWN applications; the evasion use of the same signals is analyzed as READING ONLY (DT8), never built. Detection itself can be detected — the course teaches that truth as a design constraint, not a trick to weaponize.

---

## DT0 — hello world: what a debugger/hook leaves behind
Concept: taxonomy of signals: process-state, memory, kernel, timing — and the layer map per OS. Do: the signal taxonomy table (signal class × artifact × OS where it lives); hello: run your own tiny binary under gdb in the VM and WITHOUT — diff /proc/self/status (TracerPid!) and the maps — the first two signals in your table, eyeballed; write the "what detection can and can't catch" note (vs INTEGRITY's runtime guard: what you're extending).
Verify: the gdb-vs-clean diff captured (TracerPid + maps delta); taxonomy table written.
**Lesson check:** why must a debugger leave process-visible state — and why does that single fact make detection a real (if racy) engineering field?

## DT1 — Linux: the process-state signal suite
Concept: ptrace signals: TracerPid, PTRACE_TRACEME self-attach, /proc/self/maps anomalies, /proc/self/wchan, timing (rdtsc-based delay probes). Do: implement the Linux detector module (TracerPid check, TRACEME test, maps-baseline diff, timing probe) in your app; the live test battery: clean run (all clear) vs gdb attach (detected) vs strace (detected); false-positive discipline (clean VM stays silent across 50 runs).
Verify: battery passes (gdb/strace detected, clean stays clean); module documented.
**Lesson check:** which Linux signals are EASIEST to fake by a sophisticated debugger — and what does that tell you about layering (what to pair with what)?

## DT2 — macOS: the locked-door signals
Concept: Darwin: ptrace(PT_DENY_ATTACH) (deprecated-ish but real), sysctl KERN_PROC (P_TRACED flag), task_for_pid availability dance, codesign self-check (from IT4), DTrace/LLDB-footprint reading. Do: write the macOS detector (KERN_PROC P_TRACED read — your own; PT_DENY_ATTACH behavior demo on your own app; task_for_pid probe); battery: clean vs lldb attach vs dtrace-class attach; the "why macOS gates task_for_pid" note (root/securityd — who gets the key).
Verify: battery passes; PT_DENY_ATTACH behavior demonstrated + documented (including its bypass-by-root truth).
**Lesson check:** what does macOS's tighter enforcement (deny-at-kernel vs detect-after) change about YOUR detection design — and what leaks through anyway?

## DT3 — Windows: the richest signal farm
Concept: Windows: IsDebuggerPresent, NtQueryInformationProcess (DebugPort, DebugObjectHandle, DebugFlags), manual PEB BeingDebugged read, hardware-breakpoint scan via GetThreadContext (DR0–DR3), heap/flag checks, timing; x64dbg/olly attach = the test subjects. Do: implement the Windows module (manual PEB read + NtQuery DebugPort + DR-register scan + timing) — the NT-native calls, YOUR code; battery: clean vs x64dbg vs windbg attach vs a custom inline-hook (from DI3 knowledge); the priority note (which signal fires first, reliably).
Verify: battery passes with evidence (which detector fired per tool); priority ranking documented.
**Lesson check:** why does Windows expose SO many debugger signals (architectural accident or design?) — and which TWO do you ship in production and why?

## DT4 — hook detection: the memory-truth suite
Concept: hooks live in YOUR code: inline jmp bytes (DI3 shape), patched IAT/imports (GOT on Linux, IAT on Windows, LazyBinding reality), redirected callbacks, foreign threads. Do: your uniform hook-scan: inline-byte baseline check over your own functions (hash vs installed-base), IAT-on-disk comparison (parse your own PE/ELF, compare resolved vs expected — reuse 15-08 parsers), call-stack sanity (return-address points into your module or a known-vetted one); round-trip: hook your own function with DI3's engine → your scan detects it; unhook → clean.
Verify: hook-scan detects/reports/clears round-trip on your own binary; foreign-thread note written.
**Lesson check:** why is the IAT/hook check the one every anti-cheat ships — and what hook TYPE does it routinely miss (what does a good hooker do to float under it)?

## DT5 — kernel-visible signals without a driver
Concept: what the OS logged: /proc/modules deltas (Linux), ETW/Sysmon events (Windows — EDR-course skill), dmesg deltas, loaded-framework lists (macOS); the "read the receipts" layer. Do: implement per-OS kernel-receipt checks: dmesg delta on your Linux app (debug events logged?), sysmon/ETW event query for your Windows process (process-access events = attempted attach), /proc/modules diff for debug-module load; the "what you can learn without a driver" essay.
Verify: at least 2 OSes show real receipts under attack (attach = new events); essay written.
**Lesson check:** what do kernel receipts see that in-process signals can't (the attacker outside your window) — and what's their cost (permission, noise, latency)?

## DT6 — Android: mobile debug + hook-framework signals
Concept: mobile is different: Debug.isDebuggerEnabled, TracerPid legacy, ptrace self, Frida detection (maps/ports/threads signatures — DETECTION rules from IT6 reading), Xposed/LSPosed detection (reading), adb/USB state, emulator signals. Do: write the Android detector module in your own app (JDWP flag, TracerPid, maps+ports scan for the classic Frida signatures, adb state); battery on emulator: clean vs debugger-attached vs your Frida attach attempt (detected by signature); the "why Android detection races QEMU-level hiding" note.
Verify: battery passes on the emulator; signatures documented as detection rules (never bypass).
**Lesson check:** why is mobile detection MORE dependent on signature-hunting than desktop — and what does an emulator-hiding adversary do to your confidence (the honest answer)?

## DT7 — iOS: the wall, read and skeleton'd
Concept: iOS: P_TRACED via sysctl, PT_DENY_ATTACH, jailbreak-file/dylib checks (common set), Frida's /usr/lib injection paths — all DETECTION reading; Apple's own hardening reduces the attack surface. Do: reading + Swift skeleton: the detection-checks set (sysctl read, deny-attach, jailbreak checks, Frida paths) COMPILED (full run needs device — honest ceiling); the "iOS vs Android detection reality" comparison (who has it harder and why); the sheet: which checks would be effective on a jailbroken device.
Verify: skeleton compiles; comparison sheet + effectiveness table written.
**Lesson check:** does iOS's locked model make detection EASIER or HARDER for your app — and what remains detectable on a jailbroken iPhone anyway?

## DT8 — the meta-layer: detection-of-detection, analyzed
Concept: sophisticated adversaries detect YOUR detector (timing on your checks, API-call patterns, behavior) — and malware uses the SAME signals to evade sandboxes; the dual-use truth. Do: the analysis unit: write the model "how my detector is detectable" (timing side-channel on my checks, abnormal call cadence, library surface); build a countermeasure-lite: randomized check order + check fusing (one pass collecting many signals) + lazy/one-shot checks; the 300-word essay: defensive detection vs malware evasion — the line, the reading-only rule, why the difference is purpose, target, and consent.
Verify: countermeasure-lite implemented (randomization measurable); essay written.
**Lesson check:** what does randomization actually protect against — and what is the "purpose, target, consent" test when a check is identical in both hands?

## DT9 — evasion-resistant design (defensively)
Concept: make detection hard to defeat: multi-signal correlation, baseline discipline, kernel/hardware assist (TPM latency attestation from INTEGRITY, SE attestation on mobile), coverage-gap honesty. Do: design pass on your suite: correlate signals (attach→traffic→timing→latency clusters score as ONE alarm), add hardware assist where you can (TPM quote snapshot from RK7 — attestation tie-in at app start), the gap doc: what a ring0 adversary (or driven emulator) beats — written, not hidden; implement one correlation alarm + one hardware-assist hookup.
Verify: correlation alarm fires on compound attack (attach+timing shift); gap doc honest.
**Lesson check:** why does CORRELATION beat single-signal checks — and what does the honest gap doc do for your product's trust?

## DT10 — the product: detect-suite
Concept: one CLI, per-OS modules, battery of live tests: status/watch/verify. Do: `dxdetect` (your name): `status` (one-pass signal report), `watch` (live monitoring + alarm), `verify` (deep scan incl. hook-set); the battery script: launches the REAL tools (gdb, lldb, x64dbg, frida, adb) against your test apps and asserts detection; run the battery on Linux+Windows (reachable) + Android emulator; report card per OS (signal count | reliability | FPs).
Verify: battery green on ≥3 hosts; report cards written; watch alarms demonstrated.
**Lesson check:** what's the difference between "detects" and "reliable" in this business — and what does your FP discipline look like in the report cards?

## DT11 — the real world, read: VAC/EAC/BattlEye and EDRs
Concept: the professionals: anti-cheat and EDR hook/debug detection (kernel drivers, attestation, machine learning); what's common knowledge vs proprietary. Do: reading: how a major anti-cheat detects debuggers/hooks (public engineering docs + leak analyses — READING), what EDR telemetry watches for process access (Sysmon event 10-style, from EDR-course); the "what you'd add if you had a driver" essay (your design grown up: ring0 callbacks, patchguard-safe, attestation); map their layers to your DT1-DT7 modules.
Verify: mapping table + driver-essay written; three "steal with attribution" ideas noted.
**Lesson check:** what does a KERNEL driver unlock that you couldn't reach — and why do even they still run userland correlation (what's the real bottleneck)?

## DT12 — CAPSTONE: the suite, cold
Prereq: DT0–DT11. **Close all notes.** Cold: rebuild ONE OS detector core + the hook-scan + the correlation alarm in one sitting (no notes), then run the full battery live: launch real tools from your lab against your fresh test app, catch them all; write the DT8 essay fresh. Write `labs/detect-capstone.md`: architecture, signal map, battery results table, gap doc, three proud decisions, regret, roadmap (driver-backed suite, ML correlation, hardware assist).
**Pass = the cold rebuild catches the live battery; the gap doc and essay read like a security engineer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in DT0/DT3 boilerplate (signature scaffold, NT-call wrappers) — modules, scans, correlation, suite written by you; erase-and-retry once when stuck; reference reading is analysis, never copy-paste.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Line: protects YOUR OWN apps; real tools attack only your lab artifacts; malware-evasion use = analysis-only (DT8); detection ≠ evasion — building bypass/evasion circuits is out.**
5. Honest bar: VAC/EAC/BattlEye and EDRs are multi-million-dollar kernel-plus-ML systems; this course's bar = a working cross-OS signal suite that reliably catches real tools in your lab with honest gap docs, proven cold at the capstone — the floor for detection engineering and the complete answer to "am I being watched".

## Where this lives
The detection continuation of INTEGRITY (guards) + INSTRUMENT (hook knowledge, reversed) + ROOTKIT-DEFENSE (audits) + EDR (telemetry); the mobile half extends IT6/IT7; the DT8 essay is the same dual-use truth VO10 asked you to sign — one family, one discipline, every major OS.