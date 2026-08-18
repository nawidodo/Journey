# Jailbreak & Root Path — a prerequisite-locked chain to your own iOS jailbreak and Android root

Goal: on your own devices, write the exploit chain that lands you a working jailbreak (iOS) and root (Android) — assembled from primitives you re-derived, not copied. Each gate **unlocks the next only when its artifact exists** (Exit-Criteria boxes ticked in the referenced step READMEs + the gate's own deliverable in `labs/`). No gate skip; the chain is the point.

Safety: own device only (one dedicated iPhone/iPad + one Android you own, Wi-Fi-off during tests), lab-only, re-derive public classes (checkm8 → KFD family; Linux/Android root gaps). You are learning competence, not chasing 0-days — a re-derived, *working* chain on your own hardware is the benchmark.

---

## G0 — Foundations (everything below depends on this)
Finish, in order, each artifact ticked:
1. `00-01` C (K&R) — C is the language of every link below.
2. `00-02` memory model — pointers/stack/heap; the model every exploit mutates.
3. `01-xv6` all 10 steps — **the OS ground-up: syscalls → pgtbl → traps → threading → fs → net** (04: SV39 page tables is the map every memory bug walks).
4. `03-01` buffer overflow → `03-02` ROP → `03-03` heap/UAF — the three primitive families.
5. `03-04` mitigations — ASLR/canary/NX/SMEP/SMAP (and PAC later).
6. `03-05` pwn.college — reps until the primitives are reflex.
7. `15-01` Ghidra workflow — you must read binaries before you mutate them.
**Gate artifact:** `labs/build-own-note`: a one-page writeup with a diagram of *one* stack overflow you wrote, traced to the exact xv6 pgtbl page table that let it land. → unlocks L1.

## Lane A — iOS (L1…L8)

### L1 — arm64 + the Apple trust chain
Prereq: G0. Finish: `04-05` arm64 basics → `04-06` XNU internals → **`04-07` Apple security guide (KTRR/PPL/PAC/APRR/SSV)** — this gate is the map of what you must defeat.
**Gate artifact:** `labs/apple-trust-chain.md` — draw the full boot chain on one page, annotating each link's protection and naming the one public exploit class that hit it. → unlocks L2.

### L2 — read the target, kernel-level
Prereq: L1. Finish: `15-02` XNU kernelcache RE (IDApython/Ghidra practice) + `24-97` Mach-O parser (you build the format reader yourself — `labs/` shows it) + `24-110` arm64 playground (syscalls/PIC on your own Mac — the ISA becomes muscle).
**Gate artifact:** pick one kernelcache function (e.g., `IOLockSleep` family killer) — disassemble, name its structs, write it up with your Mach-O parser feeding your disassembler. → unlocks L3.

### L3 — the sandbox you must leave
Prereq: L2. Finish: `06-01`→`06-03` (container/seccomp escapes — the class) + `06-04` macOS Seatbelt + `06-05` iOS/macOS escape techniques.
**Gate artifact:** `labs/sandbox-escape-card.md` — for each iOS app-sandbox boundary (file, mach, network), one escape primitive and which public chain used it (Trident era: the JS→sandbox→kernel path). → unlocks L4.

### L4 — kernel exploit primitives, iOS era
Prereq: L3. Finish: `07-01` checkm8 (bootrom — how the chain fires from reset) → `07-05` KFD (the current engine) — read the public analysis until you can explain each stage — → `05-03` Dirty COW + `05-05` Dirty Pipe (the shared page-cache/UAF ancestry of iOS kernel bugs).
**Gate artifact:** `labs/kfd-stage-map.md` — KFD's stages (oof/physpuppet/cVE getter) re-explained with your own diagrams and one PoC-class re-creation on your own device captured as evidence (syslog + exit-boxes). → unlocks L5.

### L5 — PAC/pointer-auth bypass
Prereq: L4. Finish: `07-06` arm64e PAC deep-dive + `03-04` PAC section re-read.
**Gate artifact:** `labs/pac-bypass-note.md` — the three PAC-bypass families (ret2dir-style, JOP, signing oracle) with the one your chosen chain uses, justified. → unlocks L6.

### L6 — assemble YOUR jailbreak runtime
Prereq: L5. Finish: `07-07` Fugu15 → `07-08` Dopamine source (`libjailbreak`/`launchdhook`/`systemhook`/`choma`) — read the product code that turns a kernel primitive into a working jailbreak + `24-104` dylib injector (you already built the loader-side injection!) + `24-109` plist + `24-97` (you own the format layer).
**Gate artifact:** on your own test device: primitive → bootstrap → your own tweak injected via YOUR code. `labs/live-jailbreak.md` with logs. This is **your jailbreak** — assembled, not downloaded. → unlocks L7.

### L7 — the chain, end to end (your writeup)
Prereq: L6. Finish: `09-01` ForcedEntry → `09-04` Syzkaller-XNU (read how majors chain) + `09-03` Pwn2Own-style writeup reading.
**Gate artifact:** `labs/my-jailbreak-README.md` — your full chain documented as a disclosure-grade writeup: bug classes, mitigation-bypass order, reproduce instructions, patch timeline. → unlocks L8.

### L8 — capstone: re-derive blind
Prereq: L7. **Close all notes** (the 24-43 systems-gauntlet method): rebuild your jailbreak from the kernel back with only the public CVE numbers as hints. 
**Gate artifact:** working jailbreak #2 + `notes/rebuild-diff.md` (what you forgot, what you now own). Pass = your own re-build runs. This is the year's proof.

## Lane B — Android (R1…R6)

### R1 — Android security model
Prereq: G0. Finish: `16-01` model (SELinux/permissions) + `16-02` app attack surface + `24-110` arm64 (same ISA, your muscle).
**Gate artifact:** `labs/android-model.md` — the sandbox/SELinux/packagemanager boundary map with the one kernel primitive that crosses it (binder/UAF class). → unlocks R2.

### R2 — Android kernel surface
Prereq: R1. Finish: `16-03` binder + kernel surface → `16-04` real Android kernel CVEs (read 2, explain the root cause class in your words) → `05-06/05-09` Linux heap-grooming classes (arm64-adjacent).
**Gate artifact:** `labs/kernel-surface.md` — binder call paths + 2 CVE root-cause cards. → unlocks R3.

### R3 — bootloader/TrustZone boundary
Prereq: R2. Finish: `16-05` rooting/bootloader/TrustZone + `16-07` OP-TEE lab (the TEE you'll flash around).
**Gate artifact:** `labs/boot-chain.md` — your device's unlock→boot→TEE chain; what root kits must bypass, which part is open (vendor unlock) vs closed (SLA). → unlocks R4.

### R4 — assemble YOUR root
Prereq: R3. Finish: `16-06` capstone root-chain study + `24-118` io_uring (kernel-API literacy) + `24-119` ETW (telemetry reverse view).
**Gate artifact:** on your own device: root achieved via your own assembled chain (KernelSU-style init or re-derived vendor-gap), `labs/my-root.md` with adb evidence. → unlocks R5.

### R5 — detection round-trip
Prereq: R4. Finish: `21-06` mini-EDR (you build detection) — write the rules that catch your own root — + `17-05` detection-defense read.
**Gate artifact:** `labs/root-detection.md` — your root triggers your EDR, you fix, re-run (before/after). → unlocks R6.

### R6 — capstone: root re-derived blind + dual-platform synthesis
Prereq: R5 (+ L7 recommended). Rebuild root with notes closed. Then write `notes/cross-platform.md`: iOS-jailbreak vs Android-root — same primitives (UAF/race/PAC-vs-SELinux), different fences. Pass = re-build runs + comparison writeup.

---

## Rules
1. **No gate skip** — a gate's artifact missing = you're back on the previous gate.
2. **Artifact = runnable evidence** — screenshots/logs in `labs/`, not prose claims.
3. **Own hardware only**, Wi-Fi off during exploitation, device wiped after each chain run.
4. **Re-derive, don't copy** — reading is fuel; your chain must run from YOUR code.
5. **Timeline (not packed)** — G0: ~3 months; Lanes: ~6–9 months each at 8–10 h/wk; run lanes interleaved (L4 ↔ R2 share kernel-grooming reps).
6. Realistic bar: a self-written 0-day jailbreak is a multi-year research career. This path's bar is the *competence standard*: re-derived primitives, assembled into a working chain on your own device, documented disclosure-grade. That is the skill employers and projects hire.

## Where this lives in the plan
Replaces no existing phase — it is the *gated route* through Steps that already exist: G0 (00/01/03/15), L1–8 (04/07/09/24), R1–6 (16/21/24/17). Mark it against the year: G0 = months 1–3, Lane A main = months 4–9, Lane B interleaved = months 6–12.