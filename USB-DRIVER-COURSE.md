# USB Driver Course — Absolute-Beginner (hello world → your OWN USB driver moving real data on a virtual bus, gated)

Zero USB knowledge assumed — do `DRIVER-COURSE` V0–V6 first (module/MMIO/DMA/IRQ + KMDF + dext: one trainer device three OSes) and browse phase 22 Embedded/USB steps (09-12). Lab: your Linux VM under QEMU with **QEMU's virtual USB devices** — no physical hardware needed, the USB stack is 100% real: descriptors, endpoints, transfers — driven on a virtual bus. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/usN-quiz.md`)** — no advance without both. ~3h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): USB is **descriptors + endpoints + transfers**: a device describes itself (device → configuration → interface → endpoint), transfers are four kinds (control for setup, bulk/interrupt for data, isochronous for time-sensitive), and a driver is "bind to an interface, speak its endpoints, live its lifecycle (probe/remove/suspend)". The genius of QEMU: you can `-device usb-tablet` / `usb-serial` / `usb-storage` and get real device behavior with zero dead hardware. Everything you build maps OS-by-OS — Linux URBs, IOKit families, KMDF/winusb — the structure is constant, the APIs cycle.

Safety: lab VM discipline only (snapshots, own device stubs); no physical-device risk, nothing to brick. Rules like every course: verification + own-words quiz gate each unit; copy only US0/US10 boilerplate (skeleton, bench harness); erase-and-retry once when stuck; honest ceilings per OS (documented where VM/reality blocks full runs).

---

## US0 — hello world: the USB model
Concept: host ↔ device over a bus: the stack (device, bus, HCD, core, class drivers), descriptors tree, four transfer types. Do: in VM: `lsusb` inventory (your qemu virtual devices), `lsusb -t` bus tree, read the USB 2.0 spec's device-framework chapters (descriptors + transfers); draw the full map: your device's place from HCD to userspace.
Verify: bus tree + descriptor-tree diagram produced; you name each transfer type's use-case.
**Lesson check:** why is everything *descriptor-driven* — and what does a driver actually bind to (device? interface? endpoint)?

## US1 — descriptors by hand: read a device cold
Concept: descriptors are byte layouts (bLength, bDescriptorType, ...); reading them by hand is the USB literacy test. Do: a userspace descriptor dumper (libusb basics only for access) that walks your qemu device's descriptors RAW and prints fields; verify field-for-field vs `lsusb -v`; list your device's endpoints + types + maxpacket.
Verify: your dumper matches lsusb -v on all fields; endpoint table produced.
**Lesson check:** walk one configuration — what four structural levels describe a device, and which does a client driver bind?

## US2 — the Linux model: driver meets device
Concept: Linux: usb_driver + probe + interface; the interesting moment is probe: who owns this device? Do: write your Linux USB driver skeleton binding to ONE qemu virtual device (start with `usb-serial` or `usb-tablet`: parameterized vendor/product), log probe/remove, bind it (module + sysfs binding discipline); verify `lsusb -t` shows driver ownership; swap binding with the stock driver back and forth.
Verify: your driver owns the interface (lsusb/readlink in sysfs proves it); probe logged per attach.
**Lesson check:** what does probe receive — and what makes the device yours vs the stock driver's (who decides, and how)?

## US3 — URBs and transfers: bytes actually move
Concept: URBs (USB Request Blocks) = async transfer + completion callback; control (setup) vs bulk/intr endpoints. Do: implement control transfer (get descriptor / set config) via your own URB + completion; then bulk reads on the virtual serial's bulk IN endpoint; write a tiny userspace byte pump; verify with `usbmon` traces (your URBs visible, endpoint + data match).
Verify: control + bulk transfers complete correctly; usbmon shows YOUR URBs; bytes round-trip.
**Lesson check:** what is a URB (what holds the data, when does it complete) — and why async over sync in the core?

## US4 — lifecycle: probe, remove, hotplug
Concept: devices come and go; drivers must live that truth (suspend/resume, disconnects mid-transfer). Do: robust remove (pending URBs killed, buffers freed — the teardown discipline), suspend/resume handlers, then LIVE hot-unplug: `device_del`/reattach in qemu monitor while your driver is mid-read — NO crash, clean teardown; error paths logged; re-plug re-probes cleanly.
Verify: hot-unplug mid-transfer handled without panic; re-plug re-probes; teardown log clean.
**Lesson check:** what's the actual danger of a mid-transfer detach — and why is teardown-order a correctness bug class, not a style issue?

