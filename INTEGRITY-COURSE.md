# Application Integrity Course — Absolute-Beginner (hello world → YOUR OWN cross-OS app integrity protection + attestation, gated)

Zero integrity knowledge assumed — you need your Mac + Linux VM + Windows VM + Android emulator (the four major apps-hosts), the crypto floor (own hashes/signatures from 20-crypto steps), the hooks-eye from INSTRUMENT-COURSE (runtime detection reuses your hook vision), and TPM/measurement ground from ROOTKIT-DEFENSE RK5/RK7. The course builds the full integrity stack honestly: static verification (manifests, hashes, signatures) → runtime self-protection (own-bytes checks, hook detection) → per-OS signing chain mastery → and the honest endpoint: remote attestation (the only place client-side integrity actually wins). This is the DEFENSIVE twin of the VM-obfuscation world: if VO taught you to hide logic, this teaches you to VERIFY state — your own software's integrity, everywhere it runs. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/itN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): "is this app what it claims to be" has three answers: statically (files match a signed manifest), at runtime (self-checks + hook scans confirm nothing moved), and remotely (a server challenges the device — attestation). Layers stack: your own hash-verifier, the OS's signing system (Authenticode/codesign/APK), your runtime guard, and finally the TPM/Play-Gatekeeper-class remote attester. Each major OS signs and verifies differently — the course owns the differences precisely and builds one integrity-suite across all four.

Honesty clause (unit IT0's constitution): client-side integrity alone NEVER wins against a determined tamperer (the game is racisation until remote attestation, and even it has limits — you'll write that essay at IT9); this course builds the real product shape (defense-in-depth + server check), not a magic shield; tamper-resistance work is done ON YOUR OWN binaries, with tampering tests performed by YOU on your own artifacts (attack = measurement).

---

