# SIEM Course — Absolute-Beginner (hello world → YOUR OWN log-collecting, correlating, alerting SIEM, gated)

Zero SIEM knowledge assumed — you bring pipeline/correlation muscles from XDR XD0–XD6, query/event-store instincts from DATABASE-course, and the SOC discipline from BD (triage/alert lifecycle). SIEM = Security Information & Event Management: ingest LOGS (syslog, Windows events, app logs — the boring gold of an IT floor), normalize them, index them, correlate across them, and alert — plus the compliance/audit layer (why companies BUY SIEMs). XDR built agent telemetry on your own hosts; SIEM is the log-swallowing big sibling that eats EVERYTHING a network emits. Mastery ladder: collect (listeners/parsers) → normalize (grok-lite field mapping) → index+query (your search language) → correlate+alert (time-window rules) → dashboard+report (the evidence layer) — ending in the compliance truth: SIEMs exist because "prove you watched" is a business requirement. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/siN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): SIEM's bet: security lives in the LOG STREAM. Your job: make logs searchable in seconds, correlated across days, and defensible in audits. The LADDER: intake (syslog listener + file tails) → parse (log formats → fields) → store (rolling shards + inverted index) → query (field:value syntax) → rules (time-window correlation) → alert (dedup/severity) → dashboard/report (the human + compliance face). Every log is noise until a schema turns it into a question you can ask.

Honesty notes (unit SI0): SIEMs are famous for "collect everything, drown in noise" — your course fights that with FP discipline from BD and honest storage limits from DATABASE-course; ingestion is real work: parsing correctness is THE engineering skill here; your lab = generated/simulated log sources (a fake-network log generator you write) + your real lab logs — no third-party production data, ever.

---

## SI0 — hello world: the SIEM anatomy and the compliance driver
Concept: collect → parse → index → search → correlate → alert → dashboard → report; why SIEMs exist (audit/compliance + investigation + detection at log scale). Do: the anatomy diagram (each stage + what it eats); the "why SIEM" note (PCI-DSS/SOC2-grade requirements — reading one compliance requirement list); hello: your fake-org log generator (auth events, firewall-ish allow/deny, app errors — with planted incident patterns) writing syslog-format lines to a file.
Verify: anatomy + compliance note written; log generator runs producing a week-ish dataset.
**Lesson check:** what does a SIEM do that an XDR agent never sees (where do their worlds overlap/diverge) — and why does "prove you watched" drive real budgets?

## SI1 — collection: listeners and tails
Concept: intake: syslog (UDP/TCP, RFC 3164/5424), file tail, Windows Event forwarding (WEC/WMI reading), API pulls. Do: your syslog listener (UDP+TCP ports, RFC 5424 header parse, forwarding rules), file-tail watcher (rotated-log-safe), Windows-lane as reading + optional VM demo (forwarded events read); the intake dashboard: source | rate | last-seen (ingestion health — the silent-killer table from XD3).
Verify: your generator streams into the listener; health view shows sources live; tail catches rotation.
**Lesson check:** what breaks silently in log collection (rotation, clock skew, truncated lines) — and why is INGESTION HEALTH the first tab an operator opens?

## SI2 — normalization: formats → fields
Concept: parse: Apache/Nginx, syslog, JSON, CSV, Windows Event XML — each to the SAME schema (ts, host, source, type, actor, action, target, result, detail). Do: your parser library: grok-lite (pattern dictionary + named captures — YOUR rules) for 4 formats; schema-mapper (per-source type → canonical fields); parser test suite (known-correct expectation per format — 100% match); the famous parse-errors reality: a field dropped is a fact lost — your error log.
Verify: 4-format battery passes; mapper emits schema-valid events; parse-error report honest.
**Lesson check:** why is normalization the SIEM's hardest 80% — and what does a MALFORMED line cost (who notices a dropped field)? — name one real incident class this hides.

