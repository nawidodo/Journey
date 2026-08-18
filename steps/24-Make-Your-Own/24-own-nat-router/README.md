# 24-24 · Own NAT/router — the internet in a box (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../23-own-quic-lite`](../23-own-quic-lite/README.md)

## Objective
Every home network runs one. Build a router: forwarding, NAT (conntrack-lite table: mapping, aging, hairpin), and a firewall rule language (iptables-lite: chains, match, accept/drop). Then attack/defend your own box — NAT traversal why-it-works (pairs 24-23 QUIC migration), and the firewall as your own mini-detection surface.

## Tasks
- [ ] Forwarding: IP forwarding across two interfaces (VM pairs), ARP resolution, MTU; ping across your router
- [ ] NAT: conntrack table (tuple → mapping, TCP/UDP/ICMP), aging/timeouts, hairpin; masquerade vs static NAT
- [ ] Firewall: rule chains (INPUT/FORWARD/OUTPUT), matches (proto/port/state), default-deny; log hits
- [ ] Labs: NAT traversal — why QUIC/UDP punches holes (pairs 24-23, 24-18 WG); port-forward to an internal service; firewall drop log as mini-detection (pairs 21-07 hunting) — `labs/`
- [ ] Writeup: what home routers do wrong (UPnP, default creds, exposed admin) — `notes/`

## Resources
- RFC 2663 (NAT); netfilter/iptables docs; your 24-05 stack + 24-20 sniffer notes

## Exit Criteria
- [ ] VM network routed + NATed through your router — `labs/`
- [ ] Firewall rules enforced, traversal lab documented — `labs/` + `notes/`

## Links
- [RFC 2663 (NAT terminology)](https://www.rfc-editor.org/rfc/rfc2663)
- [netfilter docs](https://www.netfilter.org/documentation/)
