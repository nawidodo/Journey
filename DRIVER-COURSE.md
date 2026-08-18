# Kernel Driver Course — Absolute-Beginner (hello world → the same driver on Linux, macOS, Windows, gated)

Zero driver knowledge assumed. You need: a Linux VM (QEMU/UTM), your Mac (for the Apple units), a Windows VM (eval), and a trainer device that exists in all three worlds — **QEMU's "edu" educational PCI device** (free, in QEMU, exercises MMIO/DMA/IRQ like a real card). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/vN-quiz.md`)**. No advance without both. ~2–3h/unit, 12 units + capstone ≈ 7–9 weeks. Feeds Phase 14 (Cross-OS Driver Dev) directly and pre-warms the kernel-exploit courses (Linux K7/K8, Windows N7/N8).

Compass (re-read when lost): a driver is **a program the kernel runs on hardware's behalf**: the OS hands it a device (probe), it maps memory (MMIO), moves data (DMA), answers interrupts (IRQ), and exposes control (a file or service) to userspace. Every unit answers: "how does THIS OS spell 'hello hardware'?" — and the attack surface is exactly the seams: parsing input, DMA races, IRQ handling.

Safety: VMs for Linux/Windows (KVM/QEMU/UTM; snapshots — kernel panics are normal here, restore is free); macOS units on your own Mac only (DriverKit dext, no kext needed on Apple Silicon; system settings may need a reboot for driver acceptance); test-signing only; drivers talk to the edu trainer or nothing real — never touch production hardware you don't own 100%.

---

## V0 — kernel module hello (Linux, because it's open — you can read the rules)
Concept: a kernel module = code the kernel loads and runs in kernel mode; the oldest hello in the driver world. Do: complete the 04-03 module lab: `hello_world` init/exit, Makefile (kernel build dir), `insmod`/`rmmod`, `dmesg` shows your strings; `lsmod` lists yours; try crashing it (module that dereferences a bad pointer) → kernel oops → `dmesg` backtrace read → snapshot restore.
Verify: your module loads/unloads with dmesg output; you read one oops correctly.
**Lesson check:** user mode vs kernel mode — what can your module do that a userspace program cannot, and what happens when it misbehaves?

