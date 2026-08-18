# Desktop Offensive Path — macOS/Linux/Unix/Windows, kernels & drivers by writing and manipulating

Goal: the desktop twin of the mobile lanes — understand the major OSes offensively by **writing the kernel/driver side and manipulating it**, plus full malware-dev and C2. Same contract: **a gate unlocks the next only when its artifact exists**; no skip; own VMs/hardware only; lab-only; re-derive, don't copy; artifacts are runnable/logs, not prose.

Prereq: **G0 completed** (JAILBREAK-ROOT-PATH.md foundations: C → xv6 → exploitation primitives → Ghidra). Lanes A/B (mobile) are independent; this lane interleaves after G0.

---

## W1 — Linux kernel rookie: module → exploit
Prereq: G0 (absolute beginner? two floors first: [`LINUX-SECURITY-COURSE.md`](../LINUX-SECURITY-COURSE.md) K0–K12, then the driver-build side [`DRIVER-COURSE.md`](../DRIVER-COURSE.md) V0–V12 — same trainer device under Linux/macOS/Windows).
Finish: `05-01` modprobe (first kernel trigger) → `05-02` ret2usr → `05-03` Dirty COW → `05-05` Dirty Pipe → `04-03` write a Linux module (the craft side) + `14-01` driver craft.
**Gate artifact:** `labs/linux-k1.md` — you wrote a vulnerable toy module, then exploited it with a PoC you wrote (module + exploit + log evidence in `code/`+`labs/`). → unlocks W2.

## W2 — Linux kernel depth: write it, then break it
Prereq: W1. Finish: `05-04` kernel ROP → `05-06/07/09` heap-grooming classes (vermin, kmalloc-cve, cross-cache) → `24-01` own OS (you build a kernel end-to-end) → `24-82` kernel gdb stub (you debug it live) → `24-84` init system → `24-118` io_uring + `05-14` own eBPF tool (kernel-API literacy).
**Gate artifact:** `labs/linux-k2.md` — your own-OS booted under your gdb stub, one heap-class exploit demonstrated against your own test kernel, hook/ebpf tool listing your own processes. → unlocks W3.

## W3 — macOS offensive: the trust chain you manipulate from userland
Prereq: W2 (or JAILBREAK-ROOT-PATH L1 if mobile-first). Finish: `04-07` Apple trust chain + `04-06` XNU + `24-103` process monitor + `24-104` dylib injector + `24-105` logarchive + `24-110` arm64 playground + `25-10` macOS privesc + `06-04` Seatbelt.
**Gate artifact:** `labs/macos.md` — own process injected & monitored end-to-end (inject → observe via your monitor → artifact in logarchive), one privesc class demoed on your own VM. → unlocks W4.

## W4 — macOS kernel-side: kext/dext-class territory
Prereq: W3. Finish: `18-05` macOS rootkit (kext/dext study) + `14-04` DriverKit dext + `07-05` KFD-era outputs for the kernel-primitive mindset (read-level) + `24-111` Metal (the GPU side of the platform).
**Gate artifact:** `labs/macos-k.md` — your own driver-runtime experiment on own VM (dext or kext-class stub), caught by your own EDR rule (W5 pairs), with the trust-chain diagram of what protected you from yourself. → unlocks W5.

## W5 — Windows kernel & drivers via HEVD
Prereq: W2 (absolute beginner? complete [`WINDOWS-SECURITY-COURSE.md`](../WINDOWS-SECURITY-COURSE.md) N0–N12 first — HEVD re-creation, token privesc, AD attacks, and the driver build are already done there). Finish: `11-01` userland exploit dev → `11-02` Windows internals/debugger → `11-03` driver RE (HEVD) → `11-04` pool-overflow/token stealing → `11-05` real CVEs → `11-06` mitigations/ROP → `14-03` KMDF driver craft → `11-07` capstone.
**Gate artifact:** `labs/windows-k.md` — two HEVD classes exploited + one real CVE re-derived on your own Windows VM + your own KMDF driver compiled and loaded. → unlocks W6.

## W6 — Windows telemetry & the detection you must beat
Prereq: W5. Finish: `24-119` ETW consumer (build the blue telemetry yourself) + `12-03` AMSI/ETW evasion + `25-04..08` privesc (token model, mimikatz, potato, service misconfig) + `24-45` registry parser.
**Gate artifact:** `labs/windows-detection.md` — your W5 exploit activity captured by your own ETW consumer, then a mitigation that evades it, then a rule that catches that — before/after table. → unlocks W7.

## W7 — malware dev & C2 (the offensive product layer)
Prereq: W6 (absolute beginners: three floors first — drivers [`DRIVER-COURSE.md`](../DRIVER-COURSE.md) V0–V12, the watcher-builders [`EDR-COURSE.md`](../EDR-COURSE.md) T0–T12, then the malware/C2 twin [`WINDOWS-MALWARE-COURSE.md`](../WINDOWS-MALWARE-COURSE.md) F1–F5+M1–M12 — attacker and defender built by the same hands). Finish: `12-01` PE/API → `12-02` injection/persistence → `12-03` evasion → `12-04` C2 implant → `12-05` detection → `12-06..10` sektor7/anti-forensics/mini-loader/own-packer builders + `27-01..06` full C2 (wire protocol, agent, teamserver, transports, capstone).
**Gate artifact:** `labs/implant-c2.md` — your implant under your C2, full op on own VMs (deploy→persist→exfil→kill-switch), caught by your own YARA/Sigma+EDR (21-03/21-06), fixed, re-run. → unlocks W8.

## W8 — rootkit/bootkit + the full-engagement capstone
Prereq: W7. Finish: `18-01..07` rootkit/bootkit across Linux/Windows/macOS + `09-05` own exploit chain + `21-01/02/05` DFIR/IR-frame.
**Gate artifact:** `labs/engagement.md` — one full desktop engagement (own VMs): kernel/rootkit work from W1–W4 → implant (W7) → detection fight → disclosure-grade report with patch timeline. Pass = tutorials closed.

---

## Rules
1. Same no-skip artifact contract; runnable evidence; own VMs only (Windows = eval VM; macOS = your Mac/VM; Linux = QEMU).
2. Unix breadth: the Linux lane (W1–W2) is the Unix spine — FreeBSD/`04-09` + `24-33`/`24-99` parsers give the other-Unix perspective; Android root (Lane B) IS the Linux-kernel-on-ARM proof.
3. Timeline (not packed): W1–W2 ≈ months 2–6 (interleaves G0-end), W3–W4 ≈ months 5–8, W5–W6 ≈ months 7–10, W7–W8 ≈ months 9–12 — mobile lanes interleave when hardware/time allows.
4. Realistic bar: self-written 0-day kernel exploitation is career research; this path's bar = re-derived primitives + your own driver/module/rootkit artifacts + disclosure-grade writeups — the hiring/credible-researcher standard.

## Where this lives
`steps/` unchanged (route: 05, 04, 14, 24, 25, 11, 12, 27, 18, 19, 21). Three docs now form the complete offensive curriculum: `JAILBREAK-ROOT-PATH.md` (mobile kernel), `MOBILE-OFFENSIVE-PATH.md` (mobile surface), this doc (desktop kernel+malware+C2).