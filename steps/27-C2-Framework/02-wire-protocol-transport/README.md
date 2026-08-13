# 27-02 · Wire protocol + HTTP/S transport — framing, crypto, beaconing

**Week:** W31–33 · **Track:** S · **Prev:** [`../01-c2-architecture`](../01-c2-architecture/README.md) · **Next:** [`../03-agent-core`](../03-agent-core/README.md)

## Objective
The first working slice: a real protocol between agent and server. Design the wire format and crypto yourself, then build a minimal HTTP beacon + listener that round-trips a task.

## Tasks
- [ ] Protocol design: framing (JSON vs msgpack vs binary), versioning, message types (check-in, task, result, error, kill), correlation IDs; why length-prefixed binary beats ad-hoc JSON for parsing and size
- [ ] Crypto: session key exchange on first check-in (EKA — RSA/ECDH), AES-GCM for message bodies, per-message nonce, key rotation on re-check-in; replay protection; what happens if crypto fails — fail-closed (no command execution) or beacon-silent
- [ ] Beacon timing: configurable interval + jitter (random ±%), sleep command, long-sleep with wake-up (Windows: WaitableTimer/Event); measurement of periodicity entropy
- [ ] Build `code/beacon` (Go or your Track E language): HTTP check-in with encrypted task loop; `code/server`: minimal listener + task queue + result store; run agent on your own VM against the server
- [ ] TLS options: self-signed vs Let's Encrypt-ish; what JA3/JA4/JARM reveal about your client (feeds Track M); why a plaintext HTTP C2 is a lab-only toy
- [ ] HTTP profile: URL paths, user-agent, header order, response body camouflage (image/png-like or HTML padding) — the Malleable-C2 concept, hand-rolled

## Resources
- ired.team C2; Sliver source (`server/c2/` + `client/transport`); MalDev Academy implant-comm modules

## Exit Criteria
- [ ] Encrypted check-in → task → result round-trip over HTTP, no plaintext payloads — `labs/`
- [ ] Beacon jitter measured; periodicity not constant across 100 check-ins — `labs/`
- [ ] Explain in ≤5 lines your key-exchange + message-crypto design and its failure modes — `notes/`
- [ ] Your protocol spec written down (framing, types, crypto) — `notes/`

## Links
- [Sliver source](https://github.com/BishopFox/sliver)
- [ired.team C2](https://www.ired.team/offensive-security/command-and-control)
- [JA3](https://github.com/salesforce/ja3)
- [JARM](https://github.com/salesforce/jarm)
