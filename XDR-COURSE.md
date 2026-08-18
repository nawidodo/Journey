# XDR Course — Absolute-Beginner (hello world → YOUR OWN cross-source detection-and-response platform, gated)

Zero SIEM/XDR knowledge assumed — but you carry the whole stack: EDR-COURSE (endpoint detection), DETECTION-COURSE (debugger/hook signals), NETEXEC+PROXY-MITM (network/traffic eyes), DATABASE-course (event storage), INSTRUMENT (telemetry internals), plus every lab discipline. XDR = Extended Detection and Response: collect telemetry from MANY sources (endpoint, network, email, cloud, identity), correlate across them, and respond — the product tier above EDR. This course assembles YOUR OWN `xdr-lite`: two-plus collectors → normalized pipeline → correlation engine → response playbook → SOC lifecycle (triage, alert, case) — all on your own lab, with safety rails hard-wired. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/xdN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 9-11 weeks.

Compass (re-read when lost): EDR saw ONE host; XDR sees MANY and connects: "process on host A opened a socket to host B's port X, which resolved to a domain your DNS log flags" — the correlation IS the product. The ladder: telemetry sources (each your own collector) → pipeline (normalize/enrich/store) → detection (rules + correlation + baselines) → response (containment with safety) → people (triage and the case file). You've already built most of the organs separately; this course wires them into a body and runs a real incident through it.

**Safety hardline (unit XD0): response = power** — automation can destroy a lab or a network. This course's response actions: kill/block/isolate run in DRY-RUN first, allowlist-protected, own-lab targets only, every action logged + reversible; the course's incident simulations attack YOUR OWN lab hosts with YOUR OWN tools (the NETEXEC/PROXY/INSTRUMENT armory you built, used blue-side). No real network, no real victims, no persistence in response logic.

---

## XD0 — hello world: the detection stack and XDR's place
Concept: EDR → XDR → SIEM → SOAR: layers, feeds, and the "correlation across sources" that defines XDR. Do: the stack map (tier × function × product-example × your equivalent); inventory your existing organs: EDR-collector, traffic eyes, event store, detection rules — as YOUR future modules; the hello: one endpoint event + one network event from your own lab, joined by a shared process/asset key in a scratch pipeline (a notebook or tiny script proving the join concept).
Verify: stack map written; your module inventory listed; the two-source join demoed.
**Lesson check:** what exactly does "extended" buy over EDR — and why is the DATA MODEL (the join key) the real product?

## XD1 — telemetry core: the event schema
Concept: telemetry = facts: process, network, file, user, host — one schema to rule them. Do: design YOUR event schema (JSON: uuid, ts, host, user, source_type, object/action/result pairs, asset tags); canonical JSON events for: process spawn, socket connect, file open, DNS query, module load; a schema version + validation (own validator); write the "schema is the contract" note (what happens when collectors disagree).
Verify: schema validates your 5 canned events; version bump demonstrated (breaking change → validator catches).
**Lesson check:** why is ONE normalized schema the make-or-break of correlation — and what do real products fight here (field-name sprawl, type drift)?

## XD2 — collectors: your eyes per source
Concept: collectors translate host/network reality into schema events: endpoint (process/network/file via your EDR/INSTRUMENT machinery), network (flow/DNS/packet metadata via packet-capture skills), email/cloud (reading + lab option). Do: build TWO collectors from your existing skills: endpoint collector (reuse/migrate your EDR-course collector emitting your schema) + network collector (flow + DNS logs from your PROXY/NETEXEC lab traffic); both feeding one local sink (file/JSONL).
Verify: both collectors stream schema-valid events for a scripted lab session (start a process, open a socket, resolve a name — all appear).
**Lesson check:** what does a collector LOSE between reality and schema (who decides what's worth keeping — and what's the cost of keeping everything)?

