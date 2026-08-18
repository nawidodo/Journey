# 24-62 · Own SOCKS proxy — SOCKS5 + HTTP CONNECT tunnel (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../61-own-p2p-sync`](../61-own-p2p-sync/README.md) · **Next:** [`../63-own-raycaster`](../63-own-raycaster/README.md) · **Pairs:** 24-17, 24-22, 24-18, 03-07

## Objective
Every C2, Tor exit, and pentest tool routes through a proxy. Build one: SOCKS5 (greeting/auth, CONNECT/BIND/UDP-associate, the state machine) + HTTP CONNECT tunneling (pairs 24-17 server + 24-22 onion layering), then chain it: your proxy → your 24-18 WireGuard tunnel, or 3 proxies in a row (the latency/ordering lesson). Security relevance: proxy misuse detection (21-07 hunting), auth bypass, and why proxies are how attackers hide.

## Tasks
- [ ] SOCKS5: greeting + method negotiation (no-auth/user-pass), CONNECT handshake, address parsing (IPv4/6/domain), the relay loop (bidirectional copy with proper close semantics)
- [ ] BIND + UDP-associate (the modes nobody implements — the skill gap); CONNECT via 24-17 HTTP
- [ ] Chaining: proxy → proxy (SOCKS handshake relayed), proxy → 24-18 WG tunnel; latency/MTU observations
- [ ] Auth lab: user-pass auth, brute-force counter (rate limit — pairs 24-17); unauthenticated proxy → your 24-56 scanner finds it (misconfig) — `labs/`
- [ ] Self-check: curl --socks5 + ssh -o ProxyCommand work through it (real clients, the oracle)

## Resources
- RFC 1928 SOCKS5 (the manual); RFC 7231 CONNECT; your 24-17/24-22/24-18 code

## Exit Criteria
- [ ] SOCKS5 CONNECT/BIND/UDP + HTTP CONNECT, real client interop — `labs/`
- [ ] Chaining + auth/misconfig lab — `labs/` + `notes/`

## Links
- [RFC 1928](https://www.rfc-editor.org/rfc/rfc1928)
- [RFC 7231 §4.3.6 CONNECT](https://www.rfc-editor.org/rfc/rfc7231#section-4.3.6)
