# Behavior Detection Course — Absolute-Beginner (hello world → YOUR OWN "suspicious activity" detector: whoami/ping-class signals and the sequences that mean something, gated)

Zero behavioral-detection knowledge assumed — but you carry the organs: EDR-COURSE (process telemetry + detection), XDR-COURSE (pipeline + alert lifecycle), DETECTION-COURSE (signal craft), INSTRUMENT (event capture). This course specializes: detecting SUSPICIOUS USER BEHAVIOR at the command line — the classic signals (whoami before privilege escalation, ping as C2 connectivity checks, recon command sequences) — as a SOC-style detector running on YOUR OWN lab users/sessions. The course is fundamentally about INTENT INFERENCE: the same `ping` is maintenance on Tuesday and recon on Thursday — your job is the context, the sequence, and the honest humility about what you can't know. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/bdN-quiz.md`)** — no advance without both. ~2-3h/unit, 11 units + capstone ≈ 5-7 weeks.

Compass (re-read when lost): commands are the verbs of a session: every action a user (or malware in their context) takes runs through exec. Detection here = watching the verb stream and asking: who is acting, what class of action is it, what came before it, and does that violate the baseline? The ladder: capture exec events (your collector) → classify commands (whoami/ping/curl/... → technique classes) → find SEQUENCES (whoami→ping→exfil-shape) → add context (user/host/baseline) → alert with severity. What you build is a policy-scoped "know your lab" watcher — the same engine behind EDR behavioral detections, UEBA, and insider-threat program telemetry.

**Monitoring ethics (the course constitution, unit BD0):** activity monitoring is power — real deployments require policy, notice, and consent regimes (SOC mandate, employment policy, user awareness). This course: OWN LAB users/sessions only (usually your own shell), simulated users you define, no real third-party traffic, no real-world deployment; the "why monitoring legality differs by jurisdiction" reading is required, not optional.

---

## BD0 — hello world: verbs, classes, sequences
Concept: suspicious = context, not command: same verbs, different intents; technique classes (MITRE: T1016 ping/system-recon, T1033 whoami/user-enum, T1059 cmd-exec, T1071 C2-check). Do: the verb-taxonomy table (command → class → typical-intent → benign-counter-example); map to MITRE ATT&CK techniques (read the technique pages); hello: a scripted session log of YOUR OWN commands (whoami, ping, ls, curl) — the raw material your detector will eat; write the monitoring-ethics preamble (above).
Verify: taxonomy + MITRE map written; session log captured; ethics preamble signed.
**Lesson check:** why is `ping` both the most benign and most useful command to watch — and what does "intent" require beyond the verb?

