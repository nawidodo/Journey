# MDM Course — Absolute-Beginner (hello world → YOUR OWN mobile-device-management platform, Android live + iOS fence, gated)

Zero MDM knowledge assumed — you bring: Android-lane from the mobile courses (emulator farm, app builds), DETECTION/BD signal craft (root/jailbreak checks), INTEGRITY (agent verification), XDR/SIEM workflow (compliance + incident), TLS basics (server auth, from 20-07). MDM = Mobile Device Management: enroll devices (provisioning), push policies (restrictions/config), verify compliance (OS state, apps, security posture), and act remotely (lock/wipe/update) — the enterprise's device custody layer. This course builds `mdm-lite`: your agent app + your server, a real Android Device-Owner enrollment LIVE on emulators, policy+compliance+actions end-to-end, and the iOS lane as reading + protocol skeleton (APNs/certs — the honest Apple fence). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/mdN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): MDM's shape: a server that claims custody (enrollment proves the device accepts your authority), controls it (policies + app management), knows it (compliance inventory), and can act on it (remote actions). Android does this via Device-Owner/Device Admin APIs; iOS via APNs + configuration profiles behind Apple's cert wall. The ladder you'll climb: enrollment (the handshake) → policy push (the levers) → compliance (the truth) → action (the teeth) — and the modern add-on: auto-provisioning (zero-touch onboarding) that makes fleet custody one-step.

**Privacy/fiduciary hardline (unit MD0, the constitution):** MDM holds DEVICE POWER (wipe, lock, read posture) over people's machines — BYOD means personal data on the same device. This course: emulator fleet only, simulated users, policies on your own test corpus; the privacy unit (MD8) is mandatory before remote actions (MD6); wipe/lock are reversible-lab-only here (snapshot-first, confirm-step, audit-log, no persistence); the "MDM as spyware" criticism is read and answered, not dodged.

---

