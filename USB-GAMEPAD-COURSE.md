# USB Gamepad Course — Absolute-Beginner (hello world → YOUR OWN USB game controller that works on every major OS, gated)

Zero USB knowledge assumed — you need a dev board with USB device support (RP2040 (Pico) or Teensy-class recommend; a Pico is $5 and runs tinyusb — the board-agnostic USB stack), a breadboard + thumbstick/buttons, and the four major hosts (your Linux VM, your Mac, a Windows VM, an Android device/emulator — all USB-host capable). The course builds the DEVICE side (your firmware presents a gamepad) and the HOST side (each OS reading it), plus the VIRTUAL lane (your PC's kernel emulating a gamepad itself). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/ugN-quiz.md`)** — no advance without both. ~3h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): a USB gamepad is a **HID class device**: your firmware declares a REPORT DESCRIPTOR ("I have 2 axes, 12 buttons, 4 hats") in a tiny byte language, then streams REPORT PACKETS on an interrupt IN endpoint. Any host that speaks HID — every major OS does — sees your stick. The mastery ladder: descriptor (the vocabulary) → device stack (the plumbing) → input capture + calibration (the feel) → host-side reads per OS (the five dialects) → virtual device (your OS pretending to be the pad) → latency (the gamer's judge). You'll hold a real $5 board and watch four operating systems recognize YOUR hardware as a controller.

Hands-on rules: optional-add-on realism — solderless breadboard builds fine; when a host capability is hardware-walled (macOS virtual-HID driver, Windows driver signing), that's a documented ceiling, not a cheat — the course teaches what the wall is and the legit alternative lane.

---