## SI3 — store and query: your search language
Concept: rolling shard store (day-partitioned files) + inverted index (field→values list) + query syntax (field:value, AND/OR, ranges, count/group). Do: your store (write path: shard by day, append-only, index sidecar), query engine (parse your syntax → scan/seek + aggregate), query API; the perf reality: 100k+ events/day ingested and queryable — your bench numbers (honest, from XD bench discipline); retention: configurable (day-count purge) with the "what you keep vs what you lose" policy note.
Verify: queries find/filter/group correct answers on the week-dataset; bench table recorded.
**Lesson check:** what does an inverted index buy you vs full scan — and why is RETENTION a business decision (compliance minimum vs storage cost) more than an engineering one?

## SI4 — correlation: time-window rules that mean something
Concept: single events are noise; time-window rules: "5 failed logins same user in 1 min", "auth-denied then port-scan from same IP", "app-error spike". Do: your rule engine (rule format: trigger-window + condition + aggregation + throttle); a rule set (5: fail-login burst, scan-then-deny, error-spike, never-seen-source, combo); the planted-incident test: your generator's patterns FIRE exactly their rules; the FP test: normal background noise fires zero (from BD discipline).
Verify: planted patterns fire; background silent; rules' throttle behavior demonstrated.
**Lesson check:** why do SIEM rules tune the WINDOW/Aggregation more than the condition — and what is the #1 SIEM failure mode (and how does your throttling address it)?

## SI5 — alerting: severity, dedup, delivery
Concept: alerts with legs: severity scoring, dedup (same incident → one thread), notification (console + your own webhook/email-lite), escalation to case. Do: the alert manager (from XD5/XS schema): pipeline alert → severity by rule+asset → dedup window → channels (console log + a webhook to a tiny receiver you run); the live proof: one planted event fires exactly one deduped alert thread with evidence list.
Verify: alert thread correct (single, evidence-attached); webhook received; severity order sane.
**Lesson check:** what does a DEDUPED, evidence-attached alert do that a raw notification can't — and why is alert-fatigue a PROCESS failure, not a tool failure?

## SI6 — dashboards: questions become pictures
Concept: the face of the SIEM: saved queries → widgets (auth-failures over time, top sources, error-rate, correlation hits) → reportable views. Do: your dashboard module: saved query list + widget renderer (ASCII/HTML-lite charts from your query aggregates — no chart lib unless proven necessary); build 6 widgets; the week-story: a generated dashboard page that SHOWS the planted incident's day (visual spike) annotated.
Verify: 6 widgets render real data from the store; the incident-day page tells the story visually.
**Lesson check:** what makes a dashboard a LIE (window size, aggregation hiding, cherry-picked range) — and what 3 widgets would you run first in a real SOC (argue)?

## SI7 — compliance and audit: the receipt that holds up
Concept: the business layer: append-only integrity (hash-chained event log — from crypto), retention proof (docs + config), report generator (evidence pack: coverage, alerts, resolutions). Do: hash-chain your ingested events (store integrity: each batch chained; tamper → detected — reuse INTEGRITY skill), the audit report generator (period summary: sources covered, alert count, open cases); a demo corruption test: flip a byte in a stored shard → chain check flags it.
Verify: chain check flags the tampered shard; report generator produces the evidence pack.
**Lesson check:** why do compliance auditors want INTEGRITY of logs more than clever detection — and what does hash-chaining prove that a filesystem timestamp can't?

