# 27-04 · Teamserver + operator UI — multi-session ops

**Week:** W33–35 · **Track:** S · **Prev:** [`../03-agent-core`](../03-agent-core/README.md) · **Next:** [`../05-advanced-transports-redirectors`](../05-advanced-transports-redirectors/README.md)

## Objective
The operator side. A teamserver that manages multiple sessions, and a UI to drive them — the difference between a "toy C2 server" (Track E 04) and a framework.

## Tasks
- [ ] Teamserver: session registry (agent ID → state), task queue + result correlation, per-session crypto keys (from step 02), operator auth (multi-operator: token-based), event log
- [ ] Persistence of state: sessions + results in SQLite (recover after server restart); re-key on agent reconnect
- [ ] Operator UI: minimal web UI (plain HTML + WebSocket, or a small React app) — session list, task panel, output stream; or CLI client with the same backend; pick one, keep the backend transport-clean
- [ ] Multi-operator: two operators, task isolation per session, audit log of who ran what (defender-adjacent: your own audit log is also your forensic record — feeds Track M)
- [ ] C2 profile: per-listener config — URL paths, interval/jitter defaults, UA strings (hand-rolled Malleable-C2-lite from step 02); hot-reload without restarting beacons
- [ ] Kill/remove: operator-triggered session kill, self-destruct (agent step 03), session purge

## Resources
- Sliver server + client architecture; Mythic (its agent-agnostic teamserver model is the reference design)

## Exit Criteria
- [ ] Two concurrent sessions taskable from the UI/CLI, results streamed — `labs/`
- [ ] Server restart → sessions re-establish and state survives — `labs/`
- [ ] Audit log + per-operator isolation working — `labs/`
- [ ] Explain in ≤5 lines your session/crypto/key lifecycle — `notes/`

## Links
- [Sliver](https://github.com/BishopFox/sliver)
- [Mythic](https://github.com/its-a-feature/Mythic)
- [Go WebSocket (gorilla)](https://github.com/gorilla/websocket)
