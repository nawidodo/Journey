# EDR Build Course — Absolute-Beginner (hello world → your own mini-EDR on Linux, macOS, Windows, gated)

Zero EDR knowledge assumed. You need: the Linux and Windows VMs from your other courses (or any), and your Mac. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/tN-quiz.md`)** — no advance without both. ~2–3h/unit, 12 units + capstone ≈ 7–9 weeks. This is the defensive product built from parts the repo already hands you: the platform courses taught you the attacker; now you build the watcher. Natural feeder peer of [`DESKTOP-OFFENSIVE-PATH.md`](DESKTOP-OFFENSIVE-PATH.md) W7 and `21-06` mini-EDR deepened to three OSes.

Compass (re-read when lost): EDR = **collect telemetry (process/file/network/registry-mach) → normalize into one event model → detect with rules → respond (kill/quarantine) — then lose the duel on purpose to learn** (T11). Every unit is one stage of that pipeline on one OS; the capstone wires all three.

Safety: everything against your own VMs/Mac; macOS EndpointSecurity needs an entitlement your dev identity may lack — the fallback (`log stream` + ps-diff) is a documented, honest plan B for T5/T6; the "attacks" you detect are your own lab agents; never point this at machines you don't own.

---

## T0 — hello world EDR: the process watcher (Linux)
Concept: at minimum an EDR sees processes live. Do: in your Linux VM: a Python/C watcher that polls `/proc` every second, diffs, prints NEW process (pid, ppid, cmdline, user, timestamp); run it, spawn a `sleep`/editor, watch it appear; then make it JSONL events to a file.
Verify: your watcher logs a spawn you caused, in JSONL.
**Lesson check:** why is process telemetry "the spine" of EDR — and what can't a 1-second poll see?

## T1 — the event model: normalize before you detect
Concept: detection needs one shape: time, pid, ppid, user, image, command line, flags. Do: design your JSON schema (T0 event + file/network extensions now); write the normalizer function; ingest a batch: log a week's worth of jobs (your own script loop) and query it ("which commands ran with sudo?") with a tiny query helper.
Verify: your batch query returns correct answers; schema documented in `notes/t1.md`.
**Lesson check:** why do EDRs normalize to a common schema instead of storing raw OS strings — and what's the cost of a wrong `cmdline` field?

## T3 — eBPF: see the kernel's view (Linux)
Concept: eBPF runs your code in the kernel where events can't hide; userland polls (T0) misses, eBPF watches syscalls. Do: bcc/libbpf small program attaching `tracepoint:syscalls/sys_enter_execve`, printing every exec with args; run it, exec a few commands, watch; then add a filter (only log non-root).
Verify: exec events logged by your eBPF program, filtered correctly.
**Lesson check:** what does eBPF see that /proc polling cannot — and what's the kernel-side risk of running eBPF at all?

## T4 — detection: Sigma/YARA rules that fire on you
Concept: telemetry alone is a log; rules make it detection. Do: from 21-03: write 3 Sigma rules (suspicious curl|sh from bash, `whoami` chained with download, LD_PRELOAD usage), write the matching engine over your T1/T3 event stream in your own code; plant those exact behaviors in your lab, watch your engine alert.
Verify: your rules fire on planted behaviors; alerts carry event context (rule, event, time).
**Lesson check:** detection truth table — true positive / false positive / false negative — with one example of each from your lab run.

## T5 — macOS collector (with an honest fallback)
Concept: macOS telemetry = EndpointSecurity (the real EDR pipe) or log stream (the democratic fallback). Do: plan A: a tiny ES client (C/Swift, `es_subscribe` exec/fork/exit) with your dev identity — if the entitlement blocks you, convert the lab to plan B: `log stream --predicate 'process == "your-agent"'` captured + a ps-diff layer; either way, emit your T1 schema for Mac events.
Verify: your mac collector emits exec events in the T1 schema (either path), run your T4 rules over them.
**Lesson check:** what makes EndpointSecurity better than log stream — and why does Apple make that pipe gated?

## T6 — macOS detection: the injection catch
Concept: EDR value is catching techniques, not named malware. Do: on your Mac: run your Windows-course trick adapted — `DYLD_INSERT_LIBRARIES` on a test app — have your T5 collector capture the exec/process env; write the rule "env var DYLD_INSERT_LIBRARIES on non-dev binary" → alert fires.
Verify: your rule alerts on the injection attempt on YOUR Mac, with the env captured in the event.
**Lesson check:** why do EDRs hunt techniques (injection, persistence patterns) rather than hashes — and what's a technique signature vs a malware signature?

## T7 — Windows collector: ETW (you already built it)
Concept: Windows answers for telemetry = ETW; kernel + ThreatIntelligence providers carry the truth. Do: reuse 24-119: your ETW consumer catching process creation (Microsoft-Windows-Kernel-Process or Sysmon-class events) + one file/registry provider; emit T1 schema; verify with your own runs (start `calc`/PowerShell from the lab).
Verify: your Windows collector logs process starts in the T1 schema; T4 rule engine runs over them.
**Lesson check:** ETW vs EventLog — why did you choose an ETW consumer, and what does the Threat-Intelligence provider add?

## T8 — Windows detection: the loader duel
Concept: now you detect the attacker's artifact (yours). Do: run your M1 loader (from WINDOWS-MALWARE-COURSE) or a calculator-spawn in the VM; write the rule that catches it (new process spawning from a low-integrity source + commandline anomaly); tune until true-positive without false-positives on a clean boot.
Verify: rule fires on the loader, silent on clean baseline (before/after table).
**Lesson check:** walk your own least-trust chain — which event field was the load-bearing one, and what would an attacker change to dodge it?

## T9 — the cross-platform agent: one engine, three feeds
Concept: a product, not three toys: shared schema, config-driven rules, updatable engine. Do: refactor: one rule-engine binary reads events from Linux eBPF collector, macOS collector, Windows collector (three feeds, same schema + a `os` field); config file declares rules + response actions; agents report to a small server (or file sink) with heartbeat.
Verify: one engine, three OSes: a planted `whoami`+curl on each OS alerts through the same pipeline, logs aggregated.
**Lesson check:** where does cross-platform normalization break (cmdline quoting, env, paths) — and what's the "os field" for in detection logic?

## T10 — response: the last stage
Concept: detect is half; respond = kill process (and quarantine the file carefully). Do: per-OS response: Linux kill+`chattr +i`-style quarantine copy; macOS `kill` + launchd blocklist note; Windows `taskkill` + ACL-denied quarantine dir for the artifact; wire your rules to an automatic action, gated by a dry-run flag (sandbox first!).
Verify: planted artifact killed by your engine per-OS, with the dry-run false tested first and logs kept.
**Lesson check:** why is automated response the riskiest stage — and what did the dry-run flag protect?

## T11 — the duel: your EDR vs your skills
Concept: the loop that makes defenders better: attack, detect, respond, iterate. Do: use the Linux loader/persistence from your Windows-malware knowledge (port it) against your own mini-EDR: first run — does it catch it? Then iterate (your choice: better rule, response, or telemetry) until caught; then evade again (swap technique), then catch again. Log every round.
Verify: 3 dueling rounds documented with rounds table (technique → caught/not → fix).
**Lesson check:** after the duel, name the three weakest seams of your EDR — and the one you can ship fastest.

## T12 — CAPSTONE: one EDR, three OSes, cold
Prereq: T0–T11. **Close all notes.** Fresh rebuild of the full pipeline (collect → normalize → detect → respond) on all three OSes, then: plant one technique per OS (yours — loader-ish, injection-ish, persistence-ish) and catch all three through the single engine. Write `labs/edr-capstone.md`: architecture diagram, the three catches with evidence, your honesty-gap list (what a real EDR has that you don't: AV-engines, cloud, kernel lockdown), and three things you'd add next.
**Pass = all three OSes alert through one engine without notes; the gap list is honest.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words — defensive skill is measured in precision, be honest about false positives.
2. Copying allowed only in T0/T7 boilerplate (event-consumer shell, bcc skeleton) — engines and rules written by you; erase-and-retry once when stuck.
3. 2–3h/unit; stuck past that = previous unit's verification again.
4. VMs/macOS only; response actions in dry-run first; the attacks you detect are your own lab artifacts; never run against machines you don't own.
5. Honest bar: real EDRs are company-years with AV cores and cloud SIEM; this course's bar = a working collect-normalize-detect-respond pipeline of YOUR code on three OSes that catches YOUR artifacts — the floor for detection-engineering, DFIR-product, and purple-team roles, proven cold at the capstone.

## Where this lives
`steps/` unchanged (route: 21-01..10, 24-103, 24-119, 24-105, 24-79). Pairs the offensive courses: every attacker technique from your platform courses becomes a T4/T6/T8 rule — the loop closes.

Input-layer sibling — transparent listener + detector pair per OS (no stealth, consent-first): [`INPUT-SECURITY-COURSE.md`](INPUT-SECURITY-COURSE.md) IS0–IS10.
