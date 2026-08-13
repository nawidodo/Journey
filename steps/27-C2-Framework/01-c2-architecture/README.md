# 27-01 · C2 architecture — beacon model, transports, and what real frameworks do

**Week:** W30–32 · **Track:** S · **Prev:** — · **Next:** [`../02-wire-protocol-transport`](../02-wire-protocol-transport/README.md)

## Objective
Design before code. Understand the C2 blueprint — beacon/check-in model, agent/server split, transport tiers, tasking protocol, and the ops-security reasoning behind every choice — by dissecting real frameworks, then write your own architecture doc.

## Tasks
- [ ] The model: implant (agent) ↔ listener ↔ teamserver ↔ operator; check-in interval + jitter, why long-beacon vs interactive session tradeoffs; stageless vs staged
- [ ] Transport tiers: HTTP/S (common, noisy), DNS (stealthy, slow), SMB named pipes (Windows lateral, no egress), WebSocket, TCP raw; when each wins; redirectors + why (hide teamserver IP, blend with normal web traffic)
- [ ] Tasking protocol: task queue, job types (exec, shell, download/upload, inject, sleep), result relay; why tasks are ephemeral and nothing persists on disk
- [ ] Read the real ones (docs + source): Sliver (Go), Mythic (agent-agnostic + plugins), Cobalt Strike docs (Malleable C2 profiles concept), Havoc, Metasploit (for contrast); extract: transport, framing, encryption, agent language, UI model
- [ ] Detection surface (feeds Track M): what defenders fingerprint — JA3/JA4 TLS fingerprints, HTTP user-agent/headers, beacon periodicity, DNS query entropy
- [ ] Write `notes/architecture.md`: your own C2 design — components, transport, protocol, encryption, operator UX, and *which detection signals you're accepting*

## Resources
- Sliver/Mythic/Havoc docs; Spectrum research blog; Cobalt Strike manual (Malleable C2)
- ired.team C2 section; MalDev Academy C2 modules (cross-ref Track E 04)

## Exit Criteria
- [ ] Architecture doc: full component design + threat-modeled detection tradeoffs — `notes/`
- [ ] Comparison table: 4 real C2s → transport/framing/encryption/agent/UI — `notes/`
- [ ] Explain in ≤5 lines why beaconing beats a persistent socket (and its cost) — `notes/`

## Links
- [Sliver](https://github.com/BishopFox/sliver)
- [Mythic](https://github.com/its-a-feature/Mythic)
- [Havoc](https://github.com/HavocFramework/Havoc)
- [Cobalt Strike manual](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics/welcome_cobalt_strike_4_9.htm)
- [SpecterOps C2 research](https://posts.specterops.io/)
