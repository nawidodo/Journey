# Mobile Offensive Path — the full iOS & Android attacking surface, gated

Goal: understand and practice **every** offensive capability on iOS and Android — not only kernel/jailbreak-root (that's [`JAILBREAK-ROOT-PATH.md`](JAILBREAK-ROOT-PATH.md)) but app RE, dynamic hooking, on-device traffic, keychain/secure storage, persistence, hardware-adjacent surfaces, and evasion. Same rules: **a gate unlocks the next only when its artifact exists**; no skip; own hardware only; lab-only; re-derive, don't copy; artifacts are runnable/logs, not prose.

Prereq for all M-gates: **G0 completed** (JAILBREAK-ROOT-PATH.md — C, xv6, exploitation primitives, Ghidra). Lanes A/B (kernel) help but don't gate M3+ except where noted.

---

## M0 — instrument your target (the offensive workhorse)
Finish: `19-05` iOS hooking → `19-06` Android hooking → `19-07` cross-platform hooking capstone + `24-115` iOS app analyzer (you parse the app itself) + `15-03` crackmes (RE reflexes).
**Gate artifact:** `labs/my-hook.md` — one target app (your own or an open-source build) instrumented with your own hook: method/function hooked, before/after behavior, log evidence. → unlocks M1.

## M1 — app reverse engineering, complete
Prereq: M0. Finish: `15-01` Ghidra workflow (full) + `15-02` kernelcache (already done in JAILBREAK-ROOT-PATH L2 — reuse) + `16-02` Android app attack surface (exported components/intents) + `08-01` JS-engine series (hybrid apps) + `24-94` PDF / `24-107` subtitles if media formats appear in targets (format literacy).
**Gate artifact:** `labs/app-re.md` — full RE dossier of a chosen app: technology stack, obfuscation layer, hardcoded secrets found, attack-surface inventory (exported/intents, URLs, IPC). → unlocks M2.

## M2 — on-device traffic & network offense
Prereq: M1. Finish: `24-20` sniffer (you built the packet tool) + `20-07` own TLS (you understand what you'll bypass) + `24-102` HLS (streaming surface) + `04-08` networking read (Android nsd/network stack; iOS CFNetwork reading).
**Gate artifact:** `labs/traffic.md` — your target app's traffic fully captured on your own device (VPN hook or proxy you configure), TLS-pinning bypass demoed on **your own app**, one protocol tampering test on your own test server. → unlocks M3.

## M3 — storage, keychain, backups (the data exfil school)
Prereq: M2. Finish: `20-05` OS crypto stores (Keychain/Keystore internals) + `24-115` plist/Mach-O (already built; reuse for .mobileprovision/entitlements) + `24-99` DMG + `24-33` APFS (disk-level where jailed) + `16-02` app-sandbox container layout.
**Gate artifact:** `labs/storage.md` — on your own device/target: enumerate an app's container, locate secured items, demonstrate the difference between Data-Protection-class-accessible vs TEE-held (your words + logs). → unlocks M4.

## M4 — persistence & the mobile implant
Prereq: M3. Finish: `12-04` C2 implant (the cross-platform agent core) + `17-02` Android implant core + persistence + `17-03` C2 & evasion + `24-106`/`24-115` (your own tools feeding the implant) + `12-08` anti-forensics (defensive frame).
**Gate artifact:** `labs/my-implant.md` — own mobile implant (your agent, your C2) on your own test device: deploy, beacon, exfil one file, kill-switch — full loop. → unlocks M5.

## M5 — evasion & detection round-trip
Prereq: M4. Finish: `17-05` detection-defense read + `12-03` AMSI/ETW evasion (desktop techniques transfer) + `21-06` mini-EDR (you build the blue side) + `24-79` ML detector (you build a detector too).
**Gate artifact:** `labs/evasion.md` — your M4 implant detected by your own EDR/YARA, then hardened, then re-caught, then re-hardened — the before/after table. → unlocks M6.

## M6 — hardware-adjacent surface (BT/USB/NFC/TEE)
Prereq: M5 (kernel lanes optional). Finish: `26-01..07` BT (own Mac/Android radio) + `13-01..06` USB attack surface + `16-07` OP-TEE lab + `24-116` BLE scanner + `22-12` CAN (car context bonus) + `28` Wi-Fi (if you want the air lane).
**Gate artifact:** `labs/hw-adjacent.md` — one hardware-adjacent attack on your own device/radio (BLE gadget pair exploit, USB gadget role, or TEE boundary test), before/after. → unlocks M7.

## M7 — capstone: full-stack mobile offensive engagement
Prereq: M0–M6 (+ JAILBREAK-ROOT-PATH L6/R4 recommended). One own-device engagement end-to-end: target = your own app + your own device. Recon (M1) → instrument (M0) → traffic (M2) → storage (M3) → implant+persist (M4) → evade your EDR (M5) → hardware surface (M6) → write the full disclosure-grade report (`labs/full-engagement.md`): attack tree, each primitive with evidence, detections you'd ship, fixes.
**Gate artifact:** the report + the running chain. **Pass = you did it without opening a tutorial.**

---

## Rules
1. Same as JAILBREAK-ROOT-PATH: no gate skip; runnable artifacts; own hardware + Wi-Fi-off during active phases; re-derive; realistic bar = competence, not 0-day hunting.
2. Interleave: M-gates 0–3 can run alongside Lane A L1–L3; M4+ pairs Lane B R4/R5.
3. Timeline (not packed): M0–M3 ≈ months 4–7, M4–M6 ≈ months 8–10, M7 ≈ month 11–12 — interleaved with the kernel lanes.
4. Every M-gate reuses steps you already finished in the other lanes — the wheel is never reinvented, only aimed at a new surface.

## Where this lives
`steps/` unchanged — the route threads 19, 15, 24, 20, 12, 17, 21, 26, 13, 16, 28. Cross-link: JAILBREAK-ROOT-PATH = kernel/trust; this doc = every other surface; together they are the complete mobile offensive skill.