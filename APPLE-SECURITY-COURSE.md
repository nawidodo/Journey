# Apple Security — Absolute-Beginner Course (hello world → kernel-primitive re-creation, gated)

Zero Apple-security knowledge assumed. You need: a Mac (any Apple Silicon), Xcode, and a willingness to type. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/aN-quiz.md`)**. No advance without both. ~2h/unit, 12 lessons + capstone ≈ 4–5 weeks. When done you enter [`JAILBREAK-ROOT-PATH.md`](JAILBREAK-ROOT-PATH.md) Lane A at L4 — with the floor already laid.

Compass (re-read when lost): Apple security is a *chain of signing + sandboxes + kernel hardening*: code must be signed → apps run sandboxed → the kernel is hardened (KTRR/PPL/PAC/APRR/SSV). Every attacker step in this course is "which link of that chain am I touching?" Security work = find the links that bend.

Safety: your own Mac/device only; Wi-Fi off for device units; you re-create public primitives, never hunt 0-days; wreck your own VM proudly, wreck no one else's system.

---

## A0 — the first binary and the first signature
Concept: everything on Apple is signed; even hello world is. Do: Xcode CLI "hello" in C → run → `codesign -s - hello` (ad-hoc) → read `codesign -dvvv hello` and `otool -L hello` → check SIP: `csrutil status`.
Verify: you can print your binary's signature fields and linked libraries.
**Lesson check:** what does `codesign -dvvv` show, and what is SIP's job in one sentence?

## A1 — C and memory, the 101 that everything rides on
Concept: stack, heap, pointers, array indexing — and what out-of-bounds means. Do: 00-01 C basics (ch. 1–6, one week) + a deliberately bad program: `char b[4]; strcpy(b, "WAY TOO LONG");` — see garbage, then crash. Guards on; you're learning the crash on YOUR machine.
Verify: you can print stack garbage from the overflow and explain what happened.
**Lesson check:** draw a stack frame: locals, saved return address, and where the overflow lands.

## A2 — read the binary yourself (Mach-O lite)
Concept: a Mach-O = header + load commands + segments; `__TEXT` code, `__DATA` data. Do: `xxd hello | head`, `otool -l hello`, then write a ~30-line C/Swift parser printing magic, CPU type, load-command count, and segment names of your hello binary. (24-97 builds on this later.)
Verify: your parser's output matches `otool`'s on the same file.
**Lesson check:** name three load commands and each one's job.

## A3 — processes: I own it, now I touch it
Concept: fork/exec → dyld loads the binary + dylibs → libraries can be injected. Do: build your own tiny dylib (prints "INJECTED") and load it into YOUR hello app via `DYLD_INSERT_LIBRARIES`; watch the message appear; then `sample`/`log stream` to see your process live.
Verify: your app prints INJECTED from your dylib, with a log line you can point to.
**Lesson check:** what does dyld do, and why is controlling dylib loading the oldest Apple offensive lever?

## A4 — the kernel seam: syscalls
Concept: userland asks the kernel via traps — BSD syscalls (POSIX) and Mach traps (XNU-specific); darwin syscall table maps numbers. Do: write a raw syscall (no libc): `syscall(4, fd, buf, len)` for write; trace your own process with `sudo dtruss` (or `sample`); find where `dyld` hits the kernel first.
Verify: raw-syscall write prints, and the trace shows your syscall numbers.
**Lesson check:** BSD syscall vs Mach trap — same mechanism, different tables; say it in your words.

## A5 — sandboxes and entitlements
Concept: even root apps live in sandboxes; entitlements are signed claims (files, jetsam, containers). Do: run your app under `sandbox-exec` with a profile denying all reads → watch it fail → `codesign -d --entitlements` on your own binary; add a synthetic entitlement; re-sign.
Verify: sandbox denial visible in log; entitlement list readable on your binary.
**Lesson check:** what does an entitlement grant that a signature alone doesn't?

## A6 — local exploitation primer (your first control-flow break)
Concept: overflow the stack → overwrite the saved return → the CPU jumps where you say. Do: write vulnerable C, compile `-fno-stack-protector -no-pie`, crash it in lldb, watch registers (`x0..x30`, `pc` on arm64) — drive the crash to hit an address YOU chose.
Verify: lldb shows `pc` (or saved-link-register) equal to your chosen value = controlled crash.
**Lesson check:** which register/saved value must the attacker control to hijack control flow, and why?

## A7 — the mitigations that stopped you
Concept: ASLR (randomize load), stack canary (sentinel), NX (non-exec stack), then Apple's PAC (pointer signing). Do: rebuild A6's binary WITH canary → crash behavior changes; PIE vs no-PIE → `image list` addresses move; one real bypass: leak the canary in your own debug session and defeat it locally.
Verify: before/after table in `notes/a7.md` (mitigation → symptom → your bypass).
**Lesson check:** one line each: ASLR, canary, NX, PAC — what each breaks.

## A8 — kernel truth-check: the same crime against a real kernel
Concept: userland bugs are contained; kernel bugs are the keys. Do: on a Linux VM (or xv6 from 01-xv6, run in QEMU), port your A6 overflow into a kernel-context exercise (xv6 lab trap/system call path); then re-read how Apple's KTRR/PPL/PAC/APRR/SSV (04-07 list) blocks each classic post-overflow step.
Verify: xv6 lab exercise solved + one-page "what Apple blocks and how" (`notes/a8.md`).
**Lesson check:** pick one classic exploitation step and name the Apple feature that kills it.

## A9 — meet the kernelcache
Concept: Apple ships the kernel as a compressed signed blob in the boot chain; the kernelcache is RE food. Do: locate your Mac's kernelcache (`/System/Library/KernelCaches/`), extract strings, find kernel version data; locate the boot-stage position in the chain diagram from 04-07.
Verify: you pulled a kernel version/string out of the blob yourself.
**Lesson check:** where in the boot chain does the kernelcache live, and what protects it at that stage?

## A10 — the jailbreak history map
Concept: every jailbreak = one bug class + one mitigation defeated. Do: build the era table for checkm8 (BootROM bug) → tfp0 (patchfinder/kernel r/w) → KFD (current, file-ops abuse) — read one public writeup per era.
Verify: 5-row era table (`notes/a10.md`): era, bug class, mitigation defeated, device vintage.
**Lesson check:** how did the attacker's path shift when bootrom hardening arrived (bootrom era → kernel-era)?

## A11 — your primitive re-creation (the honest lab)
Concept: competence = re-create, not copy. Do: on your own device (or fully-documented VM), re-write the *class* of a KFD-stage primitive — e.g., a userland→kernel file-ops confusion PoC you write from the public description, NOT source. Capture every step; Wi-Fi off; device wiped after.
Verify: `labs/a11.md` — PoC ran, logs captured, stage-by-stage notes.
**Lesson check:** name the stages of your primitive and why that order.

## A12 — CAPSTONE: from hello world to the chain, in your words
Prereq: A0–A11 passed. **Close all notes.** Write `labs/apple-capstone.md`: a disclosure-grade narrative of YOUR arc — hello binary (A0) → your first controlled crash (A6) → canary bypass (A7) → xv6 kernel context (A8) → kernelcache (A9) → your re-created primitive (A11) — plus the mitigation-bypass ordering and the patch timeline you'd propose. Re-run A11's PoC once, cold.
**Pass = the narrative is accurate with every artifact referenced, and the PoC runs without notes open.** Then re-open docs → JAILBREAK-ROOT-PATH Lane A L4.

---

## Rules
1. Verification + lesson check both true before next unit; quiz in own words.
2. Copying allowed only in A5's sandbox profile (tool boilerplate) — everything else written from concept; erase-and-retry once when stuck.
3. 2h/unit timebox; stuck past that = previous unit's verification again.
4. Own Mac/device only; Wi-Fi off for A10/A11 device work; devices wiped after runs.
5. Honest bar (stated plainly): a modern-shipping-macOS 0-day is multi-year research. This course's bar = you can build, parse, inject, sandbox, crash, bypass, trace, map, and re-create a public primitive — the competence floor every Apple security job assumes. The capstone is that floor, proven cold.