## US5 — one class, fully: choose virtual-serial or HID
Concept: class drivers implement a protocol on top of raw USB (HID reports, CDC-ACM framing). Do: pick one: HID (parse your tablet's report descriptor, read reports) or serial (CDC-ACM: negotiate set-line-coding via control transfers, then bulk data end to end); implement the minimal protocol side; a userspace test tool proves end-to-end bytes (typed into the qemu console / read back). 
Verify: your class implementation moves real reports/bytes end-to-end; protocol handling documented.
**Lesson check:** what does a class spec ADD over raw endpoints — and where did the "college/university" of your chosen protocol show up in the byte stream?

## US6 — macOS IOKit: same bus, different family
Concept: IOKit: IOUSBHostDevice/Interface, USER clients, families (IOUSBHostFamily); descriptors are the same bytes. Do: reading unit: map your Linux structures ↔ IOKit (usb_driver ↔ IOUSBHostInterfaceDriver? probe ↔ probe/camera?) — write the mapping table from the DRIVER-COURSE dext foundation; build an IOKit USB *client* skeleton (compiles, class registration, descriptor walk in code); honest ceiling: full in-kernel IOKit USB driver needs real Mac + device — document the fallback (client-layer proof + mapping).
Verify: mapping table + compiling IOKit USB client skeleton + ceiling doc written.
**Lesson check:** where do IOKit's families sit relative to Linux's usb_driver — and what does the USER CLIENT boundary protect?

## US7 — Windows KMDF/winusb: the client-driver route
Concept: Windows: kernel KMDF USB client drivers; but the pragmatic route = winusb (userspace speaks the device). Do: from DRIVER-COURSE KMDF skills: build the KMDF USB client *skeleton* (KMDF_USB_DEVICE_* usage skeleton, builds); then the honest winusb path: a winusb app (C) that opens your qemu guest's virtual USB device and round-trips bytes (Windows guest under qemu with the same `-device` set); verify bytes on both sides; document when KMDF-kernel is truly needed.
Verify: winusb app round-trips with the qemu device in the Windows guest; KMDF skeleton documented (build-only is fine, honest).
**Lesson check:** why does Windows make userspace-first (winusb) viable — and what pushes real devices into kernel KMDF territory?

## US8 — Android gadget lane (reading+lite)
Concept: Android phones are usually DEVICES (gadget mode): the gadget stack exposes functions over a physical USB port. Do: read the gadget stack (configfs functionfs, android gadget drivers); attempt a functionfs setup in a Linux VM emulating device-mode if your qemu supports it — else the honest doc: diagram of gadget vs host mode, where your host-side skills land, why phone-as-device is its own world.
Verify: gadget-mode diagram + (attempt or ceiling) doc written.
**Lesson check:** what flips when your machine is the *device* — and which of your host-side concepts survive the flip?

## US9 — the security lens: USB is trusted wire
Concept: USB's attack surface is rich (HID spoofing, firmware injection, serial injection); a driver author is an attacker AND a gatekeeper. Do: read the USB-attack classics (USB HID spoofing/badusb, device firmware attacks, USBGuard class mitigations); implement YOUR attach-policy enforcement: udev/allowlist + your driver's reject-list (refuses unknown VID/PID or unmatched policy) on the VM; table what your policy blocks vs the stock permissive path; write detection notes for your own lab.
Verify: policy blocks a "rogue" qemu device you add (proven live); detection notes written.
**Lesson check:** why is USB a pernicious boundary — and what does an attach-policy actually enforce (identity? capability?)?

## US10 — correctness and throughput: buffers, timeouts, isoc
Concept: transfers cost: buffer allocation/DMA, timeouts, isochronous scheduling — the driver's performance-and-correctness surface. Do: DMA-safe URB buffers (dma_map/coherent), transfer timeouts + cancels, retry-on-error clean; isoc READING (scheduling model — why it exists for audio/video); benchmark your virtual-serial throughput (your US5 path) with 3 buffer-size configs (honest table + bottleneck note).
Verify: timeout/cancel paths tested (kill mid-wait); bench table produced.
**Lesson check:** what do timeouts protect against that a robust driver must assume — and why is isochronous a schedule, not a queue?

## US11 — the ecosystem: udev, usbfs, gadget, and the pipeline
Concept: hardware is only half; the device-to-userspace pipeline and OS ergonomics are the craft. Do: your end-to-end pipeline doc: your driver (kernel) → device node (cdev/class) → udev rule → userspace tool (the whole chain from physical-ish bus to app); browse phase 22's 09-12 embedded/USB steps and mark which you've now out-built; UAC1/UVC/vendor-class reading as future-class notes.
Verify: pipeline doc + phase-22 mapping table written; your kernel→userspace chain demonstrated live.
**Lesson check:** why does a driver's job include node creation and udev — and what's the difference between "works in /sys" and "works for users"?

## US12 — CAPSTONE: your USB driver ships, cold
Prereq: US0–US11. **Close all notes.** Fresh qemu VM: cold-reconstruct your chosen class driver's core (probe → control → bulk/intr loop → teardown) in one sitting, then finish v1: lifecycle-complete, guarded teardown, policy hook, node + udev, bench notes — data round-trips end-to-end live. Write `labs/usb-capstone.md` like a driver maintainer: design, per-path diagram, OS matrix (Linux proven + IOKit/KMDF mapped + ceilings honest), three proud decisions, regret, roadmap (isoc, gadget mode, real hardware day).
**Pass = your driver moves bytes end-to-end cold; writeup reads like a maintainer's, ceilings included.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in US0/US10 boilerplate (skeleton, bench harness) — descriptors, URBs, protocols, teardown written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Own VM lab; snapshots before hotplug experiments; the stock driver is your control, not your enemy.
5. Honest bar: real USB drivers (Intel/Apple/HID vendors) are team-years; this course's bar = your own driver moving real bytes through the whole USB stack, lifecycle-tested, policy-guarded, mapped across three OS families, cold-rebuilt at the capstone — the floor for device-driver work and the model behind every USB thing you plug in.

## Where this lives
DRIVER-COURSE V0–V6 is the floor; phase 22's embedded/USB steps (09-12) acknowledge this course's depth; NETWORK-DRIVER'S ring/offload thinking carries into USB's transfer model; the security lens feeds EDR/detection lanes. One bus, one busmaster, one pair of hands.

Device-side twin — the same wire from the other end: your own USB gamepad firmware enumerating on every major OS: [`USB-GAMEPAD-COURSE.md`](USB-GAMEPAD-COURSE.md) UG0–UG12.