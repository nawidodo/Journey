# Detection Ladder — EDR → Behavior → XDR (the order and why)

One discipline, three courses — same family, different altitude. Build in this order; each one assumes the previous.

## 1. EDR-COURSE (E-units, Linux → macOS → Windows)
**Single host first.** You build the SENSOR: process/file/network collectors per OS (eBPF, EndpointSecurity, ETW), one shared event schema, one detection engine (Sigma/YARA-lite), dry-run-gated response, and the attack/detect/respond duel. This is the foundation: schema design, collector craft, and "one host, one truth" detection fundamentals. Without this, Behavior and XDR are engines with no eyes.
→ Why first: every later course REUSES this telemetry and schema. It's the ground truth.

## 2. BEHAVIOR-DETECTION-COURSE (BD0–BD11) — or DETECTION-COURSE (DT0–DT12) as an orthogonal fork
**Same host, deeper inference.** User-suspicion detection (whoami/ping-class, sequences, per-user baselines) is the specialization of EDR's own process telemetry — commands classified, sessions tracked, intent inferred with honest FP math. Take it when you want the "what does the VERB STREAM mean" layer before scaling out.
*Alternative fork:* DETECTION-COURSE (debugger/hook detection) is ALSO built on EDR's signal craft but toward your OWN app's integrity — pick after EDR, before or after Behavior by interest (they don't depend on each other).
→ Why this before XDR: multi-source correlation only means something when you can already detect well on ONE source. Context and sequence here train the inference muscles XDR's correlation reuses.

## 3. XDR-COURSE (XD0–XD12)
**Many sources, one pipeline:** endpoint + network + identity/mobile collectors feeding a normalized store, cross-source correlation (process→network→file joins), safe response playbooks, SOC alert lifecycle, threat intel, protect-the-protector. This is where EDR's single-host rules and Behavior's sequence/context skills get SCALED into a platform.
→ Why last: cross-source correlation, response automation, and SOC lifecycle all presuppose solid single-source detection and inference from the first two.

## Suggested route
EDR (E) → [DETECTION (DT) or BEHAVIOR (BD)] → XDR (XD) → INTEGRITY/DETECTION-depth when building products.
Orthogonal anywhere: INTEGRITY-COURSE (app protection) and ROOTKIT-DEFENSE (audits) share the blue muscle but target your own software, not a fleet — run them in parallel when you want product-defense, not just detection.

## Why not one merged course?
They're already modular — merging would bolt three altitudes into one slab: you'd carry EDR's collector work into Behavior's inference into XDR's scale as one 36-unit monster with no checkpoint. Separate courses = separate cold capstones = three proof points on your CV. The ladder above IS the merge, sequenced.