# 24-05 · Own TCP/IP Stack

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** [`../04-own-shell`](../04-own-shell/README.md) · **Next:** [`../06-own-regex-engine`](../06-own-regex-engine/README.md)

## Objective
Implement TCP/IP over raw sockets (Linux tun, or userspace) — ARP, IP, ICMP, TCP handshake/retransmit, then proxy one real app. This is the "what the NIC driver + net stack does" side of Phase 1's xv6-net/e1000 lab and Phase 4's virtio-net reading — the wire reality behind every network detour you'll probe.

## Tasks
- [ ] TUN device + ethernet framing, ARP table — `code/`
- [ ] IP parse/checksum; ICMP echo (ping you) — `code/`
- [ ] TCP: three-way handshake, sequence numbers, window + retransmit — `code/`
- [ ] Deliver one real HTTP/1.1 request to your stack through a local proxy — `code/`
- [ ] Debrief: how this maps to e1000/virtio-net descriptors (Phase 1/4) — `notes/`

## Resources
- "Build your own TCP/IP stack" (saminiir) — the canonical walkthrough
- RFC 791/793; your Phase 1 net lab notes

## Exit Criteria
- [ ] Stack completes a TCP handshake and proxies one real SAPP — `code/`
- [ ] Debrief note mapping to Phase 1/4 net labs — `notes/`

## Links
- [Let's code a TCP/IP stack](https://www.saminiir.com/lets-code-tcp-ip-stack-1-ethernet-arp/)
- [build-your-own-x: own TCP](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-tcp))