# VPN Course — Absolute-Beginner (hello world → your OWN secure tunnel protocol like WireGuard/OpenVPN, gated)

Zero VPN knowledge assumed — you need Python (or C if you want kernel-speed later), a Mac + one Linux VM (or two VMs), and your cryptography-floor warming up (20-07 own-TLS + 20-Crypto steps help; the crypto here is hand-rolled IN UNIT — RFC 8439 and RFC 5869 are your readings). Each unit: concept → do → runnable verification → **lesson check (own words, `notes/vpN-quiz.md`)** — no advance without both. ~2-3h/unit, 12 units + capstone ≈ 7-9 weeks. This is "make a remote network behave like a local one" — tunnel + transport + crypto + keying — and you design the whole protocol yourself.

Compass (re-read whenlost): a VPN is **a fake network interface + a wire + a cipher + a key agreement**: the tun device makes the kernel think a network exists, UDP carries your packets, AEAD makes them confidential+authenticated, and a handshake makes both sides agree on keys nobody else has. WireGuard = minimal version (static keys, one AEAD, UDP, roaming for free); OpenVPN = TLS-certified version (enterprise auth, TCP/UDP). Your course: a **WireGuard-lite** — same skeleton, your design, your RFC-style spec, your code, proven between two of your own endpoints. Every real VPN you'll ever meet is this skeleton with a different suit on.

Safety: this is your OWN protocol on YOUR OWN lab endpoints — educational crypto (never production traffic; meaningless keys; the point is structure and vectors, not secrecy). Hand-rolled crypto = lab-only BY DESIGN; production-grade VPNs must use audited libraries — that's an honesty rule, not a suggestion. Nothing routes real data; no third-party networks.

---

