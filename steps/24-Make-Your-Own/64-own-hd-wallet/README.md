# 24-64 · Own HD wallet — BIP39 + BIP32 derivation, offline signing (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../63-own-raycaster`](../63-own-raycaster/README.md) · **Pairs:** 20-11, 20-12, 20-10

## Objective
You've built E2EE (20-11) and a password manager (20-12); now the key-management crown: a BIP39 HD wallet — mnemonic → seed (PBKDF2), BIP32 hierarchical derivation (the hardened/normal path math), address derivation + offline signing (ECDSA/secp256k1 — pairs 20-10 curve work), and the security engineering: seed phrase storage, zeroization, and the attack surface (phishing, clipboard, entropy — pairs 20-12 lessons). Everything offline, own-lab only.

## Tasks
- [ ] BIP39: wordlist (2048), mnemonic + checksum, seed via PBKDF2 (pairs 20-12 Argon2id discipline, 20-07 hashing)
- [ ] BIP32: master key, CKD (hardened `'` vs normal), the chain-code + index math; derive paths m/44'/0'/0'/0/0 — verify against a reference vector (the oracle)
- [ ] Signing: secp256k1 ECDSA (reuse 20-10), message digest, DER + recoverable signatures; verify
- [ ] Security lab: weak-entropy mnemonic → recoverable (the lesson); seed in memory → zeroize; clipboard/phishing threat model writeup — `labs/`
- [ ] Self-check: your wallet signs, a reference lib verifies (or vice versa)

## Resources
- BIP39/BIP32/BIP44 specs (the manuals); libsecp/trezor-crypto source (peer); your 20-10/20-11/20-12 code

## Exit Criteria
- [ ] Mnemonic→seed→derive→sign→verify with reference-vector match — `labs/`
- [ ] Entropy + threat-model writeup — `labs/` + `notes/`

## Links
- [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)
- [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
