# 20-11 · Own E2EE messaging — X3DH + Double Ratchet from scratch (stretch)

**Week:** W20 stretch · **Track:** L · **Prev:** [`../10-toy-zk-snark`](../10-toy-zk-snark/README.md)

## Objective
Build the crypto that every messenger ships: X3DH key agreement (signed prekey bundle) + Double Ratchet (per-message keys, forward secrecy, post-compromise recovery). Own lab only — two local peers, no cloud. The security lessons: what forward secrecy actually buys, why the ratchet recovers after compromise, where real implementations leak (header parsing, padding).

## Tasks
- [ ] X3DH: prekey bundle (identity/signed/prekey/one-time), DH derivation, associated data binding; MITM-think: bundle trust problem documented
- [ ] Double Ratchet: KDF chain (symmetric ratchet) + DH ratchet on message headers; send/receive state machines, skipped-message keys
- [ ] Transport: messages over your own 24-12 SSH-server channel or plain TCP; wire format + padding discipline (length-leak think, pairs 20-04/07)
- [ ] Security lab: compromise simulation — leak a state, show past messages stay secret (forward secrecy) and future ones recover; tampered/replayed messages rejected
- [ ] Writeup: where real Signal/WhatsApp diverged or got attacked (pairing, plaintext metadata) — `notes/`

## Resources
- The X3DH + Double Ratchet specs (Signal docs — the manual); your 20-03/07 notes

## Exit Criteria
- [ ] Two local peers exchange messages; compromise simulation: past secret, future recovered — `labs/`
- [ ] Tamper/replay rejected + writeup — `labs/` + `notes/`

## Links
- [Signal Double Ratchet spec](https://signal.org/docs/specifications/doubleratchet/)
- [X3DH spec](https://signal.org/docs/specifications/x3dh/)