## MD0 — hello world: the custody model
Concept: enrollment → policies → compliance → actions; the ecosystem alphabet (MDM/EMM/MAM/UEM). Do: the four-step custody diagram + ecosystem map (who owns device/identity/apps); the "what MDM can and can't see" table (version? yes; messages? NO — the boundary that defines trust); hello: your Android emulator + debug-bridge (adb) session — observe the device's current state (version, apps) — your seed of the inventory view.
Verify: diagram + table + ecosystem map written; emulator inventory read via adb.
**Lesson check:** what does the MDM-visible boundary have to be (why CAN'T it see messages if it can see apps) — and why is that boundary the product's trust contract?

## MD1 — enrollment: the handshake of custody
Concept: enrollment proves authority: Android Device Owner (adb provisioning, DPC app), iOS MDM enrollment (APNs token + profile payload, cert-gated). Do: your DPC app (Device Owner via `dpm set-device-owner` on the emulator), enrollment flow: device → your server → token → DO granted → agent active; the enrollment state persisted + re-verified; iOS lane reading: the enrollment profile format + APNs flow (why Apple guards it — honest fence documented).
Verify: emulator enrolls to YOUR server (server shows the device); re-verify persists; iOS flow reading done.
**Lesson check:** what does enrollment GRANT and what does it NOT grant (which APIs open, which stay closed) — and why is iOS enrollment a wall (what does Apple want in exchange for the keys)?

## MD2 — policy engine: the levers
Concept: policies = restrictions + configs: camera/install-source/network settings/bluetooth; push model: pull-poll (lite) + push (reading: FCM/APNs delivery). Do: your policy store + editor (JSON policy doc: restrictions, apps, config, version), the push/pull loop (agent polls your server — low-latency-lite honest), application: a policy that disables camera on the emulator + restricts install sources; verification: device reflects policy, policy-version visible.
Verify: policy applies (camera disabled proven); version/revocation works (rollback tested).
**Lesson check:** why is policy a DOCUMENT with a VERSION (what does rollback require) — and why do real systems push AND poll (what fails with push-only)?

## MD3 — profile & managed-config layer
Concept: per-app managed config (Android managed configuration), profile storage, assignment (device groups), revocation lifecycle. Do: your assignment model (groups → policies — a config table), managed config delivered to YOUR agent app (agent reads + applies), profile lifecycle: active → superseded → revoked with the state machine; a group demo: two emulators, different policies, correct application each.
Verify: groups apply correctly per emulator; revocation takes effect; state machine clean.
**Lesson check:** what does GROUP assignment buy over device-by-device (who configures a 10k fleet?) — and why must revocation be GRACEFUL (what breaks on a hard revoke)?

## MD4 — compliance: the truth engine
Concept: compliance = scored posture: OS version, root/jailbreak detection, app allowlist, encryption state, profile integrity. Do: your compliance checks (agent-side probes: root detection via DETECTION/BD signals, package allowlist scan, encryption flag, policy-version match) → server-side scoring + report; the non-compliant state: device flagged, grace period, (optional) restricted action; a planted violation (install a non-allowlisted app / toggle root flag in lab) → compliance drops with evidence.
Verify: compliance report correct on clean + planted-violation states; grace-period behavior demoed.
**Lesson check:** why is compliance SCORING + GRACE better than binary pass/fail (what does the human operator need to decide) — and what's the honest limit of client-reported state (what can the agent NOT see)?

## MD5 — app management: the catalog
Concept: managed app lifecycle: catalog → install → update → remove: Android (package install APIs / managed Play reading), iOS VPP/MDM reading. Do: your app catalog server (APK store-lite on your lab), agent-side installer (download + verify hash (INTEGRITY) + install + report), update loop (version bump → push update), remove action; the sideload-BY-policy demo: enroll-required app installed on the emulator via YOUR catalog.
Verify: catalog install/update/remove lifecycle proven on the emulator; iOS VPP reading noted.
**Lesson check:** what does HASH-VERIFIED install buy (supply chain! why is this INTEGRITY's lane) — and why do enterprises use MDM app stores at all (what beats telling users to download)?

## MD6 — remote actions: lock, wipe, locate (with brakes)
Concept: the teeth: lock, wipe, data-erasure-classes (device vs corporate wipe), locate-by-inventory; the irreversible-safety ritual. Do: agent action-queue (server enqueues → agent executes → ack), lock + screen-pin demo on the emulator, wipe action (FACTORY-lite on a THROWAWAY emulator clone, snapshot-first), corporate-wipe concept (selective data erasure — implementing selective: your agent deletes app-data subset); the ritual: confirm-step + audit log + reversible-lab-only (no real-device wipe ever).
Verify: lock demoed; selective wipe removes planted data; audit trail complete; ritual documented.
**Lesson check:** why is WIPE a ceremony and not a button (what's irrecoverable, what's the operator pressure) — and what does CORPORATE-wipe (vs full) do for BYOD trust (the MD8 preview)?

## MD7 — zero-touch: the modern onboarding
Concept: out-of-box auto-provisioning: Android Zero-touch, Apple Automated Device Enrollment (ADE): device boots → silently enrolled → corporately owned. Do: reading unit (both flows' architecture — why they need OEM/Apple cooperation, what the device proves to whom), the design doc "if I ran a reseller-integrated MDM" (your zero-touch flow: order → claim key → unbox → enrolled), the enrollment-security note (how a stolen device is blocked: ownership proof + jailbreak-of-provisioning attempts reading).
Verify: both flows mapped + design doc written; theft-blocking analysis done.
**Lesson check:** what does zero-touch REALLY automate (who still touches the device, just not the admin) — and why does vendor cooperation exist at all (what securitizes the first boot)?

## MD8 — the privacy unit (mandatory before actions)
Concept: BYOD vs COPE, personal data on managed devices, what MDM must NOT touch, the lawful basis. Do: the BYOD/COPE matrix (ownership × data × allowed-control), the "MDM can see" truth table (technical capability vs policy-legitimacy), the 400-word essay: "where is the line between custody and surveillance on a managed device" — with the minimization answer (posture+policies, not content); the operator-guardrails checklist (consent, disclosure, data-class boundaries, audit).
Verify: matrix + table + essay + guardrails; you can defend the line in notes.
**Lesson check:** what does technical CAPABILITY have to do with LEGITIMACY (nothing — so what bridges them) — and why is BYOD the case that forces the design (what would break without the boundary)?

## MD9 — protecting the protector
Concept: the MDM server is the highest-value target: agent+server auth, TLS first, enrollment anti-spoof (device proving identity — reverse direction!), server integrity. Do: secure the plumbing: your server over TLS (own cert, from 20-07), agent↔server auth (token + mutual-ish check), enrollment replay protection (nonce — from INTEGRITY/attestation), server-side logs + integrity (hash-chain from INTEGRITY/SIEM); the threat-model doc: attacker steals agent token / impersonates server / wipes fleet — mitigations for each (including the "attestation would help" honest note).
Verify: TLS + replay + token checks demonstrated (try a replayed enrollment → rejected); threat model written.
**Lesson check:** why is the MDM server the crown jewel (what breaks fastest if it falls) — and why can't a wiped-fleet scenario be fully solved client-side (what must the server do differently)?

## MD10 — the real world: Intune/Jamf/WSO2 and the criticism
Concept: real products, UEM convergence, and the "MDM is spyware" argument answered. Do: reading: 2 product architecture/capability docs (Microsoft Intune, Jamf-class), the UEM note (why desktop+mobile merged into one console), the criticism file: read the anti-MDM arguments (worker surveillance, lock-in), your response essay (engineered: boundaries, consent, transparency — from MD8) — 300 words, argued both sides.
Verify: reading notes + UEM note + response essay written.
**Lesson check:** what is the strongest anti-MDM argument (not the weakest) — and what does honest engineering add to the debate that marketing can't?

## MD11 — the product: mdm-lite console
Concept: usable fleet view: inventory, policy editor, compliance, actions, audit — the operator's face. Do: `mdm-lite` (your name): console (device list + posture, policy editor with apply, action buttons behind the MD6 ritual, audit log view), agent app wire-up, the fleet demo: TWO emulators, different groups, one violation planted → console shows posture drop → action taken via ritual → audit complete.
Verify: fleet demo end-to-end on the console; audit log matches every action.
**Lesson check:** what does the CONSOLE decide that the backend can't (the human judgment slot) — and why must every action be an AUDIT EVENT (who reads the log at 3am)?

## MD12 — CAPSTONE: the fresh fleet, cold
Prereq: MD0–MD11. **Close all notes.** Cold: rebuild enrollment + policy + compliance + one action in one sitting (no notes), then: enroll a FRESH emulator, apply a group policy, plant a violation, see compliance drop, perform lock + selective-wipe with the ritual, write the audit story. Write `labs/mdm-capstone.md`: architecture, protocol (enroll/policy/compliance/action), privacy page (MD8 essay), threat model (MD9), three proud decisions, regret, roadmap (zero-touch integration, attestation, UEM expansion).
**Pass = the cold rebuild runs the full custody loop on the fresh emulator with the ritual and audit intact.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in MD0/MD1 boilerplate (adb scaffold, DPC skeleton) — agent logic, policies, compliance, actions, console written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Fiduciary hardline: emulator fleet only, simulated users; wipe/lock = lab-only, snapshot-first, confirm-step, audited; MD8 before MD6; no real-device remote actions ever.**
5. Honest bar: Intune/Jamf are enterprise platforms (cert pipelines, cloud scale, OEM deals); this course's bar = a working enroll→policy→compliance→action loop with privacy and safety rituals solid, proven cold at the capstone — the floor for mobile-enterprise engineering and the complete answer to "who owns this device".

## Where this lives
The device-custody twin of the detection family: Android-lane emulator skills, DETECTION/BD root-and-posture signals, INTEGRITY hashing, 20-07 TLS, XDR/SIEM audit workflow — one discipline (custody with consent), one floor (your lab), one spine (evidence + privacy).