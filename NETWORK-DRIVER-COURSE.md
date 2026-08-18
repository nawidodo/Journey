# Network Driver Course — Absolute-Beginner (hello world → your OWN NIC driver moving real packets, gated)

Zero driver knowledge assumed — do `DRIVER-COURSE` V0–V6 first (module hello, MMIO, DMA, IRQ on the QEMU "edu" trainer: one device, three OSes) plus the Linux module basics of `LINUX-SECURITY-COURSE` K-units. Lab: your Linux VM under QEMU with a **virtio-net NIC** — the standard virtual NIC — and `iperf3`/`ping` for proof. Each unit: concept → do → runnable verification → **lesson check (own words, `notes/ndN-quiz.md`)** — no advance without both. ~3-4h/unit, 12 units + capstone ≈ 8-10 weeks.

Compass (re-read when lost): a NIC driver is **ring + DMA + IRQ + the packet path**: the hardware owns descriptor rings in memory (TX = "here's a packet to send", RX = "here's a buffer to fill"), DMA moves data without the CPU, interrupts tell you something happened, and the receive path pushes packets up the networking stack. Everything else (offloads, filters, stats) hangs off those four pieces. virtio-net is the perfect first NIC: real ring semantics, but you read the device spec instead of a 600-page datasheet — and the driver you write moves actual packets between your VM and the QEMU host.

Safety: standard lab discipline (own VM, snapshots, no real hardware to break). Rules like every course: verification + own-words quiz gate each unit; copy only ND0/ND10 boilerplate (module skeleton, bench harness); erase-and-retry once when stuck; benchmark honestly.

---

## ND0 — hello world: what a NIC driver does
Concept: the packet lifecycle: app → stack → driver xmit → ring → DMA → wire; and the reverse with RX + IRQ. Do: audit V0–V6 skills; in your VM list the virtio-net device (`lspci`), its driver (`lsmod | grep virtio_net`), its sysfs stats; draw your lifecycle map (TX and RX sides, marking where the CPU is involved vs DMA); load an empty skeleton module.
Verify: lifecycle map + device/driver inventory produced; skeleton module loads in-VM.
**Lesson check:** why does DMA exist (what does the CPU stop doing) — and where does the driver sit between the stack and the ring?

