# Input-Observation & Defense Course — Absolute-Beginner (hello world → YOUR OWN input listener + the detector that catches it, transparent, gated)

Zero input-API knowledge assumed — you need your Mac (IOHID), a Linux VM (evdev), and a Windows VM (hooks/raw input), plus module/basics discipline from earlier courses. **The course has one hard design rule, set in unit 0 and enforced by every verification: your listener is TRANSPARENT (visible to the user, logged to a visible file, single-session only, no persistence, consent-documented) and you BUILD THE DETECTOR THAT CATCHES IT in the same course.** "Stealth," evasion, persistence, or capture of anyone's input beyond your own sessions is out by design — the profession that owns this skill is input-security (accessibility engineering, QA capture, EDR detection), and it builds both sides of the table. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/isN-quiz.md`)** — no advance without both. ~2-3h/unit, 10 units + capstone ≈ 6-7 weeks.

Compass (re-read when lost): input is a trust boundary: hardware → driver → OS event system → apps. Every OS exposes read-APIs (evdev, IOHIDManager, hooks/raw input) gated by privileges/TCC — because who can read keystrokes can read everything else. This course owns the API layer honestly (your devices, your sessions) and then the DETECTION layer (what a listener looks like to the OS) — which is the half that turns the knowledge from spyware-shaped into security-shaped.

