# DLP Course — Absolute-Beginner (hello world → YOUR OWN data-loss-prevention system, endpoint + network + file, gated)

Zero DLP knowledge assumed — you bring the organs: PROXY-MITM (traffic eyes), USB-DRIVER/GAMEPAD + 22-embedded steps (device lanes), BD (monitoring ethics + FP discipline), XDR/SIEM (pipeline + case workflow), INTEGRITY (chain, verification). DLP = Data Loss Prevention: detect and stop sensitive data (PII, card numbers, secrets, documents) leaving your perimeter through email, uploads, USB, clipboard — and find it at rest in files. The course builds `dlp-lite`: content inspection core → policy engine (audit/quarantine/block) → three sensor lanes (clipboard/USB/network + file scan) → incident workflow → dashboards — all on YOUR OWN lab data with a privacy constitution up front. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/dlN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): DLP = content understanding + policy + enforcement: recognize WHAT is sensitive (patterns, dictionaries, scores), decide what policy applies (this channel, this class, this action), and ACT (audit silently, quarantine, block) at the right choke points. The three lanes mirror real products: endpoint (clipboard/USB/print), network (email/upload/egress), and storage (files at rest). Every lane reduces to: catch the bytes, classify, apply policy, evidence it — and every deployment answer the same question FIRST: what does monitoring do to the people being monitored? (That's the constitution.)

Privacy hardline (unit DL0, the course's constitution): DLP inspects CONTENT — power over people. This course: owns-lab data only (your generated datasets, your dev VMs), patterns-match-ONLY (batches/not-policy → don't store raw content), notice-first design (the tool announces its policy), minimization by default (metadata + match evidence, not transcripts), and the BD monitoring-law reading is REQUIRED before unit DL3. No deployable surveillance product emerges; the point is the engineering + the honesty.

---

## DL0 — hello world: DLP's three worlds
Concept: endpoint / network / storage DLP; content types (PII, CCN, secrets, documents); the privacy-first framing. Do: the three-lane map (channel × what-it-sees × where-it-acts); content-type taxonomy (PII classes, card formats, key formats — with the regex/spec notes); write the constitution (above); hello: a tiny detector that spots a US-CCN-shaped string in a file (Luhn-verified — write Luhn yourself) — the seed of everything.
Verify: lane map + taxonomy + constitution written; Luhn CCN detector works.
**Lesson check:** what makes each lane see DIFFERENT data (why can't one sensor cover all three) — and why is "minimization" a design requirement, not a nice-to-have, for content monitoring?

## DL1 — content detection: the scoring core
Concept: detection = confidence: regex patterns + Luhn/checksums + dictionary scoring (names, secrets, document markers) + context (nearby keywords). Do: your content-classifier: pattern library (CCN with Luhn, SSN, IBAN-style, API-key shapes, email/phone), dictionary scorer (weighted term hits — your dictionary), context booster (keyword surrounds); the test corpus (you generate: clean paragraphs, embedded-secret files, borderline near-miss) — report: detection, confidence, misses.
Verify: corpus report shows correct class + confidence; near-miss analysis written.
**Lesson check:** why is CONFIDENCE (not just yes/no) what a policy engine needs — and what does pure-regex miss that context catches (and vice versa)?

## DL2 — the policy engine: rules with teeth (and a brake)
Concept: policy = who/what/where → action: rule format (channel, direction, content-class, threshold, action: AUDIT/QUARANTINE/BLOCK); dry-run-first (XD5 discipline). Do: your policy format + engine (rule parse, match evaluation, action dispatch), the action bus (audit-log, quarantine-vault, block-result — abstract, so lanes plug in), policy config files (YOUR examples: USB-block-CCN, email-out-audit-PII, file-quarantine-secrets); a demo run with dry-run vs armed (same events, different results demonstrated) + audit trail.
Verify: policy battery (3 rules × 3 test events) correct in dry-run AND armed; audit log complete.
**Lesson check:** why must policy live OUTSIDE the sensor (who edits policy, how audited — the config-is-law principle) — and what does dry-run buy a DLP rollout (what's the classic rollout disaster)?

## DL3 — endpoint lane: clipboard, USB, and the device exits
Concept: endpoint exits: clipboard transfer/copy, USB mass-storage write (blockable), print, removable-media races. Do: the clipboard monitor (your own: OS clipboard API polling with change-event detection — cross-OS reading: Wayland/X11/clipboard on Linux, macOS pasteboard, Windows clipboard chain) — labels each copy's content class per policy; the USB-write lane (reading + eBPF-based block on your Linux VM — from USB-DRIVER/22 steps: mount/write interception or device-allowlist lite); the lane report: per-exit events + classifications.
Verify: clipboard-copying a planted secret → policy hit routed; USB write attempt to non-allowlisted media → blocked (log evidence).
**Lesson check:** why is the clipboard the highest-frequency DLP exit (what does EVERY workflow touch) — and why is USB blocking a RACE (what does a determined exfiltrator do that your block must beat)?

## DL4 — network lane: egress inspection from your proxy work
Concept: network exits: SMTP, web upload, cloud sync — inspect what your own proxy machinery already decrypts: TLS metadata + plaintext POSTs/SMTP (YOUR traffic only, lab servers). Do: network sensor: from PROXY-MITM skills, a content-inspection pass on your own lab HTTP/SMTP: classify bodies' content classes; the TLS blind-spot honesty (e2e-encrypted channels are invisible — document the boundary and the metadata-only fallback); egress-flow analytics (who uploaded how much — volume anomaly). 
Verify: a lab SMTP/HTTP leak (planted) is classified + policied; TLS blind-spot + fallback documented.
**Lesson check:** what does the network lane see that endpoint never will (centralized egress) — and why is encrypted egress the permanent DLP Achilles heel (what do real products do — policy, not magic)?

## DL5 — storage lane: files at rest, the sweep
Concept: find sensitive AT REST: share/file-system scan → classify → index → quarantine vault; the boundary case: names (crypto-strength vs label). Do: your file scanner: walk your lab directory tree, classify contents (from DL1 core), index hits (file, class, confidence, timestamp), quarantine vault (move-with-evidence + review queue); the cache: only re-scan changed files (inode/mtime) — the perf lesson; a planted-secret test file found + quarantined.
Verify: scanner finds the planted file; quarantine + review flow works; re-scan skips unchanged (bench line).
**Lesson check:** why does storage-DLP need a cache (what's the real cost of full-tree scans) — and what does QUARANTINE buy over delete (what's the review workflow)?

## DL6 — cloud and mobile: the frontier lanes
Concept: cloud upload/Drive-class sync, mobile messaging/MDM DLP — mostly READING (vendor APIs), honest walls. Do: cloud-lane reading (upload API metadata inspection — what a real product does via e-discovery/API), mobile DLP reading (MDM containerization, iOS/Android MAM — why mobile DLP is app-level not kernel), the "what's actually protectable on mobile" table; your integration note: where YOUR dlp-lite would plug an API uplink if licensed (design doc, no vendor deps).
Verify: reading table + design doc written; mobile ceiling documented as such.
**Lesson check:** why is cloud/mobile DLP structurally weaker than endpoint (who owns the stack?) — and what does MAM (app-level) buy that MDM (device-level) can't?

## DL7 — the privacy balance: the hardest unit
Concept: content monitoring vs privacy: the law (BD précis upgrade: GDPR lawful basis, employer monitoring, consent), the notice design (your tool announces policy, in-app + documented), minimization engineering (match-evidence only, auto-expiry, no transcripts), the "chilling effect" essay. Do: the privacy design review of YOUR dlp-lite (threat-model style: what could an operator misuse, what does the tool log, retention, who's the data subject) + a notice page for a fictional org + the 300-word essay taking a position (when is content-DLP justified — arguer's honor). 
Verify: privacy review + notice + essay; the review lists 3 misuse vectors and their mitigations.
**Lesson check:** what EXACTLY does minimization give up (detection quality?) and keep (lawful operation) — and why is "notice" central to lawful monitoring (what does surprise do to consent)?

## DL8 — evasion reality: what DLP can't see
Concept: bypasses: zip/encoding, word-splitting, tiny fragments, steganography, encrypted tunnels — the honest limits. Do: the bypass lab (your own lab): re-encode a planted secret (base64/zip/rot) and run your classifier — the misses table (what still caught it: volume, filenames, entropy?); the limits essay (what is fundamentally undetectable and why real DLP pairs content with BEHAVIOR — tie to BD); the "DLP as deterrent + detector, not wall" position.
Verify: bypass lab produces the misses table; essay written.
**Lesson check:** what survives re-encoding that content-matching loses — and why do real deployments lean on-behavior signals too (what does BD add that content can't)?

## DL9 — incident workflow: from alert to lesson
Concept: DLP events become work: severity (class × channel × user), review queues, remediation playbooks (false-positive release, true-positive containment, user education), metrics. Do: your DLP case workflow (from XD6/BD): alert → queue → review (evidence, class, confidence) → action (release/contain/educate) + metric counters (correct vs FP per rule); the 3-event shift (one true leak, one FP, one training-teachable) walked to completion; the feedback loop: FP → rule/policy tweak (measured).
Verify: 3-event shift complete with metrics + one rule tweak measured.
**Lesson check:** what does the review queue decide that automation can't (damage assessment vs user intent) — and why is FP-rate the metric that makes or breaks DLP trust?

## DL10 — the product: dlp-lite
Concept: one CLI + config: content core, policy engine, three lanes, dashboards, report. Do: `dlp-lite` (your name): `scan <path>`, `watch <lane>`, `policy reload`, `quarantine list`, `report`, `health`; connect all lanes to the policy bus; the integrated demo: a full scenario (plant secret → copy it → mail it from lab server → drop the file) — all three lanes hit policy correctly with the case queue populated.
Verify: full scenario runs end-to-end; case queue + report show all hits with evidence.
**Lesson check:** what did INTEGRATING the lanes force you to standardize (and what resisted) — and why is the REPORT the commercial face of DLP (who reads it)?

## DL11 — the real world: vendors and roadmap
Concept: what the majors do (MS Purview, Forcepoint, Symantec), DLP-in-SIEM/XDR integration, insider-threat overlap. Do: reading: 2 vendor architecture docs (what they claim, where they concentrate), the DLP+SIEM integration note (your dlp-lite feeding your SIEM — design, one day of work), the insider-threat overlap (DLP meets BEHAVIOR: who's exfiltrating vs who's careless); the vendor-gap essay (what you'd buy vs build).
Verify: reading notes + integration design + essay written.
**Lesson check:** what do real products do that your dlp-lite honestly can't (scale, OCR, semantics, ML) — and where is the SAME core? (content → policy → action is universal)

## DL12 — CAPSTONE: the leak, cold
Prereq: DL0–DL11. **Close all notes.** Cold: rebuild content core + policy + ONE lane + queue in one sitting (no notes), then run a FRESH planted-leak scenario (never before: new secret shape, new path) — caught at the lane you rebuilt, triaged, remediated. Write `labs/dlp-capstone.md`: architecture, policy set, privacy review (DL7), bench (scan/classify rates), three proud decisions, regret, roadmap (OCR, ML semantics, cloud API lanes).
**Pass = the cold rebuild catches the unseen leak; the privacy review and essay are your own.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in DL0/DL3 boilerplate (Luhn scaffold, clipboard-poll skeleton) — classifier, policies, lanes, queue, suite written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Privacy hardline: own-lab data only; pattern/match-evidence only (no transcripts); notice-first; minimization by default; BD monitoring-law reading required pre-DL3; no deployable surveillance product.**
5. Honest bar: commercial DLP is enterprise-scale (OCR, semantics, integrations) driven by regulation; this course's bar = a working three-lane content→policy→action system with privacy engineering as a first-class component, proven cold at the capstone — the floor for data-protection engineering and the complete answer to "where did the data go".

## Where this lives
The content-protection twin of the detection family: PROXY-MITM gives the network lane, USB/GAMEPAD + 22-steps the device lanes, BD the ethics + FP discipline, XDR/SIEM the workflow and (optionally) the feeds — one discipline (protect data), three altitudes (device, wire, disk), one privacy spine.