## ND1 — ethernet framing, read like a driver
Concept: a driver sees frames, not "packets": dst MAC, src MAC, ethertype, payload; VLAN tags; CRC. Do: hand-parse a `.pcap` of your own VM traffic (from 24-20's sniffer) into fields; write an ethernet-header pretty-printer in C; verify against `tcpdump` output for the same capture; note where the driver would *care* (which frames it must pass vs drop).
Verify: your parser matches tcpdump field-for-field on 10 frames.
**Lesson check:** what does the driver check before the stack sees a frame — and what does "EtherType" actually gate?

## ND2 — rings and DMA: the virtio memory model
Concept: virtqueue = descriptor ring (guest memory) + available/used rings + kicks; descriptors point to DMA buffers. Do: read the virtio spec sections (virtqueue + net device); in your module: probe the virtio-net PCI device (following DRIVER-COURSE MMIO/DMA patterns), negotiate feature bits (per spec), and build ONE TX descriptor ring + ONE RX ring with DMA buffers (dma_alloc_coherent style); log ring setup.
Verify: rings initialized with correct descriptor layout (log + your own ring dumper); feature negotiation logged.
**Lesson check:** why is the ring in *shared* memory at all — and what does "kick" do that the ring alone can't?

## ND3 — the TX path: your module sends real bytes
Concept: xmit: map buffer, fill descriptor, kick, handle completion; the stack calls you, you call the device. Do: register a minimal netdev (`net_device` + NDOs), implement `ndo_start_xmit` on the virtio ring: map skb, enqueue descriptor, kick, reap completions; bring the interface up; `ping` the gateway/host from the VM and watch your module's counters + host side receiving (tcpdump on host).
Verify: ping works and your TX stats move; host tcpdump sees your module's frames.
**Lesson check:** walk one ping echo through TX — what does the driver do per packet vs once at setup?

## ND4 — the RX path: packets come home
Concept: RX ring always pre-filled; IRQ fires; driver polls, allocates skbs, hands up via the stack API; stats. Do: implement RX: pre-fill RX ring buffers, IRQ handler → NAPI polling → `netif_receive_skb` (or napi_gro_receive) with per-packet stats; replies flow (ping echo reply reaches the VM's stack — `ping` succeeds both ways); packet count matches tcpdump.
Verify: bidirectional ping works through YOUR driver; RX stats accurate vs host capture.
**Lesson check:** why are RX buffers pre-allocated in a ring — and what happens when the ring drains (drop, backpressure)?

## ND5 — interrupts and NAPI: the throughput lever
Concept: interrupt per packet melts at 100k pps; NAPI defers to polling with budgets. Do: implement NAPI properly: `napi_schedule`, poll budget, softirq path; measure CPU% and throughput with and without NAPI (bench script); IRQ coalescing notes (`ethtool -C` on real NICs — reading, virtio behaves); record the difference honestly.
Verify: NAPI version sustains higher pps at lower CPU (your table); budget semantics documented.
**Lesson check:** why does interrupt-per-packet die at scale — and what does "budget" actually protect?

## ND6 — offloads: checksums and the split of labor
Concept: NICs checksum/segment in hardware; virtio announces capabilities; software fallback exists for the corner cases. Do: negotiate RX/TX checksum offload features; implement the *software* checksum verify path (your own checksum code — TCP/UDP over IPv4, RFC 1071) as the honest fallback; log when hardware offload is used vs not; document the offload matrix (checksum/GSO/GRO: enabled? what the NIC does).
Verify: checksum verify matches expected values on your capture; offload negotiation logged.
**Lesson check:** what does checksum offload save the CPU — and why does the driver still need the software path for exotic frames?

## ND7 — parsing at the driver line: IP/TCP by hand
Concept: the driver sees raw bytes; protocol parsing is a stack job — but drivers parse just enough to classify/steer. Do: in your driver: parse ethernet → IPv4 → TCP/UDP headers on RX (hand-written, no helpers), log proto/ports; add a steering counter (e.g., count HTTP flows); STILL hand packets up untouched (parsing ≠ touching); document the line between driver-touching and stack-owning.
Verify: your parse-logs match host capture ports for 20 flows.
**Lesson check:** why parse in the driver if the stack parses anyway — and what's the danger of drivers mutating packets?

## ND8 — filtering at the wire: your mini-rx-policy
Concept: drivers can drop at the wire: cheap, before the stack — the "driver as security boundary" idea. Do: in your driver: RX filter hook with a small rule engine (drop by MAC / ethertype / port — your rules, in-VM); rules loaded via sysfs or module param; prove: blocked traffic never reaches the VM's stack (tcpdump shows wire, stack sees nothing); mirror to your network-detection knowledge (21-07) in the writeup.
Verify: filter-drop demonstrated live (before/after port-check table); rules persist reload.
**Lesson check:** what does dropping-at-the-driver save vs dropping in the stack — and where's the line between filter and firewall?

## ND9 — TX batching: through the queue
Concept: batching amortizes kicks: one kick per N descriptors beats N kicks. Do: implement TX batching (accumulate descriptors, single kick per batch), tunable batch depth; measure pps gain vs ND3's per-packet kick (honest table); queue-organization notes (why some NICs have many queues — multiqueue/SR-IOV reading).
Verify: batched TX pps higher than unbatched at same load; batch-depth tuning chart.
**Lesson check:** what does one kick save — and what sets the practical batch ceiling?

## ND10 — performance honest: pps, Mbps, CPU
Concept: drivers are measured; the delta is where the hardware's in the loop. Do: full bench: iperf3 (Mbps) + pktgen-lite or netperf-class (pps) through your driver vs the stock `virtio_net` driver; CPU% via your bench script; bottleneck analysis essay (where did throughput go: copy vs DMA vs IRQ rate); IRQ affinity reading.
Verify: bench table driver-vs-stock (yours slower but proportionally sane — the lesson); bottleneck essay written.
**Lesson check:** why will the stock driver beat yours — and which bottleneck did YOU choose to improve first (budgeting)?

## ND11 — the hardware reality tour (reading)
Concept: real NICs (Intel ixgbe, Realtek, Apple silicon Ethernet) add rings per queue, SR-IOV, RDMA, firmware, offloads — same skeleton, bigger trees. Do: read ONE real Linux driver's ring setup (choose ixgbe or r8169) — map its structures to your virtio ones; SS-read SR-IOV/RDMA concepts; write the "where virtio diverges" doc; honest barrier written: a real datasheet driver is months + hardware — your virtio-net-lite is the same skeleton, proven.
Verify: mapping doc (your rings ↔ ixgbe's structures) + divergence notes written.
**Lesson check:** what's genuinely different in real NICs (queues, firmware, RDMA) vs what's the same skeleton?

## ND12 — CAPSTONE: your driver ships, cold
Prereq: ND0–ND11. **Close all notes.** Fresh VM: cold-reconstruct the TX+RX+NAPI core (ring setup → xmit → IRQ→poll → receive), then finish virtio-net-lite v1: TX batching, RX NAPI budget, checksum fallback, rx-filter, stats — and move traffic through it (ping + iperf3). Write `labs/netdriver-capstone.md` like a driver maintainer: design, per-path diagram (TX/RX), bench table vs stock, three proud decisions, regret, roadmap (multiqueue, XDP, real-NIC port).
**Pass = your driver moves real traffic start-to-finish cold; writeup reads like a maintainer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in ND0/ND10 boilerplate (module skeleton, bench harness) — rings, paths, filters, checksums written by you; erase-and-retry once when stuck.
3. 3-4h/unit; stuck past that = previous unit's verification again.
4. Own VM lab; snapshots before driver-reload experiments; stock driver intact as the control.
5. Honest bar: production NIC drivers are years of vendor engineering; this course's bar = your own module moving real packets through every path (TX/RX/NAPI/offload-lite/filter), measured honestly, cold-rebuilt at the capstone — the floor for driver work and the exact mental model behind every network device, virtual or physical, in the systems you'll run.

## Where this lives
DRIVER-COURSE V0–V6 is the floor; LINUX-SECURITY K-units the module basics; your 24-20 sniffer understanding grows into driver-context parsing. Pairs the EMULATOR path (a device model you emulate + a driver you write = full ownership of one NIC) and network-defense lanes (21-07) on the other side of the wire.