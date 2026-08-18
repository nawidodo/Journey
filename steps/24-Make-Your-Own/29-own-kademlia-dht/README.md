# 24-29 · Own Kademlia DHT — distributed hash table from scratch (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../28-own-quantum-simulator`](../28-own-quantum-simulator/README.md) · **Next:** [`../30-own-sampling-profiler`](../30-own-sampling-profiler/README.md)

## Objective
The distributed-systems gap: every P2P network runs Kademlia (BitTorrent's DHT, IPFS, Ethereum). Build it: XOR-distance routing, k-buckets, iterative lookup, store/find_value, replication + republish. Then the security angle — the Sybil attack (the fundamental P2P trust problem) and eclipse attacks; run them against your own nodes in a local VM swarm.

## Tasks
- [ ] Routing: 160-bit ID space, XOR distance, k-buckets, bucket splitting; ping/RPC
- [ ] Lookup: iterative k-closest queries (the "converge on the value" walk); store/find_value with replication + republish timers
- [ ] Swarm: run N local nodes (VMs/containers — your 24-13 runtime helps), put/get keys, churn survival (nodes join/leave)
- [ ] Attack lab: Sybil — one node owns many IDs, influences lookups; eclipse — surround a victim, starve it of honest peers; measure + document (pairs 22-own-onion-router's trust discussion) — `labs/`
- [ ] Self-check: 20-node swarm put/get; after attack, honest-ratio threshold where lookups still resolve

## Resources
- the Kademlia paper; BitTorrent DHT spec (the manual); libp2p docs (peer); your 24-22 notes

## Exit Criteria
- [ ] Swarm put/get + churn survival — `labs/`
- [ ] Sybil/eclipse attack measured + writeup — `labs/` + `notes/`

## Links
- [Kademlia paper](https://pdos.csail.mit.edu/~petar/papers/maymounkov-kademlia-lncs.pdf)
- [BitTorrent DHT](https://www.bittorrent.org/beps/bep_0005.html)