## XD3 — pipeline: normalize, enrich, store, query
Concept: raw → clean → enriched (assset/user context) → stored (queryable) with a search API. Do: pipeline stages in your own code: normalizer (schema-validate + type-coerce), enricher (asset/user/host-lookup tables — your own), store (your DATABASE-course TSDB/file-store or own rolling log store), query API (time-range + filters + counts, your search syntax); feed hours of collected events; the pipeline-stats report (per-stage drop/error rates — honest).
Verify: query API finds a known event by (host, action, time); pipeline stats report written.
**Lesson check:** where do events get DROPPED in a real pipeline (the three silent killers) — and why is query design as important as collection?

## XD4 — detection: rules, correlation, baselines
Concept: detection = patterns over the stream: Sigma-class rules (from EDR), cross-source correlation (process↔network↔file joins), behavioral baselines (what's NORMAL for your lab — anomaly = deviation). Do: your rule engine (rules in your format: match → alert) with 5 rules (e.g., "process spawned whose parent never spawns", "host dialed flagged domain", "file modified outside known set"); ONE cross-source correlation rule (same key across two collectors fires only jointly); a baseline profiler (normal-activity model for your lab; deviation alert); test battery: your lab's "attacker" tools (NETEXEC-scan, PROXY-mitm, INSTRUMENT attach) each fire ≥1 rule.
Verify: battery fires rule-set with alerts; correlation rule needs BOTH sources (proved by disabling one); baseline flags an anomaly you introduced.
**Lesson check:** what do RULES miss that CORRELATION catches (and vice versa) — and why does EVERY rule engine drown in false positives eventually (what's the real skill)?

## XD5 — response: contained power
Concept: response actions (kill/block/isolate) automated with safety: dry-run, allowlist, audit, reversibility. Do: your response module: actions (process-kill, firewall-block by host, isolate-via-VPN-teardown on your lab) behind the safety rail (dry-run flag, allowlist of lab assets, every action = audited event + auto-revert where possible); one playbook (detected-NETEXEC-scan → block source IP → notify → report); the tabletop: run your playbook against your OWN lab scanner; the "why response automation terrifies vendors" essay.
Verify: playbook runs dry-run then armed on the lab; audit trail complete; essay written.
**Lesson check:** what is the actual risk of automated response (what breaks first under automation) — and why do real products still ship it (what does speed buy)?

## XD6 — SOC layer: alert lifecycle, triage, the human loop
Concept: alerts → tickets → triage → investigation → case: the peopleware that makes XDR useful; IR-shift simulation. Do: alert lifecycle model (new → triaged → investigating → resolved/false-positive; required fields + SLA timers); triage queue (priority scoring: rule severity × asset value × novelty); case file format (events pinned, notes, timeline, verdict); A tabletop IR shift: inject 3 alerts from your lab's own incidents (one true positive, one benign, one ambiguous) — walk each through the lifecycle with written notes.
Verify: lifecycle + queue + case demoed on the 3-alert shift; SLA timer behavior shown.
**Lesson check:** why does the HUMAN loop decide whether detection engineering succeeds — and what makes a case file evidence-grade vs a to-do list?

## XD7 — threat intel: feeds with a pinch of salt
Concept: IOCs (hashes, domains, IPs) ingested + matched + curated; intel is a LAGGING signal. Do: your intel module: IOC store (own format), match rules against your event stream, confidence + source attribution per IOC, curation workflow (IOC lifecycle: new→confirmed→expired); the "why intel lags (and why it still matters)" essay — with a real example (a known-bad domain from your own reading); the false-positive discipline: a curated IOC that MUTED a rule when noisy.
Verify: IOC match fires on a planted event; curation lifecycle walked; essay written.
**Lesson check:** what is intel's fundamental weakness (freshness? coverage? fraud?) — and what does good curation do that bulk feeds don't?

## XD8 — the network X: deep traffic eyes
Concept: beyond flow: DNS analytics, TLS metadata (from PROXY-MITM's plaintext view!), certificate anomalies, the "beaconing" shape. Do: extend your network collector: DNS query log → analyst view (rare domains, NXDOMAIN storms), TLS SNI/证书 metadata capture (your PROXY-MITM machinery, metadata only), beaconing detector (periodic-talker algorithm from your packet-capture skills); test battery: your lab's C2-simulator-ish tool (from C2-course) triggers beaconing + SNI anomaly.
Verify: all three network lens fire on the lab; analyst views demonstrable.
**Lesson check:** what does the network layer see that endpoints NEVER will (and the reverse) — and why is TLS metadata the modern battleground (what's encrypted vs what leaks)?

## XD9 — beyond the laptop: mobile and cloud
Concept: XDR's reach: mobile telemetry (app risk signals, MDM reading), cloud logs (AWS CloudTrail/GCP/entra reading + your own mini-cloud-lab), identity events. Do: the extend unit: mobile collector (android app event emission — schema events from an app you write; iOS ceiling honest), cloud reading (CloudTrail-grade log format analysis, detection ideas), identity event (login-failure analytics from your own lab services); the "XDR scope creep" essay — what products claim vs what their telemetry actually covers.
Verify: mobile app emits schema events into pipeline; cloud-format reading notes; identity analytics demoed.
**Lesson check:** why is cloud/mobile telemetry more fragmented than endpoint (who owns each log) — and what does scope creep do to an XDR's actual efficacy?

## XD10 — protecting the protector
Concept: your XDR is a target: pipeline integrity, sensor tamper detection, response-module hijack — the SOURCE ALL section. Do: harden your own build: pipeline checksum/audit (INTEGRITY skills), sensors self-report health (heartbeat + integrity hash), DETECTION-suite running against the XDR's own processes (who touches my collector?), supply-chain hygiene note (your deps vetted); test battery: tamper your own collector → health check flags + alert.
Verify: health+integrity battery passes (tamper detected); hardening writeup done.
**Lesson check:** why does an attacker target the XDR itself FIRST — and what does sensor-health give you beyond trust (what's the silent failure mode without it)?

## XD11 — the real products, read honestly
Concept: CrowdStrike/SentinelOne/Defender/Elastic: architecture reading — where they agree and where "XDR" is marketing. Do: reading: 2-3 architecture docs/papers on major XDRs; the comparison table (their sources, data model, correlation engine shape vs yours); the "XDR hype vs reality" essay — the unstated truth (data quality > tool count; nobody's XDR covers everything it claims); three ideas you'd steal with attribution.
Verify: comparison table + essay + steal-list completed.
**Lesson check:** what separates marketing-XDR from engineering-XDR — and what did the majors reveal about the ONE thing all of them still struggle with (probably data quality — argue it)?

## XD12 — CAPSTONE: xdr-lite, the live incident, cold
Prereq: XD0–XD11. **Close all notes.** Cold: stand up xdr-lite from memory: endpoint+network collectors → pipeline → 3 rules + 1 correlation → response playbook (dry-run first) — then run a scripted lab incident (your own tools simulate attacker), catch it, triage it, case it. Write `labs/xdr-capstone.md`: architecture diagram, schema, alert + case artifacts, the XD11 essay, three proud decisions, regret, roadmap (more sources, ML correlation, agent fleet management).
**Pass = the cold-built xdr-lite catches the live lab incident end-to-end (collect → detect → respond → case) with the essay standing.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in XD0/XD3 boilerplate (schema scaffold, storage bootstrap) — collectors, rules, response, lifecycle, suite written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Safety: response = power — dry-run-first, allowlist, own-lab targets, audited + reversible; attacks simulated against YOUR OWN hosts with YOUR OWN tools; no real networks/victims.**
5. Honest bar: real XDRs are multi-billion-dollar fleets with armies of detection engineers; this course's bar = a working multi-source detect-and-respond platform, correlation real, response safe, lifecycle real, proven cold at the capstone — the floor for detection-engineering leadership and the complete anatomy of every "was I breached" question.

## Where this lives
Assembles organs you already built: EDR-COURSE collector+detection, NETEXEC/PROXY-MITM network eyes, DATABASE-course store, DETECTION+INTEGRITY self-defense, INSTRUMENT/C2 lab armory (used blue-side). One repo, two dozen courses, now one body.