## BD1 — capture: exec events, your own collector
Concept: exec telemetry: who, what, when, from where: Linux (eBPF tracepoint/auditd — reading + own eBPF from your tooling), Windows (ETW/Sysmon EID 1), macOS (Endpoint-Security reading). Do: your Linux collector: exec events (cmdline, user, uid, pid, ppid, cwd, session id, time) emitted in your BD-schema; a session-labeler (tty/session map so you can attribute to "who's laptop"); verify against your BD0 log: every command appears with full fields.
Verify: collector replays your session exactly (fields complete); session attribution correct.
**Lesson check:** what fields beyond cmdline make attribution possible (cwd? ppid? session?) — and why does eBPF/ETW beat polling for this (what's invisible to a poller)?

## BD2 — the classifier: verbs → technique classes
Concept: classify each exec: recon, user-enum, conn-check, filestage, credential-touch, harmless; rules YOU write (regex/argv parse + canonicalization: `ping -c 4 host` → conn-check). Do: the classifier module (classification rules with evidence); canonicalization (flags/aliases → canonical verb); the test battery: 20 planted commands across classes — 100% correct class; the "unknown command" bucket (honest: what you can't classify defaults to?).
Verify: battery correct; unknown-bucket default documented.
**Lesson check:** why canonicalize before classify (what breaks without it) — and why must "unknown" be a real bucket and not a silent pass?

## BD3 — the sequence engine: whoami→ping→ls /tmp
Concept: one command rarely suffices; SEQUENCES signal (recon walks: enumerate-user → check-connectivity → peek-directories); stateful session tracking. Do: the session tracker (per user/session: running window of classified events), sequence rules on the window (patterns: user-enum→conn-check→filestage within 60s), planted sequence demo: your own 3-step recon fires; the "benign sandwich" test: legitimate maintenance flow (same verbs, different order) stays silent.
Verify: planted sequence fires; benign sandwich silent; window/rule config documented.
**Lesson check:** what does the WINDOW decide that the rule can't — and why is order-as-well-as-membership the signal (what makes a sequence a sequence)?

## BD4 — context: whose normal is it?
Concept: baseline per user/role: their toolset, hours, hosts; anomaly = deviation from THEIR normal, not global rules. Do: the behavior profile builder (per-user: command-class histogram, hour distribution, host affinity — fed by YOUR OWN sessions); the comparer (deviation score per event: class unseen-for-user, never-seen-host, hour-novel); test: your normal day — profile learns it; a planted novel action (a class your profile never shows) → flagged.
Verify: profile learns; novel-action flagged; "profile warm-up cost" documented (how long before trust).
**Lesson check:** why does per-user baseline beat global rules for BEHAVIOR — and what's the cold-start problem (who do you trust while the profile warms up)?

## BD5 — the alert: severity, dedup, case stub
Concept: alerting that humans can use: severity = class × context × sequence-position, dedup/suppression (known-good lab builds), case stub (XDR-style artifact). Do: the alert model (scoring formula YOUR design), dedup (same signature within window → consolidate), suppression list (lab's own build/update flows), case stub output (alert + evidence events + user context + suggestion); the 3-alert demo: one real signal, one benign-but-noisy, one suppressed.
Verify: 3-alert demo behaves (signal alert-rich, noisy deduped, known-good suppressed).
**Lesson check:** what makes an alert ACTIONABLE vs spam (evidence? context? suggestion?) — and why is suppression a FEATURE and not a cheat (what does it protect)?

## BD6 — the humility unit: ping is benign 99.999%
Concept: the math of suspicion: base rates mean most alerts are false; the FP discipline IS the skill. Do: the false-positive measurement: run your detector over a week of YOUR OWN normal sessions → FP rate computed per class (the report card); the essay (in your words): "why 'suspicious' is a likelihood, not a verdict" — the cost matrix (a missed real signal vs a busy alert queue); the tuning pass: adjust one rule to cut FP 50% with zero lost real detections (demonstrated).
Verify: FP report card written; tuning pass measured; essay written.
**Lesson check:** what does the COST MATRIX do that accuracy numbers can't — and when is a quieter detector BETTER (what's the actual product metric)?

## BD7 — the OS map: Windows, macOS, mobile
Concept: same detection, other platforms: Windows (ETW process-creation + PowerShell/CMD script-block logging — reading "what YOU can log"), macOS (Endpoint Security framework reading + its entitlement wall — honest), Android/iOS (process visibility is OS-gated — what mobile vendors expose to EMM/MDM and the ceiling). Do: the mapping unit: for each OS: capture source, what's visible, what's hidden; the "what a real cross-OS fleet needs" note (agent + OS-log integration); a Windows-VM demo if reachable: Sysmon EID 1 collector (or ETW reader from your EDR work) replaying your battery.
Verify: OS map complete; Windows demo (or honest wall) done.
**Lesson check:** why is Linux eBPF the GOLDEN goose for exec telemetry (what do the locked OSes hide) — and what does that asymmetry mean for detection coverage?

## BD8 — evasion reality: encoded and renamed
Concept: adversaries don't type `whoami`: PowerShell -enc, base64 payloads, renamed binaries, lolbas (living-off-the-land). Do: the evasion-aware rules: flag ENCODED/obfuscated exec (arg-shape heuristics: base64-length strings, -enc, /c big-blobs), renamed-binaries (hash/sig lookup for known tools — hash your own tools), the "intent in the SHAPE" essay — behavior vs name; the boundary note: this is detection of suspicious SHAPES, never a permission to bypass EDRs (that's the DT8 line you signed).
Verify: encoded/renamed plants fire; essay written.
**Lesson check:** what does the SHAPE of an argument reveal that the verb can't — and why does shape-detection outlive name-blocking (what do name-blocks miss)?

## BD9 — the product: watch-cli, live shift
Concept: the usable form: live activity stream, per-user status, alert queue, policy file — a shift tool. Do: `watch-cli` (your name): `watch` (live stream + class colors), `alerts` (queue + severity), `user <name>` (profile view), `policy` (rules file format + edit + reload); the live shift: run 20 minutes of YOUR simulated sessions (your own scripted commands incl. one real signal) — the shift log written like a SOC notebook.
Verify: watch-cli runs the shift; the signal appears in the alert queue; notebook written.
**Lesson check:** what does an operator need beyond the alert (the shift notebook — what's in it) — and why is the POLICY FILE the product's soul (who changes rules, how audited)?

## BD10 — the real world: ATT&CK, UEBA, and the law
Concept: your rules' professional home: MITRE ATT&CK coverage mapping, UEBA products (reading: what they claim vs measure), and the legal/consent reality of activity monitoring (jurisdiction differences — mandatory reading). Do: map your rules to ATT&CK techniques with coverage table; UBA/UEBA product architecture reading (entity modeling, peer-group, anomaly engines — what they do that yours doesn't); the monitoring-law précis (GDPR/SOP/employment-law flavors — reading + your worked example for a fictional org).
Verify: coverage table done; UEBA comparison written; law précis + worked example done.
**Lesson check:** what does UEBA scale-up add that your per-user profiles hint at (peer groups! entity graph) — and why does LAW matter more than accuracy in deployment (what happens without the consent layer)?

## BD11 — CAPSTONE: the shift, cold
Prereq: BD0–BD10. **Close all notes.** Cold: rebuild collector + classifier + sequence + alert in one sitting (no notes), then run a live shift: script a 15-minute simulated session with TWO planted signals (a recon sequence + an encoded exec) — catch both, dedup the noise, print the alerts as case stubs. Write `labs/behavior-capstone.md`: architecture, rule set, FP note, the shift log, the BD6 essay, three proud decisions, regret, roadmap (peer-group analytics, ML, cross-host session correlation).
**Pass = the cold rebuild catches both planted signals in the live shift; the shift log and essay carry the file.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in BD0/BD1 boilerplate (exec-capture scaffold, session map) — classifier, sequences, profiles, alerts, watch-cli written by you; erase-and-retry once when stuck.
3. 2-3h/unit; stuck past that = previous unit's verification again.
4. **Monitoring ethics: own lab sessions only, simulated users you define, no real deployment, consent/policy reading required; detection never authorization (no user-surveillance product framing).**
5. Honest bar: UEBA and XDR behavioral detections are vendor-scale systems with compliance layers; this course's bar = a working exec-verb watcher (classify → sequence → context → alert), FP-measured, ethically fenced, proven cold at the capstone — the floor for behavioral-detection engineering and the complete answer to "is this session doing something interesting".

## Where this lives
The specialization of EDR (process events) + XDR (pipeline/alerts) + DETECTION (signal craft) — same family, narrower scope, deeper intent-inference; MITRE ATT&CK links your TECHNIQUE tables from CVE-STUDY; the consent layer is the same one INTEGRITY/XDR already signed.