## VP0 — hello world: what a VPN actually is
Concept: problem: remote network should behave local; solution: tunnel (L3 interface) + transport (UDP) + crypt (AEAD) + keying (handshake). Do: draw the four-layer diagram (apps → tun → your protocol → UDP → internet → ... ) with arrows marking WHERE your code lives; read WireGuard white-paper + OpenVPN's protocol overview (two readings, notes); inventory your lab (mac + Linux VM, ping between them directly first).
Verify: four-layer diagram + two design notes written; lab endpoints ping.
**Lesson check:** what does a VPN add ABOVE the network — and why did VPNs move from "encrypted PPP of old" to "kernel interface + tunnel" (what's the interface for)?

## VP1 — the tun interface: a network that's really a file
Concept: tun = kernel feature: writes into the fd = packets IN; reads = packets OUT — the OS does routing, you do everything else. Do: create a tun (mac: utun via fcntl ioctl; Linux VM: /dev/net/tun) in Python, assign addresses, `ping` machine-A's tun IP from machine-B **with both endpoints wired to your fd** (no crypto yet — a raw tunnel); watch the transit in tcpdump (plaintext proof).
Verify: ping traverses your raw tun tunnel; tcpdump shows ICMP packets moving between endpoints.
**Lesson check:** what does writing-to-tun mean for the kernel (it believes a packet went out a NIC) — and why is this the "virtual" in VPN?

## VP2 — your tunnel protocol: framing by design
Concept: raw bytes need a format: magic, version, length, type, sequence, payload — a protocol spec YOU write. Do: your packet format spec doc (RFC-style: layout table, byte offsets, endianness); implement framing (pack/unpack, length limits, max-packet MTU math with overhead budget); multiplex control vs data types; benchmark raw-tunnel throughput (frame overhead %).
Verify: spec doc + framing code round-trips; overhead % table measured; malformed-input rejected.
**Lesson check:** what does a frame header buy you that a raw stream lacks — and why must you size packets under the path MTU before crypto adds bytes?

## VP3 — UDP transport: fire and forget, after the handshake
Concept: UDP = datagrams, no reliability; VPNs CHOOSE it (TCP-over-TCP collapse — reading) and add their own ordering needs at the protocol layer. Do: implement UDP transport state machine (idle → handshaking → established → closing), packet send/recv with source validation (who may speak?), NAT-traversal reading (why UDP is NAT-friendly; STUN concept); document your reliability decision (likely: none for data, retry for handshake).
Verify: state machine transitions logged; wrong-source packets dropped; TCP-over-TCP problem explained in notes.
**Lesson check:** why does UDP win in VPNs despite no guarantees — and what broke when TCP tunnels rode over TCP (the collapse) — and what does your state machine protect?

## VP4 — crypto layer 1: ChaCha20-Poly1305, hand-built
Concept: AEAD = one pass gives confidentiality + integrity; ChaCha20-Poly1305 (RFC 8439) is the modern default (AGL's choice in WG). Do: implement ChaCha20 + Poly1305 from the RFC (Python big-int/bytes work fine), pass the RFC 8439 TEST VECTORS (the certification ritual); wire AEAD sealing/opening into your framing with a 96-bit nonce + counter; fuzz-verify: flipped ciphertext byte → auth failure, always.
Verify: RFC test vectors pass; tamper test fails every tampered packet; per-packet overhead measured.
**Lesson check:** what does AEAD combine — and what happens when your counter reuses a nonce (the one sin) — and how did you prove correctness without an oracle?

## VP5 — crypto layer 2: the handshake and key schedule
Concept: mutual keys: X25519 (RFC 7748) for identity/ephemeral, HKDF (RFC 5869) to derive session keys; static keys (WireGuard-style) avoid CA infra. Do: implement X25519 scalar mult (curve25519 field math — Python handles it) + pass RFC 7748 vectors; implement HKDF-SHA256 per RFC 5869 + vectors; your handshake design (initiator → responder messages, nonces, MACs — model WireGuard's 3-message shape but YOUR layout); derive per-direction keys; replay counter rules.
Verify: X25519/HKDF vectors pass; handshake completes endpoint-to-endpoint; third-party (your own eavesdrop VM) cannot derive keys.
**Lesson check:** why static-keys-not-CA (manifesto of WG) — and what exactly does HKDF derive FROM what (what's the shared secret dance)?

## VP6 — the protocol spec, complete
Concept: a protocol becomes real when the spec is complete: all messages, states, errors. Do: finalize your spec (handshake msgs, data msgs, magic/version, nonce rules, error codes, timeout values); implement the full pipeline: tun → frame → AEAD → UDP and back; end-to-end: ping + a file transfer (scp-equivalent via your own tool) through the VPN.
Verify: ping + file transfer traverse your full protocol; spec doc matches code 1:1 (audit pass).
**Lesson check:** why does a dance-round (spec ↔ code audit) matter more for crypto than for any other software — and what's the cost of a protocol growing without a spec?

## VP7 — routing and the MTU minefield
Concept: VPN value = subnets reachable: routing tables on both ends, NAT if needed, and MTU/MSS — the classic trap. Do: route your peer's subnet through the tunnel (route table on both endpoints); enable IP forwarding on the VM; test LAN-to-LAN (mac subnet ↔ VM subnet) reachability; then the MTU rabbit hole: ping sizes, fragmentation behavior, MSS clamping — document each symptom (documented, honest).
Verify: cross-subnet traffic flows through your VPN; MTU failure modes documented with fixes.
**Lesson check:** what does routing through a tun require of the kernel — and why does MTU bite VPNs harder than direct links (crypto overhead + encapsulation)?

## VP8 — keepalive, rekey, and roaming
Concept: VPNs live: heartbeats detect dead peers; rekey rotates keys before re-use limits; roaming follows a peer that changed address. Do: keepalive (heartbeat messages + dead-peer detection, timeout tuning), scheduled rekey (re-handshake at a key-age limit, no drop mid-transfer), roaming (accept data from a NEW endpoint address for an established peer — WireGuard's trick, implemented + tested); all three demo'd live.
Verify: dead-peer detected; rekey completes without session loss; roam: moving the VM's IP mid-tunnel, tunnel survives.
**Lesson check:** what is rekey protecting (key reuse limits) — and why does roaming "just work" with UDP (what dies in TCP that doesn't in UDP)?

## VP9 — performance true: overhead and throughput
Concept: VPNs cost bytes and CPU: measure both. Do: throughput/latency bench (direct vs tunnel vs tunnel+offload tweaks), per-packet overhead accounting (UDP 8 + your header + AEAD 16 tag + nonce, per 1400 payload), crypto throughput bench (your hand-rolled ChaCha vs openssl's — humbling, honest); batching attempt (multiple packets per write) with measured gain.
Verify: overhead table + bench chart + crypto-speed comparison all honest, reproducible.
**Lesson check:** where do YOUR VPN's bytes go per packet — and why is hand-rolled crypto slower (what does the hardware/asm do that your Python can't)?

## VP10 — the threat model: hardening your own design
Concept: VPNs are security software: threat model is the spec section that matters most. Do: write the threat model for YOUR protocol: assets (keys, traffic, endpoints), attackers (passive eavesdrop, active MITM/forgery, DoS on handshake, replay), mitigations (AEAD, static keys, source validation, cookie mechanism — WireGuard's anti-DoS reading, rekey); implement TWO hardening fixes from the list (e.g., handshake rate-limit + cookie-lite).
Verify: threat model doc written; two hardening fixes implemented + tested (attack simulation for each).
**Lesson check:** which of your mitigations stops which attacker — and why is "handshake DoS" a VPN-specific weakness (public IP + unauthenticated first packet)?

## VP11 — the real VPNs, read at depth
Concept: your skeleton meets the giants: WireGuard (minimal, audited, kernel-fast) vs OpenVPN (TLS CAs, enterprise, TCP). Do: deep-read WireGuard's protocol (compare YOUR message layout vs theirs — where did you differ and why); deep-read OpenVPN's TLS usage (why certs, why TLS session-then-channel); write the design-diff paper: your choices vs theirs on keying, framing, transport, anti-DoS; the enterprise question (cert/PSK/SSO auth) as reading.
Verify: design-diff paper (3 sections, mapped to your units) + honest verdict on where your design would lose to theirs.
**Lesson check:** what did WG strip that OVPN keeps (and why) — and which of your unit-decisions would change in a prod VPN (and into what)?

## VP12 — CAPSTONE: ship the protocol, cold
Prereq: VP0–VP11. **Close all notes.** Cold rebuild the core (tun setup + framing + handshake + AEAD data path) in one sitting, then finish vp v1: keepalive/rekey/roaming, threat-model hardening, spec doc, CLI; bring TWO of your endpoints into one network; ping + file transfer + a roaming test + a rekey during load; write `labs/vpn-capstone.md` like a protocol designer: spec summary, threat model, bench table, three proud decisions, regret, roadmap (kernel-speed impl? TCP transport? certificates?).
**Pass = two endpoints share a network through YOUR protocol, cold; the spec and threat model read like a designer's.**

---

## Rules
1. Verification + lesson check both true before next unit; quizzes in your own words.
2. Copying allowed only in VP0/VP1 boilerplate (tun scaffold, socket loop) — framing, crypto, handshake, spec written by you; erase-and-retry once when stuck.
3. 2-3h/unit; stuck past that = previous unit's verification again.
4. Lab-only: endpoints you own; meaningless lab keys; never production traffic; hand-rolled crypto is a LEARNING exercise only (audited libraries are the production rule).
5. Honest bar: WireGuard/OpenVPN are audited, years of engineering; this course's bar = your own designed, specified, implemented, hardened, bench-tested tunnel protocol moving real traffic between your endpoints, cold at the capstone — the floor for secure-network-software engineering and the exact skeleton of every VPN, tunnel, and encrypted-transport you'll ever read.

## Where this lives
Feeds from 20-crypto steps (07 own-TLS, 11 e2ee, 13 side-channel awareness), 24-20 sniffer, and your PROXY-MITM course (the decrypting twin: a proxy TEARS TLS apart, a VPN BUILDS it — same bytes, opposite directions; knowing both sides is the point). AUTH-TOOLING's Kerberos/SSL work is the same discipline one protocol higher.

All-OS pipe course — same protocol, five tunnel interfaces (wintun/utun/tun/VpnService/NE): [`VPN-ALLOS-COURSE.md`](VPN-ALLOS-COURSE.md) VA0–VA8.