## UG0 — hello world: what a HID gamepad IS
Concept: USB classes; HID = human-interface device with descriptors + reports; a gamepad is just a HID device declaring axes/buttons. Do: read the HID spec's gamepad-relevant pages (usage pages, report structure); install your board toolchain; hello: blink your board's LED from your own code (or the board's documented demo — toolchain proof); list what a "gamepad" must declare (the shopping list: axes, buttons, hats — write your target descriptor plan).
Verify: board blinks via your toolchain; descriptor-plan written (axes count, buttons, hats).
**Lesson check:** what does "HID class" mean at the USB level — and what exactly does a REPORT DESCRIPTOR declare that makes a host call you a gamepad?

## UG1 — the report descriptor, written by you
Concept: descriptor = report structure in usage-page tokens: axes as X/Y/Z usage, buttons as 1-bit fields, hats as 4-directional; hosts parse it to map your bytes. Do: write YOUR gamepad report descriptor (1 stick 2-axis + 12 buttons + 1 hat — or your plan's numbers); validate with a descriptor checker (UsbHid descriptor tool or `hidrd`-style preview); enumerate: plug a bare-enumeration build and show dmesg/System-Report/Wireshark-usb seeing a HID device (even before reports flow).
Verify: descriptor validates; board enumerates as HID on at least one host with descriptor visible.
**Lesson check:** what does each descriptor field (Usage, Report Size, Logical Min/Max) control — and when do hosts reject a malformed descriptor (what breaks first)?

## UG2 — the device stack: endpoints and polls
Concept: device needs a stack: tinyusb on your board — HID device config, IN endpoint interrupt, bInterval (poll rate); CDC-debug for logs. Do: enable tinyusb HID device class on your board; IN endpoint at 1 ms interval (1000 Hz nominal); a status LED toggling per report sent; add CDC serial debug and print report-send counts; measure your ACTUAL report rate (counter over 1 s — honest hardware number).
Verify: board sends reports at a measured interval; debug stream shows counts; endpoint/interval config documented.
**Lesson check:** what does bInterval actually promise (and not) — and why interrupt-endpoint rather than bulk for HID?

## UG3 — sticks: ADC, calibration, deadzones
Concept: analog axes: ADC reads → range mapping (0–65535) → centering/deadzone → report values; feel = calibration quality. Do: wire a thumbstick (X/Y pots) to ADC pins; convert ADC→axis with YOUR mapping; calibration routine (read min/max/center yourself; store in flash); deadzone + response curve (linear vs curve — pick, justify); live report: a debug tool prints axis bytes as you move the stick.
Verify: axes track your stick (min/center/max correct); calibration + deadzone implemented + demonstrated.
**Lesson check:** why does raw ADC → report map need calibration — and what happens to a game when center-drift goes unreported (which game mechanic breaks)?

## UG4 — buttons and hats: debounce and report assembly
Concept: digital inputs: matrix/GPIO scan → debounce (spurious presses) → button states packed into report bits; hats as 8-way states. Do: wire 8+ buttons (matrix or direct GPIO); debounce in YOUR code (time- or edge-based; measure bounce on a scope/log — honest); assemble the full report packet (axis bytes + button bits + hat) per your descriptor's layout; a live gamepad viewer (your own small host tool prints every field).
Verify: buttons report clean (no ghost presses in a 1000-press test); packet layout matches descriptor 1:1; viewer prints all fields.
**Lesson check:** what is debounce fighting at the physical layer — and why must the REPORT BYTES exactly match the DESCRIPTOR'S bit order (what breaks with a mismatch)?

## UG5 — host 1: Linux, the joyful default
Concept: Linux joydev: your HID pad becomes /dev/input/jsX; the kernel does HID→evdev→joydev; games read the js API. Do: attach the board to your Linux VM; confirm jsX + `jstest` showing axes/buttons; write your OWN host-side reader (open /dev/input/jsX, parse events) printing stick motion; then the reverse surprise: Linux uinput — your PC EMITS a joystick for games (module uinput) — write a uinput emitter that sends your firmware's exact packet pattern.
Verify: jstest + your reader both show the pad; uinput emitter visible as a virtual joystick.
**Lesson check:** what layers does Linux interpose between your descriptor and a game — and why is uinput a "device in software" (what kernel trick is that)?

## UG6 — host 2: macOS
Concept: macOS: IOKit HID (IOHIDManager) reads any HID device; GCController/Controller Services (the game-facing layer); no special driver needed for standard HID pads. Do: on your Mac: attach the board; IOHIDManager app (or the built-in Game Controller pane) sees your pad; write a small IOKit HID reader printing axes/buttons; how GCController maps standard HID pads (P1/P2 returns) as reading; note the divergence: what your custom descriptor lacks for full GCController support (the honest field-format gap).
Verify: macOS shows and reads your pad (IOHIDManager reader works); GCController-mapping gap documented.
**Lesson check:** what does IOKit do that Linux's joydev doesn't have to — and why does Apple want STANDARD HID usage layouts (what does the framework assume)?

## UG7 — host 3: Windows
Concept: Windows: Raw Input (WM_INPUT) reads generic HID pads; XInput is a SEPARATE path (Xbox-class) — many titles only speak XInput; your HID pad needs RawInput apps or a wrapper. Do: in your Windows VM: attach the board; Device Manager shows HID-compliant game controller; RawInput reader app (yours) prints your report; the XInput wall: write the test — a RawInput-only title works, an XInput-only title refuses (document it); the legit bridge: XInput emulator READING (how wrappers map HID→XInput, why Microsoft gates it).
Verify: RawInput reader works; XInput-vs-HID refusal demonstrated + documented; wrapper reading noted.
**Lesson check:** why does Windows have TWO gamepad worlds — and what does the XInput/HID split teach about platform lock-in (who decided, and what won)?

## UG8 — host 4: Android and the mobile lane
Concept: Android: USB HID host API (UsbHostManager) reads HID devices; mobile contexts: pads for emulators, cloud gaming. Do: Android device/emulator with OTG: your board attached; your Kotlin app: UsbManager + HID endpoint read → prints the same fields; the "keyboard/mouse HID" alternative (Android's InputManager handles standard HID input classes — gamepads land in InputManager on some versions; read the mapping); a test: your pad drives a simple on-screen reticle in your app.
Verify: Android app reads your pad (fields printed); reticle tracks the stick.
**Lesson check:** what does Android's HID host path share with Linux's (and not) — and why does the mobile world prefer BLUETOOTH pads over USB (what does the transport change)?

## UG9 — latency: the gamer's judge
Concept: input latency = firmware scan → report → host parse → app event; measured honestly: loopback rig (switch/button to GPIO; LED-on-event; camera-free TOF-ish timing via debug timestamps). Do: build the latency rig: press state → host app sees it, timestamps both sides (firmware send-tick vs host receive-tick); measure per OS (Linux/macOS/Windows/Android) the same physical press; the table; two optimizations (poll interval vs interrupt timing; host-event skipping) measured.
Verify: per-OS latency table (same rig, honest method); optimization deltas demonstrated.
**Lesson check:** where does your measured latency actually accumulate — and why does 1000 Hz polling not automatically mean 1 ms end-to-end (what's between)?

## UG10 — the virtual lane: your OS as the gamepad
Concept: the product trick: virtual gamepads (software emits the same HID/event stream) — used by remappers, accessibility, test harnesses. Do: complete the virtual lane: Linux uinput pad (from UG5) + a Windows virtual-HID path (ViGEm-style library READING + a signed-driver reality wall documented; alternative: a RawInput-compatible shim app) + the macOS ceiling (no public virtual-HID API — documented with workarounds); a live test: your uinput pad drives a real game/app in the Linux VM; the matrix of what virtual-emits what per OS.
Verify: uinput pad drives a game; Windows/macOS lanes documented with their walls; matrix complete.
**Lesson check:** what does a virtual device share with your firmware pad (the kernel-side contract) — and why do platform walls differ so wildly on VIRTUAL HID (who controls input trust)?

## UG11 — beyond one pad: wheels, joysticks, composite devices
Concept: the descriptor is a language: extend to multi-axis wheels, throttle/brake, force-feedback (output reports), composite (gamepad + keyboard). Do: write descriptors for: flight joystick (2 axes + throttle + buttons + hat), racing wheel (steering + pedals, 360° logical range), force-feedback HID (output report + effect channels as READING — implement the descriptor, note the size); composite device (2 HID interfaces) on tinyusb — if your board has the USB endpoints; pick TWO beyond-pad descriptors and enumerate them on Linux.
Verify: two extended descriptors enumerate + read on Linux; composite attempt documented (endpoint reality).
**Lesson check:** what stays constant across all HID devices (the contract) — and what does a FORCE-FEEDBACK device add to the DIRECTION of reports (what changes)?

## UG12 — CAPSTONE: one firmware, four hosts, cold
Prereq: UG0–UG11. **Close all notes.** Cold: rebuild the firmware's core (descriptor + report builder + one input source) in one sitting, then: attach the SAME board to Linux + macOS + Windows + Android and get a field-reading on each (your minimal host reader, cold-written if more than a trivial copy); one uinput virtual pad driving a game; the latency table re-run. Write `labs/usb-gamepad-capstone.md`: architecture (device stack + descriptor + report flow diagram), the four-host read matrix (with walls), latency methodology + table, feature map (how your pad fell short of a commercial one), three proud decisions, regret, roadmap (force-feedback, wireless, driver-signed virtual lane).
**Pass = your board reads as a gamepad on all four hosts cold; the virtual lane and latency table stand.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in UG0/UG1 boilerplate (board toolchain, descriptor-checker use) — descriptor, stack config, calibration, readers, virtual emitters written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Honesty rules: measured numbers are measured; platform walls are walls (documented, not faked: macOS virtual-HID, Windows signing); any external library beyond tinyusb gets a note on why-it-earned-its-place.
5. Honest bar: shipping game controllers pass thousands of hosts, certification (USB-IF), and years of feel-tuning; this course's bar = one physical board reading as a gamepad on four major OSs, one virtual lane driving a game, latency measured and improved, proven cold at the capstone — the floor for USB HID device engineering and the complete anatomy of every controller you've ever held.

## Where this lives
The DEVICE-side twin of USB-DRIVER-COURSE (host-side drivers) — one pair of hands on both sides of the same wire; 22-Embedded-USB steps (firmware, signals) are the predecessor lane; HID knowledge feeds your INPUT-SECURITY pipeline maps; the virtual-lane walls (input trust) are the same walls ROOTKIT-DEFENSE and EDR courses teach blue-side.