# Rootkit & Boot-Integrity Course — Absolute-Beginner-audited (rootkit MECHANICS minus evasion + the defense that catches them, gated)

**This is the honest version of "write my own rootkit/bootkit."** Rootkits (kernel-stealth, persistence) and bootkits (UEFI boot-chain takeover) are the most dangerous malware classes, and their defining feature — hiding from the defender — is evasion that this repo does not build BY DESIGN (the Windows-MALWARE Defender-ON line, the KERNEL-EXPLOIT no-ship line, extended). What this course DOES: teach the full mechanics — syscall interposition, hooking, kernel object manipulation (DKOM-class), the entire boot chain — behind **non-evading, self-announcing labs**, plus the complete defensive twin: kernel integrity checking, hook auditing, boot measurement, and anti-rootkit thinking. Prereqs: DESKTOP-OFFENSIVE-PATH W1–W7 or LINUX-SECURITY K-units + WINDOWS N5–N7 (you build kernel modules already, non-stealth). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/rkN-quiz.md`)** — no advance without both. ~3h/unit, 10 units + capstone ≈ 7-8 weeks.

Compass (re-read when lost): a rootkit's skill is the SKILL, not the hiding: kernel callbacks, hook points, object lists, boot stages — those are functions. Stealth is a crime, not a craft. Every unit builds the function and then the thing that detects it — you become the engineer who writes both the interposer and the integrity checker, the boot analyzer and the measurement verifier. The profession this trains for is EDR, boot security, and DFIR — the rooms where rootkits are caught, not written.

Safety hardline: labs only (VMs, snapshot discipline, Secure-Boot-capable VM for the boot units); kernel components are **self-announcing** (log to dmesg, visible module names, no persistence, no hiding anything); real rootkits/bootkits = reading-only (LoJax/BlackLotus-class papers, unit RK6); no malicious artifact is ever produced; nothing ships.

---

## RK0 — hello world: taxonomy and the line
Concept: rootkit categories (kernel module hooks, DKOM, inline hooking, bootkit/UEFI), and why evasion is the line. Do: taxonomize the classes with their detection countermeasures (hook audit, integrity checks, PatchGuard, measured boot); read DESKTOP W7's rootkit section; write `notes/rk0-line.md`: why this course builds function-minus-evasion and who the profession serves.
Verify: taxonomy table (class × how-it-works × what-detects-it) + your line-page written.
**Lesson check:** what one feature separates a kernel module from a rootkit — and why is that feature the one this course refuses to build?

## RK1 — interposition: kernel components that watch (not hide)
Concept: kernels support callbacks and syscall layers LEGITIMATELY (auditing, sandboxes, EDR) — the function under every rootkit trick. Do: in your lab kernel: syscall interposition that LOGS (a visible syscall-audit module echoing every execve to dmesg, module name in `lsmod`, removal after demo); process/credential notify callbacks (same visible discipline); a demo script proving both fire on your own commands.
Verify: execve log proves interposition; module visible + cleanly removed; callbacks fire.
**Lesson check:** which legitimate products do exactly this — and what would "evasion" ADD to your module that you deliberately did not build?

## RK2 — hooking mechanics, read then re-create (announced)
Concept: hooking = redirect: syscall-table patching, inline hooks, ftrace/kprobes, Windows SSDT/PatchGuard. Do: reading unit on hook mechanisms (kprobes on Linux, inline-hook theory, why PatchGuard exists); re-create the SAFE version: a kprobe-based tracer (Linux) or ETW-enabled trace consumer (Windows per N9) that announces itself + logs — build the MECHANISM, skip the stealth; detect your own hook with a hook-inventory tool (kernel module list + probe listing).
Verify: kprobe/ETW trace works visibly; your inventory lists it; the "what would hide it" column written (theory only).
**Lesson check:** what does PatchGuard protect — and why does inline hooking risk the very stability defenders bank on?

## RK3 — DKOM mechanics: kernel objects, honestly
Concept: DKOM manipulates live kernel structures (process lists, tokens) — legitimately used by debuggers and forensics; hiding is the abuse. Do: read the DKOM technique family (EPROCESS/list manipulation on Windows, task_struct on Linux); safe re-creation: a lab module that ANNOTATES (adds a visible, logged flag to a process it picks) without hiding it — the manipulation demonstrated, the concealment refused; detector: a list-walker that validates the visible state and logs "sanity check" on your own lab process.
Verify: annotation module works + is visible; your list-walker logs the expected state; no hiding code exists.
**Lesson check:** what does DKOM touch that a hook doesn't — and why do forensic list-walkers exist specifically because DKOM abuses lists?

## RK4 — the detection twin: integrity and hook audits
Concept: detection = validate the trusted state: module hashes, hook inventories, callback audits, baseline comparisons. Do: build TWO detectors: (1) hook/module integrity checker (Linux: verify loaded module list + kallsyms integrity of a probe point; Windows: audit registered callbacks per N9/EDR), (2) baseline integrity (hash your own /boot + kernel modules, compare, flag deltas); run against your RK1–RK3 labs — they should be FOUND (that's the test); then self-check: a clean boot shows zero deltas (false-positive discipline).
Verify: detectors flag your own lab components; clean-boot run shows zero flags; report written per detector.
**Lesson check:** what does an integrity checker assume (a trusted baseline) — and why does anti-rootkit ALWAYS race (what can it never fully close)?

## RK5 — the boot chain: where bootkits live
Concept: boot = firmware (UEFI/BIOS) → bootloader → kernel: Secure Boot verifies signatures, measured boot records hashes into TPM PCRs — the ground bootkits attack. Do: map the boot chain with trust points (where signature checks happen, where TPM records); in a Secure-Boot-capable VM: enable/disable Secure Boot, observe behavior difference (unsigned vs signed boot artifacts); TPM tools: read/quote PCR values, extend a PCR manually, verify a measurement; document every trust point.
Verify: SB on/off behavioral comparison documented; PCR extend + quote verifiable with your tools.
**Lesson check:** what does Secure Boot verify that measured boot does not (and vice versa) — and why are they a PAIR?

## RK6 — bootkits, read-only: LoJax and BlackLotus
Concept: the modern bootkit classes and the artifacts that caught them. Do: reading-only study: LoJax (UEFI persistence), BlackLotus (signed-bootkit chain) — mechanism, where each attached in YOUR RK5 map, what finally exposed it (vendors, measurement, forensics); papers/researcher posts only; the "lingering artifact" lesson: why bootkits make forensic goldmines.
Verify: RK5-map annotated with both bootkits + "caught by" columns; two pages written.
**Lesson check:** what does a bootkit persist IN that a kernel rootkit can't touch — and why does that make it both scarier and MORE detectable?

## RK7 — build the defensive boot side: your measured-boot lab
Concept: the countermeasure in your hands: measure, attest, verify. Do: your own boot-integrity lab: a script/tool that snapshots PCR values + boot artifacts at clean boot, then re-measures and verifies post-change; SIMULATE (with a lab artifact, e.g., an altered option in the VM's UEFI config) → your verifier flags the delta; the "rollback attack" concept (why firmware updates + non-volatile PCRs matter) as reading.
Verify: your verifier flags the lab alteration; clean state re-passes; rollback-reading notes written.
**Lesson check:** why do you need measurement BEFORE trust (what can't you validate post-hoc) — and what is the TPM's role that software alone can't play?

## RK8 — anti-rootkit engineering: your own full loop
Concept: the product: combine detectors: hooks + integrity + callbacks + boot baseline into one check suite (your "mini anti-rootkit"). Do: integrate RK4's two detectors + RK7's boot verifier into a single `rk-audit` tool (multipOS if desired: Linux + Windows variants per your floors); add the audit-report format (finding, confidence, evidence); run the suite on a clean VM (zero findings) and a VM hosting your RK1–RK3 labs (all four finds + evidence); write the "why defenders can't rest" essay.
Verify: full-suite output on both states (clean=0, lab=4 finds), reports saved; essay written.
**Lesson check:** what does a finding MEAN when the adversary is also in-kernel (can audit be trusted?) — and where does that leave defense (hardware roots)?

## RK9 — the policy essay: evasion is regulated for a reason
Concept: this skill's market is defense: EDR, anti-rootkit vendors, boot security, DFIR. Do: the 400-word essay (your position): why evasion code is treated as malware by law (CFAA/DMCA-class, unauthorized access intent), where your mechanics knowledge legitimately lives (defensive products, research, remediation), the guardrail checklist (no hiding code, no persistence, labs only, disclosure-first); defend it in notes.
Verify: essay + checklist written; you can answer "why should I trust you" from it.
**Lesson check:** which single artifact turns a lab demonstration into a prosecutable item — and what does the essay REQUIRED here do that the skill alone doesn't?

## RK10 — CAPSTONE: the audit wins, cold
Prereq: RK0–RK9. **Close all notes.** Cold: rebuild ONE interposer (non-evading, syscall-log), its detector (hook/module integrity check), and run the boot-verifier — against your own lab in one sitting; produce the three-way report (lab state, findings, clean-state baseline) + the RK9 essay as cover. Write `labs/rootkit-capstone.md` like an EDR engineer's: architecture, the line-page, the findings format, roadmap (live monitoring, TPM attestation deeper, multi-OS audit).
**Pass = the full loop (build → audit → report) works cold; the line-page and essay are your own.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in RK0/RK10 boilerplate (module scaffold, audit harness) — interposers, detectors, verifiers written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. **Line-hardline: labs only, self-announcing components, no hiding, no persistence, no malicious artifact, real malware/bootkits = reading only; snapshots always.**
5. Honest bar: real anti-rootkit engineering is a vendor-research profession (ESET/Kaspersky/Bitdefender-class teams, academic boot security); this course's bar = mechanics mastery of every rootkit class, a working audit suite that catches its own labs across boots, and boots-verification built by your hands — the floor for defensive-kernel and boot-security engineering, proven cold at the capstone.

## Where this lives
The defensive twin of DESKTOP-OFFENSIVE-PATH's W7 rootkit gate (same mechanics, opposite purpose); pairs KERNEL-EXPLOIT's mitigation thinking, EDR-COURSE's detection craft, and your WINDOWS-MALWARE reading. One pair of hands: understand the threat, audit the machine, own the blue side.