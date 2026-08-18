# 24-90 · Own rate limiter — token bucket, sliding window, the DoS wall (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../89-own-tts`](../89-own-tts/README.md) · **Next:** [`../91-own-supply-chain-signer`](../91-own-supply-chain-signer/README.md) · **Pairs:** 24-17, 24-66, 24-57

## Objective
Every public service is kept alive by one small component: build it — a rate limiter middleware for your 24-17 HTTP server — token bucket (the classic), sliding-window counter, and fixed-window (the flawed one you'll explain), with keyed limits (per-IP/per-token), then the lab that proves it: your own 24-80 replayer floods your server; the limiter's 429 curve holds, the server's latency curve doesn't — the before/after measurements (pairs 24-66 load-balancing, 24-57 detection).

## Tasks
- [ ] Token bucket: refill model, burst allowance, the parameters that matter (rate/burst — the tuning lab)
- [ ] Alternatives: sliding-window log vs counter (the memory/time tradeoff), fixed-window boundary bug demo
- [ ] Integration: middleware in 24-17, keyed by IP + auth token (24-70/24-12 auth reuse), 429 + Retry-After
- [ ] Flood lab: 24-80 replayer at 1000 rps against own server with/without limiter — the two curves, both measured — `labs/`
- [ ] Writeup: rate-limit bypasses (header spoofing, key rotation, distributed floods — pairs 24-66), why the industry added CAPTCHA/PoW (pairs 20-08 GPU) — `notes/`

## Resources
- Redis/nginx limit docs (the manual — their fields); your 24-17/24-66/24-80 code

## Exit Criteria
- [ ] Limiter holds flood; curves measured before/after — `labs/` + `code/`
- [ ] Bypass + design writeup — `notes/`

## Links
- [Cloudflare rate limiting](https://www.cloudflare.com/learning/ddos/glossary/rate-limiting/)
- [nginx limit_req](https://nginx.org/en/docs/http/ngx_http_limit_req_module.html)