## V1 — kernel contexts: params, /proc, sysfs
Concept: drivers configure via module params, expose state via /proc or sysfs (the kernel's filesystem API), and respect locking. Do: add a `module_param` (int/string) set via insmod arg and `/sys/module/.../parameters`; create a `/proc/hello` entry your userspace can read; add a spinlock-protected counter incremented from two contexts.
Verify: param settable both ways; /proc entry readable; counter correct under your own two-thread test.
**Lesson check:** why do drivers expose state through pseudo-files — and what locking problem does a shared counter immediately show you?

## V2 — your first character device
Concept: the driver as a file: open/read/write/release handlers serving userspace. Do: chardev driver: allocate device numbers, `cdev` + `class_create` + `device_create` → /dev/hello appears (udev); implement read (returns a string) and write (stores bytes); userspace test program opens/reads/writes it.
Verify: your userspace test drives your device file end-to-end; `cat /dev/hello` prints your string.
**Lesson check:** the file API as the driver boundary — what does each of open/read/write map to inside the kernel?

## V3 — buses and discovery: platform device + device tree
Concept: kernels don't hardcode devices — they discover them (device tree on ARM/embedded, ACPI/PCI elsewhere) and call probe. Do: platform-driver lab (04-04 prewarm): declare a platform device in a device tree overlay (QEMU virt machine), your driver's `probe` runs on match; add `devicetree` compat string; print the resource passed in.
Verify: probe fires from the tree entry; you can rename the node and see probe stop.
**Lesson check:** discover-vs-hardcode — why did kernels move to "match + probe", and what does the matching key (compatible string) actually compare?

## V4 — memory-mapped I/O and the "edu" trainer
Concept: devices live at memory addresses (MMIO); drivers map them and read/write registers. Do: QEMU edu device (add `-device edu` to your VM command): use `ioremap` (or modern `devm_ioremap`), read the edu device's ID/info registers (`readl`), write a calculation register, read the result back — your driver just "did math" on a real (virtual) card.
Verify: edu ID read matches the spec; your readl/writel math returns correct results; captured in `labs/`.
**Lesson check:** what does ioremap do, and why can't a driver just dereference the device address directly on most systems?

## V5 — DMA: the "zero-copy" truth
Concept: devices want memory directly (DMA), but must know the physical addresses and the buffer must not move — the DMA API's job. Do: extend your edu driver: allocate a DMA buffer, fill it, trigger the edu's DMA write transfer (`edu` docs' DMA register set), verify contents in the buffer after the transfer completes; read about streaming vs coherent mappings.
Verify: edu DMA transfer writes your data correctly (hash-compare before/after).
**Lesson check:** why does the CPU copy data to the device (PIO) — and what's the correctness risk DMA introduces (address space, cache, ownership)?

## V6 — interrupts: stop waiting, get called
Concept: devices signal completion via IRQ; drivers defer real work (tasklet/workqueue/threaded IRQ). Do: on edu: enable interrupts (`IRQ_STATUS` register), have the device raise an interrupt on DMA completion, your handler fires (printk + a counter), defer the heavy resume part to a workqueue; verify you can differentiate spurious vs real interrupts.
Verify: IRQ counter increments on each transfer; prior workqueue path logged.
**Lesson check:** why interrupt-driven beats polling — and why must heavy work move OUT of the handler?

## V7 — macOS: the DriverKit debut
Concept: Apple's modern driver story = DriverKit (dext), userspace-hosted, no kexts on Apple Silicon. Do: complete the 14-04 dext lab: a "hello" dext (C++/DriverKit SDK) with an IOKit user client; a small macOS app that connects and reads a string from your dex service; load it, verify with `ioreg`/Console logs.
Verify: your app talks to your dext service on your Mac; `ioreg` lists it.
**Lesson check:** how does a userspace-hosted driver (dext) change the "kernel module" mental model from V0 — and what stays the same (service, clients, I/O)?

## V8 — macOS deeper: IORegistry and a data ring
Concept: drivers publish into IORegistry (the device tree of Apple world); data flows via user-client methods or shared memory. Do: extend your dext: publish a property, read it from your app via IORegistry; pass a small data buffer app→dext→processed→app through a method that echoes transformed bytes.
Verify: IORegistry property visible; echo round-trip works from your app (hash-matched).
**Lesson check:** IORegistry vs userspace app — name the registry's role and why user-client methods are the controlled door into a driver.

## V9 — Windows: the KMDF hello
Concept: Windows driver frameworks — KMDF (kernel) managing the boilerplate; loading = service + test-signing. Do: complete the 14-03 KMDF lab: non-PnP "hello" KMDF driver (compile with WDK), install as service in test-signing VM, `sc start hellodrv`, see your strings in DebugView; then extend with a device interface so a userspace app can open it.
Verify: driver starts/stops via sc; DebugView captures your messages; app opens your device interface.
**Lesson check:** what does a driver framework (KMDF/WDF) do for you — and what does Windows require before your driver even runs (signing, service)?

## V10 — Windows deeper: the SAME edu device, now on Windows
Concept: a real PCI driver pattern on Windows: find your device, map BAR, read/write registers, handle IRQ — the exact V4–V6 work, new spelling. Do: edu PCI device `-device edu` on the Windows VM (test mode signing on): KMDF PCI driver, device match on edu's vendor/device ID, probe→BAR mmio (read ID/write calc regs), DMA+IRQ via WDFDMAENABLER + interrupt callbacks.
Verify: Windows driver reads edu ID correctly; register math works; IRQ fires on DMA completion (evidenced).
**Lesson check:** map each V4–V6 concept to its WDF name (MMIO→MappedBase, DMA→DMA enabler, IRQ→EvtInterrupt)— and say what "the framework" absorbed that Linux made you write by hand (V5).

## V11 — the security lens: your driver's bugs are the kernel's bugs
Concept: every driver seam = a potential kernel bug; the cheapest lesson is testing your own. Do: on your Linux chardev (V2): write userspace "fuzz-lite" — throw random/structure-corrupting input at read/write for an hour; find the crash; fix it (bounds + validation); then read the HEVD-style vulnerability classes (N8's ladder, Phase 14-05/06) and map them to seams in YOUR code; write your hardening checklist.
Verify: a real bug found+fixed with before/after; checklist in `notes/v11.md`.
**Lesson check:** name three driver vuln classes and the exact seam in your V2/V4 code where each would live.

## V12 — CAPSTONE: same device, three OSes, one writeup
Prereq: V0–V11. **Close all notes.** Re-create: the edu driver working on Linux, macOS (dext equivalent), and Windows (KMDF) — you've already built each; now bring them to one clean state and add the comparison: `labs/drivers-capstone.md` — model table (module/kext-vs-dext/KMDF), loading+signing flow, API surface, and **attack surface** (which seams differ per OS, where you'd look first as an attacker per platform). Re-build the Linux driver from scratch, cold, as the proof.
**Pass = edu works on all three OSes from your code; capstone table reads like a hiring-interview answer.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in V0/V9 boilerplate (Makefile shape, WDF skeleton) — every driver body written from concept; erase-and-retry once when stuck.
3. 2–3h/unit; stuck past that = previous unit's verification again.
4. VMs + snapshots always (panics are normal, restore is free); macOS dext on your own Mac only; test-signing only; the edu trainer or nothing — never real hardware you don't own.
5. Honest bar: shipping drivers are hard (locking, power, certification); this course's bar = the same trainer device driven by code YOU wrote under three OS models, plus a written attack-surface comparison — the floor for kernel/driver work on all three platforms, proven cold at the capstone.

## Where this lives
`steps/` unchanged (route: 04-03/04, 14-01..08). The kernel-exploit courses (K7/K8, N7/N8) and this course are two sides of one coin: yours is the build side; they are the break side — finish both and you own the whole seam.

Network NIC variant — the same driver lab, rings+DMA+NAPI+offloads on QEMU virtio-net: [`NETWORK-DRIVER-COURSE.md`](NETWORK-DRIVER-COURSE.md) ND0–ND12.

USB-bus followup — the same lab, descriptors/endpoints/URBs on QEMU's virtual devices: [`USB-DRIVER-COURSE.md`](USB-DRIVER-COURSE.md) US0–US12.