## SI8 — scale-lite: throughput and the honest ceiling
Concept: SIEMs die at scale: parsing throughput, shard count, query latency curves; the honest numbers. Do: your bench: events/sec ingest (single-thread vs batched), query latency vs index size (the curve — it degrades; plot it), shard-partition strategy (day + source); the "vs a real SIEM" humility table (Elastic/Splunk-class: what they do that you don't — distributed, replication, hot-warm); verdict essay: where YOUR design would collapse first at 10⁶ events/day.
Verify: bench curves recorded; humility table + verdict written.
**Lesson check:** what scales poorly in YOUR design (find the real bottleneck with measurements) — and why do real SIEMs shard by TIME (what breaks without it)?

## SI9 — SOC workflow: from alert to case to lesson
Concept: alerts become work: triage decision tree, case lifecycle (from XD6), runbook links, after-action notes. Do: wire your alert manager to a case store (triage: true/benign/needs-work, fields: evidence, owner, notes, status); the 3-alert shift (from XD6 pattern): one real planted incident, one benign pattern, one ambiguous — full lifecycle walked; the after-action note (what would've caught it earlier — fed back as a rule change).
Verify: 3-alert shift completes with cases + one rule-change feedback loop demonstrated.
**Lesson check:** what separates a SIEM that catches things from one that just stores them (hint: it's not the parser) — and why feed findings BACK into rules?

## SI10 — the market, read: Splunk/QRadar/Elastic and the debate
Concept: SIEM history and controversy: the "SIEM is dead/long live SIEM" debate, XDR-convergence, modern log analytics. Do: reading: 2-3 architecture docs or postmortems on major SIEM deployments; the comparison table: their ingestion/query/correlation vs yours (honest gap list); the 300-word essay: "is SIEM outdated?" — your position (what still only a SIEM does; what XDR/behavior obsolesced); the "collect everything" cautionary tale (a real overload incident from reading, summarized). 
Verify: comparison + essay + tale summary written.
**Lesson check:** why do enterprises KEEP buying SIEMs despite the noise problem — and what does the XDR convergence tell you about where the market thinks detection lives now?

## SI11 — the product: siem-lite CLI
Concept: usable shape: ingest/query/alert/dashboard/report from ONE CLI with config files; a SOC-week simulation. Do: `siem-lite` (your name): `ingest <source>`, `query '<syntax>'`, `watch` (rule loop), `dashboard`, `report`, `health`; config files (sources, rules, retention); the simulation: generate ONE SOC-week of logs with THREE planted incidents at different days; run the week; your shift log catches all three with evidence.
Verify: CLI drives the week; three incidents surfaced with evidence; shift log written.
**Lesson check:** what makes a SIEM a PRODUCT and not a script set (config, health, report, docs) — and which two config files would a new operator need first?

## SI12 — CAPSTONE: the live floor, cold
Prereq: SI0–SI11. **Close all notes.** Cold: rebuild collection→parse→index→correlate→alert core in one sitting (no notes), then: ingest a FRESH generated week (unseen incidents), run the shift, catch the planted signals, generate the report. Write `labs/siem-capstone.md`: architecture, schema, rule set, bench numbers, the SI10 essay, three proud decisions, regret, roadmap (distributed shards, machine-learning correlation, cloud-native ingestion).
**Pass = the cold rebuild catches the unseen week's incidents end-to-end; the report and essay carry the file.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in SI0/SI1 boilerplate (syslog scaffold, file-tail) — parsers, index, query, rules, alerts, dashboards, chain written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Data discipline: generated/simulated sources + your own lab logs only; no real third-party production data; ingestion health is always on; hash-chain integrity on by default.**
5. Honest bar: Splunk/Elastic-class SIEMs are distributed, replicated, ML-questioned platforms; this course's bar = a working collect→parse→index→query→correlate→alert→report loop with honest bench, integrity, and discipline, proven cold at the capstone — the floor for log-analytics security engineering and the complete answer to "what happened at 03:00".

## Where this lives
The data-heavy sibling in the detection ladder (EDR → Behavior → XDR → SIEM): XDR XD0–XD6 gave pipeline/correlation; BD gave SOC/FP discipline; DATABASE-course gave store instincts; INTEGRITY gives the chain; CVE/KV gives the rule-worthy technique knowledge. Same family, one more altitude — and the compliance layer none of the others own.