# 24-66 · Own load balancer — reverse proxy routing, health-aware draining (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../65-own-audio-modem`](../65-own-audio-modem/README.md) · **Next:** [`../67-own-adblock-dns`](../67-own-adblock-dns/README.md) · **Pairs:** 24-17, 24-48, 24-42

## Objective
Everything behind a URL is load-balanced. Build one: reverse proxy (extend your 24-17 HTTP server) with round-robin/least-connections/consistent-hash routing (pairs 24-42 hashing), health checks + draining (pairs 24-48 orchestrator reconciliation), sticky sessions, and connection pooling. Then the failure lab: kill backends mid-request, observe draining + retries — no dropped sessions (the deliverable). Security tie: LB as TLS-termination point (20-07), header trust (X-Forwarded-For spoofing — the classic auth bypass), and misconfig as attack surface.

## Tasks
- [ ] Core: proxy handler (24-17), backend pool, round-robin → least-conn → consistent hash (ring — pairs 24-29 DHT ring reuse)
- [ ] Health: active probes (GET /healthz) + passive (5xx counting), drain on failure → no-request-failover window
- [ ] Stickiness: cookie/IP hash; the session-consistency rationale
- [ ] Security lab: spoofed X-Forwarded-For → backend trust bug (auth bypass demo in your own 24-17 app); TLS termination (20-07) — `labs/`
- [ ] Failure matrix: kill-backend mid-session, slow-loris backend (pairs 24-17), rolling drain — `labs/`

## Resources
- Nginx/HAProxy docs (the manuals — the routing + health design); your 24-17/24-48/24-29 code

## Exit Criteria
- [ ] LB routes across backends with health-draining, no dropped sessions — `labs/`
- [ ] Header-spoof + failure-matrix writeup — `labs/` + `notes/`

## Links
- [HAProxy docs](https://www.haproxy.org/documentation/overview/)
- [nginx docs](https://nginx.org/en/docs/)