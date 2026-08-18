# 24-12 · Own SSH server — protocol + crypto + auth, then pre-auth attack surface (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../11-own-sqlite-file-format`](../11-own-sqlite-file-format/README.md) · **Next:** [`../13-own-container-runtime`](../13-own-container-runtime/README.md)

## Objective
SSH is half the internet's plumbing. Build a server: binary packet layer, KEX (curve25519), host keys, channels, auth (password → pubkey). Then the security payoff — flip to the attacker: pre-auth attack surface, version-rollback, KEX downgrade, padding-oracle thinking. Test by logging into your own server with real `ssh`.

## Tasks
- [ ] Transport: binary packet protocol (padding, MAC, sequence numbers — why the invariant matters), version exchange; interop: real `ssh` client connects
- [ ] KEX: curve25519-sha256, key derivation (pairs 20-07 transcript-hash discipline); host-key verify path
- [ ] Auth: password (timing-safe compare — the constant-time lesson), then pubkey (authorized_keys, verify vs real ssh-keygen output)
- [ ] Channels: session/exec, env, pty — your server runs a real shell over your protocol
- [ ] Attack pass: KEX downgrade / weak-alg negotiation, version rollback, padding-oracle probe; write up what a pre-auth bug class looks like (the OpenSSH 2024 regreSSHion shape) — `notes/`
- [ ] Self-check: `ssh -o KexAlgorithms=...` negotiation matrix logged; auth brute-force attempts rate-limited

## Resources
- RFC 4251–4253; OpenSSH source (peer); your 20-07 TLS notes (same protocol discipline)

## Exit Criteria
- [ ] Real `ssh` client runs a shell on your server — `labs/`
- [ ] Pre-auth attack-notes + negotiation matrix — `notes/`

## Links
- [RFC 4253 (transport)](https://www.rfc-editor.org/rfc/rfc4253)
- [OpenSSH](https://github.com/openssh/openssh-portable)
