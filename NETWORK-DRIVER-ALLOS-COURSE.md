# Network Driver × All-OS Course — followup (your Linux virtio driver, re-spelled for every major OS, gated)

Prereq for the deep half: complete `NETWORK-DRIVER-COURSE` ND0–ND12 — you own the ring/DMA/ISR skeleton once; this course makes you re-read it in other dialects. Lab: your QEMU farm with virtio-net everywhere (Linux done; add FreeBSD and Windows eval VMs; macOS VM legality is gray — that lane is reading + compiling skeletons only, documented honestly). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/osnN-quiz.md`)** — no advance without both. ~3h/unit, 8 units + capstone ≈ 5-6 weeks.

Compass (re-read when lost): every OS's network driver expresses the SAME skeleton you already built — attach/driver init, a transmit path, a receive path with deferred work (NAPI/taskqueue/DPC/workloop), DMA, and a teardown — behind different APIs: Linux `net_device`+`ndo_start_xmit`, FreeBSD `ifnet`, Windows NDIS miniport (`MiniportSendNetBufferLists`), macOS `IONetworkController`. The skill this course builds: **read any OS's NIC driver and see your own rings.** One OS gets a full write (FreeBSD — the friendliest second dialect); Windows/macOS get compile-level skeletons + Microsoft/Apple sample maps (real hardware + signing + entitlements are honest ceilings, documented per row).

Safety: lab VMs only; macOS lane is reading/skeleton (license reality); Windows eval VMs fine. Rules like every course: verification + own-words quiz gate each unit; copy only OSN0/OSN8 boilerplate (skeleton, bench harness); erase-and-retry once; honest per-OS ceiling tables.

---

## OSN0 — the farm and the matrix
Concept: one model, five dialects: netdev/ifnet/NDIS/IONetworkFamily + Android's Linux core. Do: build the VM farm (Linux+FreeBSD+Windows eval, each with virtio-net); write the OS stack-map table (driver API, packet envelope type, deferred-work mechanism, DMA API per OS — skb/mbuf/NET_BUFFER/IOEthernet represents packet; NAPI/taskqueue/DPC/workloop does the deferral); inventory each via `lspci`/`devinfo`/Device Manager.
Verify: 4-column stack map produced; every VM sees its virtio-net device.
**Lesson check:** which three concepts survive EVERY dialect (name them) — and which is the one that genuinely differs (and why)?

## OSN1 — FreeBSD: the second full dialect
Concept: ifnet + driver attach + bus_dma + taskqueue/ITHREAD: closest kin to Linux without being identical. Do: study FreeBSD's existing virtio-net driver structure (OpenBSD/FreeBSD source reading), then write YOUR lite ifnet driver: attach, ifmedia, transmit path, RX through a taskqueue, ioctl basics; ping across the FreeBSD VM (host sends, YOUR driver moves it); teardown clean.
Verify: ping works through YOUR FreeBSD driver; attach/detach logs clean; bus_dma usage documented.
**Lesson check:** what's genuinely nicer in ifnet vs net_device — and what did you have to unlearn (or re-learn) to switch?

## OSN2 — Windows NDIS: miniport read + skeleton
Concept: NDIS 6: NDIS provides the abstraction; a miniport implements MiniportXxx entry points, transfers via NET_BUFFER_LIST (NBL), defers via DPC. Do: read Microsoft's NDIS 6 miniport model + the virtual-miniport sample; write YOUR miniport skeleton (buildable project: device entry, miniport init, send/receive stubs with NBL handling) in the Windows VM build env; honest ceiling writeup: binding to qemu's virtio net in Windows = NDIS driver + INF signing — document where that stops (SDK sample mode is the reachable summit here).
Verify: miniport skeleton builds + loads in the eval VM (or failure-at-mapped-step documented precisely); NBL flow diagram written.
**Lesson check:** what does NDIS give the miniport (vs Linux's do-it-yourself) — and why does the Windows ceiling (signing/FFU) exist at all?

## OSN3 — macOS IONetworkFamily: skeleton + the real map
Concept: IONetworkController + IOEthernetInterface: IOKit's network families; Apple's e1000/ene1000 DER samples are the canon. Do: read the DER sample set (the closest Apple ships to a sample driver); compile your IONetwork skeleton (controller class, publish interface, transmit/receive methods) off-hardware; the honest line: real MAC needs hardware + signing + often entitlement-free basic kexts — write the ceiling doc + the "if I had a Mac" roadmap (what you'd attach where).
Verify: skeleton compiles; DER-to-your-ring mapping table (where's the ring? the workloop? the buffer?) completed.
**Lesson check:** what does IOKit demand of a network controller that the other three don't — and why does the workloop exist instead of raw interrupts?

## OSN4 — the one-model-many-apis chapter
Concept: the synthesis: your Linux rings in every dialect. Do: build the "ring of truth" table: for each OS column write WHERE the descriptor ring lives, WHAT object wraps a packet, WHERE DMA is set up, WHO schedules deferred RX, WHO tears down; then a side-by-side of YOUR ND-code lines mapped to each dialect's call (e.g., `napi_schedule` ↔ `taskqueue_enqueue` ↔ `NdisMQueueDpc` ↔ `IOWorkLoop`).
Verify: full table + your-ND-annotated copy per OS (three annotated listings in notes).
**Lesson check:** which mapping was hardest — and what does that difficulty tell you about the OS's core philosophy?

## OSN5 — envelopes and offloads per OS
Concept: packets travel in envelopes (skb/mbuf/NBL/IOEthernetBuffer) with different metadata; offload hooks differ (checksum/TSO/GRO per API). Do: envelope-copy matrix (what each envelope carries, what's header-touched vs headroom); offload hook map (where each OS exposes checksum/segmentation negotiation); write your software-checksum fallback (ND6's RFC1071) adapted to mbuf (FreeBSD) — compile + unit-test on the mbuf envelope in your FreeBSD VM.
Verify: matrix + mbuf-checksum test green; offload hook map documented.
**Lesson check:** why do envelopes differ (what does each OS need alongside the bytes) — and what stays identical (the bits on the wire)?

## OSN6 — the honest bench across OSes
Concept: same virtio device, same packets; measure the OSes. Do: bench your Linux driver vs your FreeBSD driver vs stock drivers on BOTH OSes (iperf3 + pps-lite, same qemu config): the OS-column table (Mbps/pps/CPU%); bottleneck essays per OS (what degraded each); IRQ-affinity and deferral-trade notes.
Verify: 4-row bench table + 2 bottleneck essays; numbers reproducible on re-run.
**Lesson check:** which OS's overheads surprised you — and what does "same hardware, different stack" teach about where performance lives?

## OSN7 — per-OS security: who guards the driver boundary
Concept: network drivers sit on the attack surface; each OS walls them differently (WFP NDIS filters, IONetwork kext entitlements+signing, ifnet fine-grained, Linux BPF/XDP). Do: map your ND8 filter-lite idea onto each platform's filter point: WFP (Windows: user/callout layers), NEF/BPF (macOS/BSD: NEF clients, BPF attach), BPF/XDP (Linux, you own it) — write the per-OS policy table (what you'd filter where, what law each enforces); detection notes for your own lab builder's traffic.
Verify: per-OS filter-point table + which-you-could-actually-build ranking (honest).
**Lesson check:** why does each OS choose a DIFFERENT enforcement point — and what does the filter table reveal about trust models?

## OSN8 — CAPSTONE: cold second dialect
Prereq: OSN0–OSN7. **Close all notes.** Cold: re-attach the core (attach → xmit → RX → teardown) against FreeBSD's API in one sitting — no notes, your ND skeleton as the only memory — then finish that driver (stats, ioctl basics) and ping it end-to-end. Write `labs/netdriver-allos-capstone.md`: the full matrix, envelope table, ceilings per OS (realistic: FreeBSD proven, Linux proven, Windows skeleton+signing-wall, macOS reading+skeleton), three proud decisions, roadmap (XDP/bpf on all? WFP filter? real hardware day?).
**Pass = FreeBSD driver moves packets cold; the matrix reads like the definitive map of one model across five dialects.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in OSN0/OSN8 boilerplate (skeleton scaffolding) — dialects written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Lab VMs only; macOS lane stays reading/skeleton (license honesty); eval-Windows fine; snapshots before driver experiments.
5. Honest bar: production multi-OS drivers are vendor teams with hardware + signing pipelines; this course's bar = the same skeleton you built once, proven again in a second OS (FreeBSD) and mapped to completion across Windows/macOS/Android — the floor for cross-platform driver work and the ability to read any OS's NIC driver as your own.

## Where this lives
Feeds from NETWORK-DRIVER-COURSE (the deep Linux half), DRIVER-COURSE V-floor, USB-DRIVER's cross-OS mapping method. The native-OS lanes (APPLE, WINDOWS, LINUX-SECURITY) supply the dialect context. One model, five dialects, one pair of hands.