Safety hardline (the course's constitution, your unit-0 artifact): own hardware/VMs; own keystrokes only; visible logging; single-session; no startup persistence; no evasion; consent note on every demo; snapshots before hook tests; real keyloggers = reading-only (unit IS8). The detector is not a suggestion — it's a capstone deliverable.

---

## IS0 — hello world: the input pipeline and the rules
Concept: input flows hardware → driver → OS events → apps; who may read what is a privilege decision per OS. Do: draw the pipeline for all three OSes (Linux evdev chain, macOS IOHID, Windows hook/raw-input); write `notes/is0-constitution.md`: your transparency rules (above) signed in your own words; a "hello input" program (any OS): read your own keyboard stream raw, print scancodes to a VISIBLE window for 30 seconds, exit.
Verify: constitution written; 30-second visible capture of YOUR typing prints scancodes + exits clean.
**Lesson check:** why is input a *trust boundary* and not just "an API" — and what does "transparency" concretely require of every demo you'll build?

## IS1 — Linux evdev: root of the chain
Concept: evdev gives raw device events (/dev/input/event*); root or input-group grants it; X11/Wayland add another layer above. Do: enumerate your VM's input devices ($(cat /proc/bus/input/devices)), open your keyboard's evdev, decode events (EV_KEY codes, value=1/0), print keysyms mapped from keycodes (layout-aware start); note why root/group is required — document the privilege boundary; transparent mode: visible banner + timed exit.
Verify: evdev listener prints YOUR types key-by-key (mapped to letters) for a timed session; device map documented.
**Lesson check:** what does evdev privilege protect (who else's keys could you read?) — and where does Wayland change the game (per-app capture)?

## IS2 — macOS IOHID: the TCC gate
Concept: macOS gates input via TCC (Input Monitoring permission) + IOHIDManager; user-visible consent is REQUIRED — the OS itself insists on transparency. Do: on your Mac: IOHIDManager setup (keyboard usage page), request Input Monitoring via System Settings (macOS prompts you), read your own events in a Swift/C tool with a visible window title + on-screen text echo; verify the TCC prompt + Settings entry exists for your tool (the OS's transparency story, documented).
Verify: IOHID listener captures your typing with the TCC grant visible in Settings; timed exit.
**Lesson check:** what does macOS's TCC model force that Linux doesn't — and why is that a GOOD design (who does it protect)?

## IS3 — Windows hooks and raw input: the flagged territory
Concept: WH_KEYBOARD_LL + RegisterRawInputDevice read keystrokes; these APIs are AV-flagged for a reason — the flag is part of the lesson. Do: in your Windows VM: a low-level keyboard hook (WH_KEYBOARD_LL, message loop) OR raw input, with a VISIBLE console window + timestamped visible log; run Windows Defender scan — EXPECT the heuristic to flag or monitor the pattern; write the honest finding: "this API use is exactly what malware uses, and the AV reaction is correct" — that's the course's point about why the transparent+detection framing is mandatory.
Verify: listener works visibly; Defender/VT-style heuristics noted in your findings doc.
**Lesson check:** why do Windows input APIs carry a "beware" sign — and what does that sign say about the line you're standing on?

## IS4 — the language of input: keys, layouts, dead keys
Concept: raw scancodes ≠ letters: keycode→keysym→character passes through layout, modifiers, dead keys, compose. Do: build a layout-aware decoder (your own, no library): US layout + one other (e.g., QWERTZ/Vietnamese) + modifiers + dead-key accents; verify accented typing decodes correctly; a little typing-session artifact showing raw → decoded per keystroke.
Verify: decoder matches your typed text exactly for both layouts (accented + shifted keys).
**Lesson check:** why is layout handling the hardest correctness problem in input software — and what breaks when you skip it (and who notices)?

## IS5 — the transparency layer: making yourself visibly present
Concept: the opposite of stealth: your tool proves it's there — window titles, status line, log file, quit button, session timer. Do: build the transparency UI (same tool, all three binds): visible banner with process name/PID, live "capturing: ON" indicator, log written to an obvious path with your name in it, auto-quit countdown, hotkey to stop; demo it on all three OSes; write the consent-note generator (a text file the tool writes: what it captures, why, for how long, your contact).
Verify: transparency layer live on 3 OSes; consent note auto-generated; the tool is its own evidence.
**Lesson check:** what would a stealth tool remove from this checklist — and what does that removal MEAN (who stops knowing, who gets harmed)?

## IS6 — detector half 1: catching a Windows listener
Concept: detection = knowing what a listener looks like: hook chain enumeration, DLL/module presence, ETW/telemetry, behavioral patterns. Do: from EDR-course skills: a Windows detector: enumerate hooks (NtQueryInformationProcess hook list), scan loaded modules for hook DLLs, check raw-input registrants; run it against YOUR OWN unit-IS3 listener while Defender notes its own flags; produce the detection report (what found it, what missed it).
Verify: your detector flags your own IS3 listener in the VM (report with evidence).
**Lesson check:** which artifact of a listener is easiest to find — and why do AVs still rely on heuristics (what can't they see)?

## IS7 — detector half 2: macOS TCC audit + Linux consumers
Concept: macOS: keystroke-access scans (which apps hold Input Monitoring grants), logstream TCC prompts; Linux: /dev/input openers (lsof), evdev consumer enumeration. Do: macOS: TCC grant audit script (plist/DB scan: who has input-access) + flag your own IS2 tool; Linux: lsof on /dev/input/event*, /proc scan for open fds — flag your own IS1 listener; produce the per-OS detection rule set (shell/sigma-lite).
Verify: both detectors flag their own listeners; rule set written in your words.
**Lesson check:** what does each OS's audit surface expose that its hooks hide — and why is "who can see my keys" a question worth a tool per OS?

## IS8 — the adversary, read-only: real keyloggers
Concept: the shapes real keyloggers take (RAT modules, hardware interceptors, kernel callbacks) and what catches them anyway. Do: reading-only unit: study public analyses (malware reports on keylog-capable families, hardware keylogger teardowns) — mechanism, hiding, and the artifact that exposed each; map each to your pipeline diagram (WHERE did it attach?); NO code, NO re-creation of stealth; tie to your Windows-MALWARE M-track reading.
Verify: pipeline-attach map for 3 real examples + "what caught them" column complete.
**Lesson check:** which OS layer do real keyloggers choose to attach at — and what does that say about where DETECTION must also live?

## IS9 — the consent essay: the line, argued
Concept: input capture is legitimate in EXACTLY the cases consent makes it so: accessibility (OS-assisted), QA capture (own session), device research (own hardware), parental tools (disclosed, child's device), enterprise DLP (notice + policy) — and abuse the moment the captured party doesn't know. Do: the 400-word essay: your position on transparency-as-the-line, the notice problem (why "I could have" isn't "I may"), the professional guardrails checklist (notice, scope, storage, deletion, purpose); defend each line in notes.
Verify: essay + guardrail checklist written; you can answer a hostile interview question from it.
**Lesson check:** what single fact, if hidden, turns your transparent tool into spyware — and what does that imply for the tool's default behavior?

## IS10 — CAPSTONE: the two-sided lab, cold
Prereq: IS0–IS9. **Close all notes.** Cold: rebuild ONE OS's listener (transparency layer included) + that OS's detector in one sitting — no notes — then run the round-trip live: capture your session visibly, detect your own capture, produce the report (capture log + detection evidence + consent note + the IS9 essay). Write `labs/input-capstone.md` like a security engineer's: architecture, the transparency design, the detection design, the ethics page.
**Pass = round-trip works cold (capture → detect → report) with the ethics page as your own.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in IS0/IS5 boilerplate (visible-scaffold) — listeners, decoders, detectors, audits written by you; erase-and-retry once when stuck.
3. 2-3h/unit; stuck past that = previous unit's verification again.
4. Constitution-hardline: own devices/VMs, own keystrokes, visible + timed + no persistence + no evasion, snapshots before hook tests, real malware = reading only.
5. Honest bar: real input-security engineering is a profession (accessibility, QA, EDR, DLP) with compliance duties; this course's bar = transparent capture across 3 OSes + the detectors that catch it + an argued consent position, proven cold at the capstone — the floor for input-layer engineering and the exact model behind every "who can see my keys" question you'll ever debug.

## Where this lives
Feeds from EDR-COURSE (detector skills), Windows-MALWARE M-track (read-only adversary shapes), and the accessibility-adjacent systems courses; the pipeline diagram links your driver work (evdev = the device layer you studied in DRIVER/USB courses). Build + detect, same hands, same lab, both sides of the line.