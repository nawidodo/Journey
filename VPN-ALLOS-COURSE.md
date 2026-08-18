# VPN × All-OS Course — followup (your WireGuard-lite protocol, wired into every major OS's tunnel, gated)

Prereq: `VPN-COURSE` VP0–VP12 — you own the protocol (framing, handshake, AEAD, rekey) end-to-end protocol-side. This course re-READS it into the tunnel interfaces of each OS: Linux `tun`, macOS `utun`, FreeBSD `tun`, **Windows `wintun`** (WireGuard's own virtual adapter), **Android `VpnService`**, iOS NetworkExtension (reading + skeleton — Apple's entitlement reality is an honest ceiling). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/vaN-quiz.md`)** — no advance without both. ~3h/unit, 8 units + capstone ≈ 5-6 weeks.

Compass (re-read when lost): your VPN protocol is portable — it moves bytes between a pipe and UDP. All the OS work is **the pipe**: each OS offers a different virtual-interface API, and the whole difference between "mac+Linux VPN" and "Windows VPN" is a one-file adapter (packet read/write + MTU). Wallfrak builds this abstraction; you'll build your own `PacketPipe` interface and bring three+ implementations live. Windows is the crown: `wintun.dll` (signed by the WireGuard project) hands you ring-based I/O that feels exactly like the tun you already know.

Safety: lab VMs/emulators only (Windows eval VM admin console fine; Android emulator fine); iOS lane = reading + Swift skeleton compile (needs Apple dev team + device — documented as ceiling); macOS lane on your own Mac with root for the utun test. Rules like every course: verification + own-words quiz per unit; copy only VA0/VA1 boilerplate (pipe scaffold); erase-and-retry once; honest ceiling tables; lab keys only.

---

## VA0 — the tunnel matrix: every OS's pipe
Concept: one protocol, many pipes: tun (Linux/BSD), utun (macOS), wintun (Windows ring adapter), VpnService fd (Android), NE packet-tunnel provider (iOS). Do: build the tunnel-matrix table (API, who owns the fd/dll, privileges, MTU default, quirks-per-OS); inventory your lab farm (Linux VM done, add FreeBSD VM + Windows eval VM + Android emulator); write the one-page "what a pipe must be" spec for YOUR `PacketPipe` interface (read/write/close/MTU/setup).
Verify: matrix + pipe-spec written; farm up (each OS sees its virtual NIC path).
**Lesson check:** which four properties does EVERY pipe expose — and why is "one interface, five implementations" the correct architecture here?

## VA1 — Linux + FreeBSD: the known pipes, refreshed
Concept: tun on Linux and if_tun on BSD: close kin, small differences (ioctls, names, flags). Do: port your VPN's tun binding into the pipe adapter (Linux `/dev/net/tun` + FreeBSD `tun`); run your PROTOCOL over BOTH pipes; ping + file transfer on each; note the differences (persist flags, multiqueue, header modes) in the matrix; benchmark both pipes' raw packet rate.
Verify: protocol runs over both pipes; matrix rows for Linux/FreeBSD completed with your measured numbers.
**Lesson check:** what differs between Linux and BSD tun beyond names — and what did your pipe abstraction absorb vs leak through?

## VA2 — Windows wintun: the crown pipe
Concept: wintun.dll presents a ring-based adapter (read/write via mapped rings), signed by WireGuard, kernel driver included — you only write userspace. Do: in the Windows eval VM (admin): load wintun.dll (documented download), create adapter, session + ring setup + start, pump packets; plumb a route (`netsh`/route); run YOUR protocol end-to-end through the Windows pipe: ping + file transfer with your Linux VM.
Verify: Windows pipe moves your protocol; end-to-end VPN traffic proven; wintun session/ring lifecycle documented.
**Lesson check:** what does "signed driver, userspace dll" buy WireGuard — and how does a ring differ from an fd (who blocks, who spins)?

## VA3 — macOS utun: your own OS, deep
Concept: utun = BSD-style tun on Darwin; IOCTLs + a sysctl name dance. Do: on your Mac (root for the tun part): utun setup via fcntl (`UTUN_OPT_IFNAME`), plumb a route, wire your pipe, run your protocol mac↔Linux; document the Darwin quirks (no /dev/net/tun; ioctl names; `route` vs `route -n`); benchmark the mac utun raw rate.
Verify: mac↔Linux VPN traffic through YOUR utun pipe; Darwin quirk-notes written.
**Lesson check:** what does macOS change about the tun ritual vs Linux — and why would a production VPN vendor still love utun (speed, stability)?

## VA4 — Android VpnService: the app-is-the-VPN
Concept: Android VPN = an APP: your app holds a VpnService fd; ALL routing is the app's. Do: in the Android emulator: VpnService skeleton (Kotlin) that creates the tun fd (blocking routes, DNS config), a native (or Kotlin) packet pump running YOUR protocol (C-core via JNI is the honest shape); force your emulator through it: traffic to your Linux VM routes through the VPN app; verify with ping/curl + a log line per packet.
Verify: emulator's traffic traverses your protocol; VpnService lifecycle (connect/disconnect) clean.
**Lesson check:** why does Android force VPNs into app-land — and what does VpnService's "blocking" API actually configure (what could it silently not cover)?

## VA5 — iOS NetworkExtension: reading + the honest ceiling
Concept: iOS: `NEPacketTunnelProvider` requires entitlements (Packet Tunnel Provider), a dev team, and a device/simulator; the WireGuard iOS app is the canonical reference. Do: reading the NE architecture (provider lifecycle, packet flow, why Apple restricts it); Swift skeleton (NEPacketTunnelProvider subclass, start/stop, packet handler) that COMPILES in Xcode; the ceiling doc: cannot fully run in this course (Apple provisioning), so — roadmap for the day you have a team; map NE's flow vs VpnService's (who's more locked, and why).
Verify: skeleton compiles; NE-vs-VpnService comparison written; ceiling documented honestly.
**Lesson check:** why does Apple gate packet tunnels behind entitlements — and what does that reveal about iOS's threat model for VPN-like tools?

## VA6 — one pipe interface, five dialects: the architecture
Concept: the product moment: your `PacketPipe` interface unifies all of it — one protocol, three+ live pipes. Do: design + implement the adapter layer (abstract pipe + tun/utun/wintun/VpnService implementations, MTU negotiation); refactor your VPN lib to consume ONLY the pipe interface; live-test: a SINGLE VPN binary path driving Linux, macOS, and Windows endpoints (three nodes, one tunnel mesh or star).
Verify: three-OS endpoints in one network via your lib; matrix "live" column completed.
**Lesson check:** what leaked through your abstraction (the honest list) — and why is forcing real abstraction on real OSes the only way to learn what's portable?

## VA7 — the routing and MTU per OS
Concept: routing into the tunnel differs per OS: route/netsh/iproute/route+blocking/NE rules; MTU clamping dances differ. Do: complete the routing-table matrix (per OS: add route cmd, default-route handling, DNS plumbing, MTU clamp point, broken-by-default pitfalls); reproduce three MTU failures (Linux, Windows, Android) with fixes; write the "MTU survival guide" per OS.
Verify: routing matrix complete; three MTU failures reproduced + fixed; survival guide written.
**Lesson check:** which OS makes routing into a tunnel most error-prone — and why does MTU keep biting differently per OS (where's each clamp point)?

## VA8 — CAPSTONE: the mesh, cold
Prereq: VA0–VA7. **Close all notes.** Cold: write the pipe adapter for ONE OS you haven't got live yet (keep Linux + one other as known-good) against the pipe spec — no notes — then bring it up and join your protocol mesh (3+ endpoints, mixed OSes, ping + file transfer across each pair). Write `labs/vpn-allos-capstone.md`: the matrix (live/ceiling columns), pipe architecture diagram, routing+MTU guide, three proud decisions, regret, roadmap (iOS day, kernel-speed pipes, mesh routing beyond star).
**Pass = mixed-OS mesh VPN running cold; the matrix reads like the definitive map of your protocol across every major OS.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in VA0/VA1 boilerplate (pipe scaffold) — adapters, routing, pumps written by you; erase-and-retry once when stuck.
3. 3h/unit; stuck past that = previous unit's verification again.
4. Lab only: own VM/emulator/Mac, admin only where the OS truly needs it (Windows admin console, Mac root for utun), lab keys, never production traffic; iOS ceiling documented, not faked.
5. Honest bar: WireGuard/OpenVPN ship audited multi-OS clients via teams + app stores + signing pipelines; this course's bar = the same protocol you designed, wired live into three+ of the five major OS pipes and mapped to completion on the rest — the floor for cross-platform VPN engineering and the exact shape of every "works everywhere" network tool.

## Where this lives
Follows VPN-COURSE (the protocol) + NETWORK-DRIVER-ALLOS's method (one model, dialects); USB-DRIVER's cross-OS mapping carries here. The Android lane feeds the mobile courses; Windows wintun dovetails the Windows lanes; macOS utun is home turf.