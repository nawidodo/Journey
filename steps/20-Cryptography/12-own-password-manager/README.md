# 20-12 · Own password manager — vault format, KDF, offline-attack lab (stretch)

**Week:** W20 stretch · **Track:** L · **Prev:** [`../11-own-e2ee-messaging`](../11-own-e2ee-messaging/README.md)

## Objective
Build the vault you'd trust: encrypted blob (XChaCha20-Poly1305 or AES-GCM), master-key derivation (Argon2id — memory-hard, from your 20-01/08 lessons), key wrapping, autofill client. Then attack your own design: offline brute-force of a leaked vault (reuse 20-08 GPU skills), the KDF-parameter tradeoff table, and the "stretching cost vs GPU/ASIC" number that decides everything.

## Tasks
- [ ] Vault format: header (version, KDF params, salt), encrypted payload, MAC; entries (site/user/secret/metadata), zeroization on lock
- [ ] KDF: Argon2id parameters sweep — measure per-guess cost on CPU vs your 20-08 GPU cracker; the "1s login vs centuries offline" tuning — `labs/`
- [ ] Client: CLI + optional autofill shim; clipboard with auto-clear (the OS-clipboard leak, pairs 24-14 terminal OSC 52)
- [ ] Attack lab: leak a vault → offline attack with your own 20-08 GPU kernels at weak KDF params → succeeds; at strong params → infeasible; the table is your writeup
- [ ] Writeup: how 1Password/Bitwarden design vaults (KDF choices, secret keys), what breaks (master-password reuse, phishing) — `notes/`

## Resources
- Argon2 spec; XChaCha20-Poly1305 docs (pairs 20-01); Bitwarden/1Password security whitepapers (peer); your 20-01/08/11 notes

## Exit Criteria
- [ ] Vault encrypts/decrypts, zeroizes; clipboard auto-clear — `labs/`
- [ ] Offline-attack lab: weak-params cracked, strong infeasible, writeup — `labs/` + `notes/`

## Links
- [Argon2 RFC 9106](https://www.rfc-editor.org/rfc/rfc9106)
- [Bitwarden security whitepaper](https://bitwarden.com/help/bitwarden-security-white-paper/)
