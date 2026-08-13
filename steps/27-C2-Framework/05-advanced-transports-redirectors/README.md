# 27-05 · Advanced transports + redirectors — DNS, SMB pipes, WebSocket

**Week:** W34–36 · **Track:** S · **Prev:** [`../04-teamserver-operator-ui`](../04-teamserver-operator-ui/README.md) · **Next:** [`../06-capstone-full-op`](../06-capstone-full-op/README.md)

## Objective
HTTP is the easy transport. Make the framework survive networks where HTTP(S) egress is monitored or blocked: DNS C2, SMB named-pipe C2 (Windows lateral), WebSocket, and a redirector layer that hides the teamserver.

## Tasks
- [ ] DNS C2: encode check-in/results in A/TXT record queries (base32/base64, label length limits, entropy problem); a mini DNS server (Go `miekg/dns`) + agent transport; measure detection: query entropy, NXDOMAIN rates (feeds Track M)
- [ ] SMB named-pipe C2: Windows-only — agent connects to a named pipe on a pivot host, relay to teamserver; when it's the right call (no internet egress, lateral movement); reuse Track D/G pipe knowledge
- [ ] WebSocket transport: bidirectional, low-latency, real-time tasking (vs polling); when the beacon model doesn't fit
- [ ] Redirectors: nginx/apache reverse proxy or socat → teamserver; why the listener IP must never be the teamserver IP; TLS cert on the redirector (domain-fronting concept — what it is, current status)
- [ ] Transport switching: agent falls back HTTP → DNS → (Windows) SMB; failover config; what a defender sees during failover
- [ ] Update `code/` transports behind the protocol from step 02 — one framing, N transports

## Resources
- Sliver DNS/pipe transports source; Cobalt Strike DNS beacon docs; ired.team SMB C2

## Exit Criteria
- [ ] DNS C2 round-trip works end to end (agent ↔ your DNS server ↔ teamserver) — `labs/`
- [ ] Redirector in front of teamserver, verified via curl on the redirector's IP — `labs/`
- [ ] Transport failover demonstrated (kill HTTP → agent surfaces on DNS) — `labs/`
- [ ] Explain in ≤5 lines why DNS C2 is stealthy but slow, and when you'd still pick it — `notes/`

## Links
- [miekg/dns (Go)](https://github.com/miekg/dns)
- [Cobalt Strike DNS beacons](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics/listeners_dns.htm)
- [ired.team SMB C2](https://www.ired.team/offensive-security/command-and-control/smb-pipe-c2)
