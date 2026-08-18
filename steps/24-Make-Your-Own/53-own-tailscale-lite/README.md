# 24-53 · Own Tailscale-lite — coordination + NAT traversal + relay (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../52-own-message-queue`](../52-own-message-queue/README.md) · **Next:** [`../54-own-video-container`](../54-own-video-container/README.md)

## Objective
WireGuard (24-18) gives you the tunnel; Tailscale gives you the mesh: a coordination server (device registry + key distribution), NAT traversal (STUN-lite, UDP hole punching — pairs 24-24 NAT), and a fallback relay (DERP-style). Build it: your own devices join, connect peer-to-peer when traversal succeeds, fall back to relay when not. The security lesson: the coordination server is the trust root (why device keys + rotation matter — pairs 24-39 signing).

## Tasks
- [ ] Coordination: device registration, pubkey exchange (WireGuard keys from 24-18), peer lists; a tiny control protocol (wire format — pairs 24-17/24-46)
- [ ] Traversal: STUN-lite (discover mapped address), UDP hole punching (the simultaneous-open dance — pairs 24-24), candidate gathering
- [ ] Relay: DERP-lite (your own 24-17 HTTP server relays encrypted packets when traversal fails); automatic fallback + switchover
- [ ] Lab: two devices behind your own 24-24 NAT router — direct path when punch succeeds, relay when blocked; capture both (24-20 sniffer) — `labs/`
- [ ] Writeup: why mesh VPNs took over (vs hub-and-spoke), the trust-root design — `notes/`

## Resources
- Tailscale's design docs + DERP post (the manual); STUN RFC 8489; your 24-18/24-24/24-17 notes

## Exit Criteria
- [ ] Mesh: devices connect p2p via hole punch, relay fallback works — `labs/`
- [ ] Traversal-fail + trust-root writeup — `labs/` + `notes/`

## Links
- [Tailscale — How it works](https://tailscale.com/blog/how-tailscale-works)
- [STUN RFC 8489](https://www.rfc-editor.org/rfc/rfc8489)
