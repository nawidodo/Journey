# 24-42 · Own key-value cache — redis-lite: protocol, LRU, persistence (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../41-own-search-engine`](../41-own-search-engine/README.md)

## Objective
Every service sits on one: a redis-lite — RESP protocol (real `redis-cli` talks to it — the oracle), in-memory store with eviction (LRU/LFU), persistence (AOF-style log + RDB-style snapshot, pairs 24-02 WAL), expiration. The security tie: cache poisoning/invalidation thinking (pairs 24-19 DNS cache), and the protocol-parsing discipline (pairs 24-17 HTTP).

## Tasks
- [ ] RESP: parse/serialize the wire protocol — `redis-cli SET/GET/INCR` work against your server (the interop test)
- [ ] Store: hash tables (the classic resize/rehash), string/int ops, expiration (lazy + active cycles)
- [ ] Eviction: LRU (your 24-30 profiler shows the hot path), LFU; memory cap enforcement
- [ ] Persistence: AOF (append log + replay — pairs 24-02/24-26) and snapshot; crash-recovery test (kill -9 → restart → data back)
- [ ] Writeup: cache-as-attack-surface (poisoning, key collision), where redis design shines/struggles — `notes/`

## Resources
- RESP spec; redis source (peer); your 24-02/24-19/24-26 notes

## Exit Criteria
- [ ] redis-cli interop: SET/GET/INCR + expiration + eviction — `labs/`
- [ ] AOF crash-recovery + writeup — `labs/` + `notes/`

## Links
- [RESP spec](https://redis.io/docs/latest/develop/reference/protocol-spec/)
- [redis](https://github.com/redis/redis)