## IT0 — hello world: the integrity question
Concept: integrity = verify unchanged: static (files), dynamic (memory/behavior), remote (attestation); the three-layer map. Do: write the layer map (static/runtime/attestation × per-app-need); build hello: your own checksum manifest tool (hash every file of a sample app you wrote → manifest; verify; TAMPER one byte → verify fails); the "what does integrity protect (and not)" note (confidentiality? no — authenticity/unchangedness only).
Verify: manifest verify/fail round-trip on your own app; layer map written.
**Lesson check:** what exactly can integrity prove (and what can't it ever prove — think: is the verifier itself trusted)?

## IT1 — hashing and manifests: your own verifier
Concept: verifier needs: strong hashing, manifest format, atomic compare, failure policy. Do: manifest format YOUR design (version, per-file hash, size, mode); verifier with caching + report (file | expected | actual | verdict); test suite: clean pass, single-byte tamper, added-file, swapped-file — all reported; the "how your verifier is attacked" list (manifest swap, verifier patch, hash-in-MD5-collision note — why SHA-256+).
Verify: tamper matrix all-green (correct verdicts); verifier survives its own test suite.
**Lesson check:** what does a manifest assume about itself — and why is the VERIFIER the actual attack surface (what would you patch first if attacking)?

## IT2 — code signing, the language: chains and formats
Concept: signing = manifest + authority: certificates, chains, timestamps — the vocabulary every OS uses. Do: build the signing-chain reading notebook (per OS: format, cert chain validation, revocation, timestamping); your own mini-CA + signer (openssl classes from 20-crypto): sign YOUR sample app with YOUR key, verify positive+negative (tampered, wrong-key) — the scheme you'll map to each OS.
Verify: sign/verify round-trip works (own key); chain notebook covers 4 OSes with your notes.
**Lesson check:** what does a SIGNATURE add over a hash (authority! provenance) — and why do all OSes gate code EXECUTION on signatures (what's the privilege boundary)?

## IT3 — Windows lane: Authenticode and the PE
Concept: Authenticode: PKCS#7 signature embedded in PE; signtool/cross-signing; verification behavior (SmartScreen/defender trust). Do: Windows VM: sign YOUR test exe with your own cert (makecert/self-signed — lab key) using signtool (or your own PKCS#7 embedding from 20-crypto TLS knowledge); verify via `signtool verify` + your own extraction script; tamper → invalid (evidence); the cross-signing/EV reality as reading (why real certs are business-grade).
Verify: your signed exe verifies; tampered exe fails with evidence; extraction script shows the signature.
**Lesson check:** where is the signature STORED in a PE — and why does Windows verify chains, not just hashes (what does trust actually anchor)?

## IT4 — macOS lane: codesign, ad-hoc to notarization
Concept: codesign: ad-hoc (fingerprint only) → Developer ID (chain) → notarization + stapling (server-side scan); Gatekeeper's role. Do: on your Mac: codesign YOUR build (ad-hoc then your own identity if you have one — reading otherwise); verify + tamper → codesign --verify fails; the notarization flow as documented reading (why Apple adds server judgment — the IT9 preview); runtime status via `codesign -v` in a script.
Verify: signed app verifies; tampered app fails; notarization reality documented.
**Lesson check:** why does Apple pair signing with SERVER judgment (notarization) — and what gap does that close that signing alone leaves open?

## IT5 — Linux lane: packages, IMA, and the no-single-answer truth
Concept: Linux: package signing (APT/yum/DNF), dm-verity (read-only fs verification), IMA (in-kernel measurement/verification) — plus the honest "Linux apps are often unsigned" reality. Do: verify a package signature on your VM (apt-key/keyserver flow reading + practical `apt-get` verification), enable/observe IMA on the VM (appraise mode if supported; else implement closing: your IT1 manifest as the Linux answer); map the "where does Linux trust come from" essay.
Verify: package verification demonstrated; IMA state documented (running or wall); trust essay written.
**Lesson check:** why does Linux distribute trust DIFFERENTLY (repo keys vs per-app certs) — and what does that design gain and lose vs Windows/macOS?

## IT6 — Android lane: APK signing and Play Integrity
Concept: APK signature schemes (v1/jar, v2/block, v3/key rotation), apksigner; Play Integrity API (server-side, replaces SafetyNet) — the mobile attestation shape. Do: Android emulator: sign YOUR own APK with your key (apksigner reading + practice), verify + tamper (re-sign mismatch) → fails; runtime self-check app: verify your default APK signature + detect common hook frameworks (Frida/Xposed — READING detection APIs, your app refusing to run with them); Play Integrity docs read.
Verify: APK sign/verify round-trip; your app's runtime self-check flags its own tampered build; hooks-reading documented.
**Lesson check:** why does Android move trust to SERVER-side (Play Integrity) for app licensing — and what can a runtime check catch that static signing can't (and vice versa)?

## IT7 — iOS lane: the wall, read precisely
Concept: iOS: code signing REQUIRED (can't run unsigned — the enforcement IS the protection), entitlements/provisioning, App Attestation (device-key challenge-response). Do: reading + skeleton: signing flow review (why iOS apps literally cannot run unsigned — the OS model), App Attestation protocol reading (key in Secure Enclave, challenge-response), your Swift integrity-checker skeleton (compile only, honest ceiling: full test needs device+team); map iOS's model vs Android's (OS-enforced vs market-enforced). 
Verify: writing/reading complete; skeleton compiles; iOS-vs-Android enforcement comparison written.
**Lesson check:** what does iOS's "can't run unsigned" change about integrity engineering (what does the app NOT have to do) — and what can it never protect anyway?

## IT8 — runtime self-protection: your own guard
Concept: the live layer: self-checking bytes, hook detection, guard timers, anti-debug lite — the client-side arms race, honestly. Do: build your runtime guard (in YOUR app): periodic self-hash of own image + manifest files, INSTRUMENT-style hook-scan over your own functions (inline-hook byte check), ptrace/TracerPid self-check (from DI7), integrity timer + fail behavior (refuse/alert — your policy); test suite: patch own binary mid-run, hook own function, attach debugger → all detected with logs.
Verify: all three attack classes detected live; fail-policy demonstrated; false-positive discipline (clean run stays silent).
**Lesson check:** what can a runtime guard catch that static verification can't — and why is EVERY runtime guard ultimately defeatable (what's the honest ceiling, and what does IT9 add)?

## IT9 — the arms race, written: why client-side loses alone
Concept: tamperers patch, hook, and emulate; verification itself must be verified; the answer is elsewhere. Do: the essay (400 words, your position): the loop (guard → bypass → stricter guard → bypass), the lesson (client-side integrity = deterrence plus tripwires, not a lock), the honest architecture (remote attestation + server decision + risk-based limits); guardrail checklist (tamper-test on your own artifacts only, disclose protections, no user-hostility). 
Verify: essay + checklist written; you can answer "so why bother at all" with the tripwire defense.
**Lesson check:** what ONE capability breaks the arms race deadlock — and what does a tripwire still buy you even when broken?

## IT10 — attestation: the server side actually winning
Concept: attestation: device proves state to a challenger: TPM quotes (from RK5/RK7), iOS App Attestation, Play Integrity — challenge-response protocols. Do: build your attestation protocol-lite: client measures (hash manifest + your own binary + TPM PCRs where available) → challenge → signed measurement (your IT2 key) → server (your own Python) verifies chain + freshness + policy; running demo: honest client passes, tampered client fails, replay attack on nonce fails; the replay/relay-attack reading (why fresh challenges + device keys matter).
Verify: attestation demo passes/fails correctly incl. replay rejection; protocol doc written.
**Lesson check:** what does the CHALLENGER add that local verification can't (freshness! third-party trust) — and why does this still not defeat a full device emulator (what's the final limit)?

## IT11 — the product: integrity-suite, four OSes
Concept: one suite, per-OS modules, one report format: verify/status/attest. Do: `intygrity` CLI (your name): per-OS modules (manifest verify everywhere, codesign/authenticode/APK checks wrapped), attestation-lite client+server, `status` report (layer | finding | evidence); run the TU test script (battery of tamper mutations across a sample app on 2+ OSes — mutating YOUR OWN artifacts); report card per OS.
Verify: suite runs on Linux+macOS (+Windows VM if reachable); TU battery reproduces all detections; report cards written.
**Lesson check:** what did capturing FOUR OSes in one tool force you to unify (and what resisted unification) — and what does a product's quality hinge on besides the checks?

## IT12 — CAPSTONE: full stack on a real app, cold
Prereq: IT0–IT11. **Close all notes.** Cold: take a fresh app YOU wrote (any OS): sign it, manifest it, runtime-guard it (2 checks), attestation-lite client+server wired — in one sitting; then the tamper battery (3 mutations, own artifacts) with the report; the IT9 essay as cover. Write `labs/integrity-capstone.md`: architecture diagram, signature+manifest+guard+attestation flow, tamper report, three proud decisions, regret, roadmap (kernel-level verification, distributed attestation, DRM-adjacent ethics note).
**Pass = the full stack verifies live, the tamper battery is caught cold, and the essay reads like a principled engineer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in IT0/IT3 boilerplate (manifest scaffold, sign tool calls) — formats, guards, attestation protocol, suite written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. Boundary: protects YOUR software; tamper-tests run against YOUR artifacts; detection of hook frameworks = reading/detection, never bypass; no DRM-helping-the-ABUSE (digital-restriction beyond your own app's integrity is out).
5. Honest bar: real integrity products are business infrastructure (enterprise EMM, mobile banks' attestation, game anti-tamper); this course's bar = a working cross-OS verify→guard→attest stack with a tamper battery proving it, discipline intact, proven cold at the capstone — the floor for application-verification engineering and the exact anatomy of every "is this app safe to run" question.

## Where this lives
The verification twin of VM-OBFUSCATION (protect + its detector, one family); ROOTKIT-DEFENSE's TPM/PCR work becomes your attestation engine; INSTRUMENT's hook-vision powers the runtime guard; your crypto steps supply keys, chains, and signatures — each lane already built, now wired into one integrity spine across every major OS.