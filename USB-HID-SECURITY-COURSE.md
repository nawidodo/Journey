# USB-HID Security Course — Absolute-Beginner (hello world → HID-emitter mechanics OWNED BENIGN + the defense stack that catches malicious USB devices, gated)

**The honest version of "USB trojan":** keyboard-emulation attack devices (Rubber-Ducky-class) are attack hardware for compromising OTHER people's machines — a crime, not a craft, and this repo does not build them BY DESIGN (the USB-GAMEPAD course built the same mechanics as a gamepad; the line holds for keyboard-shaped payloads). What this course DOES: own the HID-emitter mechanics in fully benign form (your board types YOUR text into YOUR OWN VMs — automation, accessibility, QA — with a visible banner) and then the entire DEFENSE stack: USB device fingerprinting, allowlist policy (USBGuard-class), HID-typing behavioral anomaly, per-OS device audit lanes, and detection engineering that catches real rogue-HID attacks — including your own benign board used as the test adversary in your own lab. Prereqs: USB-GAMEPAD UG0–UG4 (descriptor + device stack) or equal hardware competence. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/hsN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): the attack class: a USB device lying about what it is (a "keyboard" that's actually a payload launcher) — detector = trust nothing USB says: fingerprint what it IS (descriptors, VID/PID, chains), police where it can BE (policy: allowlist/exceptions), watch what it DOES (typing rates, volume, timing that humans can't do). You'll build all three layers and the per-OS wiring, and the course's final exam is catching your own lab's HID board attempting a payload into a VM it's not allowed to touch.

Safety hardline (unit HS0, the constitution): emitter target = YOUR OWN VMs/sessions ONLY; payload strings = text you wrote (banner-visible); no persistence, no automation of real systems, no exfiltration shape; attack-class firmware = reading-only (HS8); your board is the ADversary only inside your own sandbox; no device ever targets a third party's system.

---

## HS0 — hello world: the attack class and the defense posture
Concept: USB trust history (BadUSB research), HID injection mechanics, defense mega-layers (fingerprint/policy/behavior); the constitution. Do: read ONE BadUSB-class research overview (mechanism, what a rogue device can do, why USB can't be "fenced"); the layer diagram (identify → police → observe) with your planned tooling per layer; hello: enumerate EVERY USB device on your Linux VM via sysfs/lsusb (descriptor dump) — your future fingerprint corpus.
Verify: layer diagram + reading notes written; device enumeration works (your board listed).
**Lesson check:** why can't you "block USB attacks" by protocol design (what is USB's fundamental trust property) — and which of the three layers is actually the FIRST one a defender needs?

## HS1 — the emitter, benign: your board types in YOUR VM
Concept: HID keyboard emitter mechanics: report descriptor (Keyboard class), keystroke reports (modifiers+keycodes), pacing — the benign core. Do: from UG skills: your board enumerates as a KEYBOARD; it types YOUR prepared strings into YOUR test VM via a control switch (physical button or your CLI) with a visible banner (board LED + VM-side echo); pacing control (wpm knob — the difference between human and robotic, your first behavioral signal); the demo: exact string typed with correct modifiers/mapping.
Verify: string typed correctly into the VM, banner visible, pacing adjustable; no other target ever touched.
**Lesson check:** what does the REPORT DESCRIPTOR declare that makes hosts treat you as a keyboard — and why is PACING the first tell (what does a real attack need that an honest automation doesn't)?

## HS2 — fingerprinting: what the device REALLY is
Concept: identity = descriptors: VID/PID, serial, manufacturer/product strings, HID usage pages, interface classes, port topology; lies vs tells. Do: your fingerprint tool: walk sysfs/USB device tree, extract the descriptor set, build a fingerprint hash (canonicalized — strings trimmed, chain included); your known-good DB (your own devices — board, keyboard, mouse, hub); the anomaly demo: a device claiming YOUR board's VID/PID but a different descriptor chain → flagged (the "spoofed identity" case — theoretical lab only).
Verify: fingerprints recorded for all lab devices; spoof-simulation flagged by chain mismatch.
**Lesson check:** what identifies a device better than VID/PID (what's trivially spoofable vs hard) — and why does CHAIN/topology context matter (where a device plugs in)?

## HS3 — policy: USBGuard-class enforcement
Concept: policy = decide by identity: allowlist/denylist/exceptions (by descriptor match), per-port rules; enforcement on Linux via usbguard (reading + your policy tool). Do: your policy tool: rules in YOUR format (device pattern → allow/deny, port scope), policy apply via usbguard's API (documented) or a udev-based shim you script, deny-demo: your own board (as "unknown device" — revoked from allowlist) is BLOCKED at connection; event log + re-allow flow; the "default-deny is the only safe default" note (why allowlist > denylist).
Verify: board blocked then re-allowed per policy with events; deny-by-default config active.
**Lesson check:** why is default-DENY the only correct posture (what does default-allow cost) — and what does a policy rule need to survive (what breaks when a device's strings change)?

## HS4 — behavior: the typing anomaly detector
Concept: humans don't type 300 chars/sec from HDMI-port devices: HID keystroke-stream analytics: typing rate, volume caps, burst shapes, message-of-doom timing. Do: your HID event watcher (evdev read on the Linux host — from INPUT-SECURITY skills): keystroke stream per device, features (rate, gaps, burst max, total volume), anomaly scoring vs human thresholds (documented; your honest thresholds), alert on: extreme rate, huge single-burst, macro-timing (uniform gaps — robotic signature); test: your HS1 emitter at high wpm triggers; your own REAL typing does not.
Verify: high-wpm emitter triggers; human typing silent (FP discipline measured over 3 sessions).
**Lesson check:** what feature BEST separates robotic from human typing (argue with your measurements) — and why is BEHAVIOR the layer that fingerprint/policy can't cover (what slips past both)?

## HS5 — Windows lane: device audit + policy mechanisms
Concept: Windows: device install history, driver install policy (WDAC/Device Guard reading), signed-driver requirements, Defender-for-Endpoint USB signals (event-based). Do: reading unit + hands-on: USB device history view (SetupAPI/PNP logs parsing — your own parser), a device-install policy lab (block installation for a test VID via Group Policy-lite on the VM — documented), event-watch: device arrival events → your alert feed (ETW/Event Log query); the "signed drivers vs HID" note (why HID needs no driver = why HID attacks work).
Verify: history parser works; blocked-VID demoed; event feed functional; signed-driver note written.
**Lesson check:** why does Windows's signing machinery NOT stop HID attacks (what's the gap) — and where do its EVENTS give you the audit trail the policy misses?

## HS6 — macOS lane: the honest wall
Concept: macOS: IOKit USB registry, endpoint-security-class tools reading, System Information USB tree — and the honest ceiling: no public equivalent of USBGuard-class blocking policy. Do: the USB tree exploration (your Mac, your devices), IOKit-registry dumps (iorg), the reading table: what macOS exposes vs what vendors do (3rd-party endpoint tools), the ceiling doc: "on macOS, your defense = network+EDR behaviors + user education — and here's why"; your detection idea mapped to what IS possible (log monitoring via lsof/system_profiler automation).
Verify: USB tree + registry dumps working; ceiling + mapped detection idea written.
**Lesson check:** what does Apple's closed model change about USB defense (who can't approve devices?) — and what is the BEST macOS-layer signal you found (argue)?

## HS7 — hardware realities: ports, hubs, and the bus truth
Concept: ports are physical trust: locked cases, USB port control (physical + BIOS options), superhub chaining, the "firewire/PCIe-attack" adjacent classes (reading: DMA attacks), the "USB is a bus, not a fence" essay. Do: the hardware-threat map (physical access tiers: kiosk, unlocked desk, insider) × mitigations (port control, BIOS settings reading, lockdown options); the essay (300 words: why physical access beats software defense, and what that means for your defense layering); a locked-down-VM demo (no-USB policy in the VM settings — your defense proxy).
Verify: threat map + essay written; locked-VM demo (board denied at VM level) works.
**Lesson check:** why does physical access WIN against software (what can a wire touch that an OS can't inspect) — and what does that say about where your ENVIRONMENTAL controls matter most?

## HS8 — the adversary, read-only: implants and persistence
Concept: how real USB implants work at full depth: firmware infections (HID+storage dual role, badUSB classic), controller-level attacks (reading), supply-chain compromise — READ ONLY, no code. Do: reading unit: firmware-level attacks research (mechanism, detection difficulty), supply-chain cases (who got compromised via device firmware), the "what finally caught them" column (every real case has tells — vendor response, EDR, user report); map each to YOUR layers (which layer would have caught each — honest gaps admitted).
Verify: case maps + layer-gap table complete; no code written or run.
**Lesson check:** which real implant would YOUR current stack catch (and which would win — the honest ranking) — and what does that tell you to add?

## HS9 — the dual-use logic: automation vs attack
Concept: the SAME emitter mechanics serve accessibility, QA, kiosk automation — and attack; the line is target/consent/purpose. Do: the 400-word essay (your position): when HID automation is legitimate (accessibility input, self-hosted QA, your own devices), what separates it from attack (target, consent, disclosure, purpose), the guardrail checklist (own-target, visible, no persistence, no capture); defend it in notes.
Verify: essay + checklist written; you can answer "so why can't I just demo it on a friend's laptop".
**Lesson check:** which ONE fact changes your benign emitter into an attack tool — and what does your checklist do about that fact?

## HS10 — the product: usb-watch
Concept: the integrated form: fingerprint DB + policy + behavior + event feed, one CLI + alert surface (feeds XDR/SIEM). Do: `usb-watch` (your name): `list` (devices + fingerprints + posture), `policy apply`, `watch` (live: connect/deny/typing events), `alerts`, `report`; wire alerts to your SIEM-harness (feed format documented); the full test battery: your benign board as attacker: revoked-from-allowlist (blocked), re-allowed at high wpm (behavior alert), normal use (silent) — all three lanes proven.
Verify: battery runs the three-lane proof end-to-end with report; SIEM feed demoed.
**Lesson check:** what does INTEGRATION reveal that separate tools hide (the correlation win) — and why is the REPORT the sales face (who reads a USB-security report)?

## HS11 — the real world, read: research, standards, products
Concept: the field's knowledge: forensic USB analysis (vendor research), NIST guidance, enterprise USB-security products (what they actually do). Do: reading: NIST/USB-security guidance summary, forensic analysis papers (what artifacts survive a USB incident), enterprise product arms-length breakdown (what's marketing vs engineering — my honest read); the "what I'd advise a small company" page (your layered answer, budgeted).
Verify: summaries + product read + advice page written.
**Lesson check:** what does NIST actually recommend (and what's its weak spot) — and why do enterprise USB products focus on EXFIL (what's the most common real injury)?

## HS12 — CAPSTONE: the catch, cold
Prereq: HS0–HS11. **Close all notes.** Cold: rebuild fingerprint + policy + behavior detector in one sitting (no notes), then: a fresh session — your board (unknown identity, will type a payload into the sandbox VM) — your cold-built stack BLOCKS it by policy; then re-allow it at attacker wpm (your board, your VM, your text) → behavior alert fires; the report + HS9 essay. Write `labs/usb-hid-capstone.md`: architecture (3 layers), fingerprint/policy/behavior designs, battery results, essay, three proud decisions, regret, roadmap (DMA-attack lane, firmware forensics, SIEM deep integration).
**Pass = the cold stack blocks and flags the board as designed; the report and essay carry the file.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in HS0/HS1 boilerplate (enumeration scaffold, HID keyboard skeleton from UG) — fingerprints, policy, behavior analyzer, watch written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. **Constitution: emitter targets YOUR OWN VMs only, banner-visible, your text, no persistence, no capture; attack-class firmware = reading-only; your board is the adversary only inside your sandbox.**
5. Honest bar: real USB security is vendor research + forensic practice (and physical access still wins); this course's bar = benign HID ownership plus a layered detect-and-block stack proven against your own lab adversary, cold at the capstone — the floor for USB-trust engineering and the complete answer to "should I plug that in".

## Where this lives
The defensive twin of USB-GAMEPAD (same device mechanics, opposite purpose); feeds your XDR/SIEM/MDM lines (device posture events), reuses INPUT-SECURITY's evdev vision, INTEGRITY's evidence discipline — one board, two courses, both sides of the same trust question.