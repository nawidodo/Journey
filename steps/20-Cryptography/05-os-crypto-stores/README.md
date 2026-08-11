# 20-05 · Crypto in Operating Systems: Keychains, Keystores, Full-Disk

**Week:** W18–19 · **Track:** L · **Prev:** [`../04-tls-internals`](../04-tls-internals/README.md) · **Next:** [`../06-capstone-crypto-weak-scheme`](../06-capstone-crypto-weak-scheme/README.md)

## Objective
Where crypto actually lives on your target platforms — the boundaries your exploitation (SEP, Keychain, Keystore, FileVault/BitLocker) will touch. Complements M5 and Track H's TEE work.

## Tasks
- [ ] Apple: Keychain, Data Protection classes (NSFileProtection), SEP + Secure Enclave key handling, FileVault
- [ ] Android: Keystore/StrongBox, per-app credential storage, TEE-backed keys (link Track H 16-05)
- [ ] Windows: DPAPI, BitLocker, Credential Guard/VBS (link Track D mitigations)
- [ ] Linux: dm-crypt/LUKS, kernel keyring, TPM (measured boot, seal/unseal)
- [ ] For each: what a compromise model looks like (where keys live, what survives a kernel rw primitive)
- [ ] Notes: "key location map" per OS — the cheat sheet Phase 7/Track D/H will use

## Resources
- Apple *Platform Security Guide* (Keychain/Data Protection/SEP chapters — revisit M5)
- Android Keystore docs; TPM2 spec summary; LUKS/dm-crypt docs
- Your M5 matrix + Track D `_EPROCESS`/token notes

## Exit Criteria
- [ ] Per-OS key-location + compromise map — `notes/`
- [ ] Explain one store's key lifecycle from memory
