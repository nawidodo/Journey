# 24-22 · Own onion router — Tor-lite, circuits + hidden service (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../21-own-bgp-speaker`](../21-own-bgp-speaker/README.md) · **Next:** [`../23-own-quic-lite`](../23-own-quic-lite/README.md)

## Objective
Build onion routing on your own network: 3-hop circuits, layered (onion) encryption, rendezvous — a Tor-lite with your own nodes on local VMs, own lab only. The lessons are deep: why layers, what a guard node is, traffic-analysis limits (what Tor does and cannot hide), hidden-service rendezvous.

## Tasks
- [ ] Cells + circuits: fixed-size cells, circuit extend (Diffie-Hellman per hop), relay cell cryptography (pairs 20-03; your 20-11 ratchet skills transfer)
- [ ] Onion layers: per-hop encrypt/decrypt — the "nobody knows the full path" invariant; your own nodes as OP/OR on local VMs
- [ ] Hidden service: rendezvous points, introduction points, HS descriptor (own-lab only, no public network)
- [ ] Attack/analysis lab: capture your own circuits — see the layering from the wire (only next hop visible); write up traffic-analysis limits — `notes/`
- [ ] Self-check: 3-hop path works end-to-end, each node sees only neighbor; circuit teardown clean

## Resources
- Tor design doc (the manual); Tor spec; your 20-03/07/11 notes

## Exit Criteria
- [ ] 3-hop circuit carries traffic across own VMs; HS rendezvous works — `labs/`
- [ ] Wire-view + traffic-analysis writeup — `labs/` + `notes/`

## Links
- [Tor design](https://svn-archive.torproject.org/svn/projects/design-paper/tor-design.pdf)
- [Tor spec](https://spec.torproject.org/)
