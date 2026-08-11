# 20-01 · Symmetric Ciphers + Modes of Operation

**Week:** W14–15 · **Track:** L · **Prev:** — · **Next:** [`../02-asymmetric-rsa-ecc`](../02-asymmetric-rsa-ecc/README.md)

## Objective
Know block ciphers and modes cold — and the classic attacks on how they're used. Padding-oracle is the first "crypto vulnerability research" skill.

## Tasks
- [ ] Block ciphers: AES (round structure, key schedule), why ECB leaks; stream ciphers (ChaCha) vs block
- [ ] Modes: CBC, CTR, GCM — confidentiality vs AEAD; what each protects and doesn't
- [ ] **Padding-oracle attack** (CBC + PKCS#7): build the byte-by-byte decryption yourself — `labs/`
- [ ] Bit-flipping attacks on CBC/CTR (alter plaintext without key)
- [ ] IV/reuse mistakes: fixed IV, nonce reuse (CTR) — demonstrate with a lab
- [ ] CryptoPals Set 1–2 (CryptoPals = the pwn.college of crypto)

## Resources
- *Serious Cryptography* (Aumasson) ch.1–4 — the book
- cryptopals.com (sets 1–2)
- Padding oracle: "This is the way" (robertheaton) / classic blog posts

## Exit Criteria
- [ ] Padding-oracle decrypts a target ciphertext without the key — `labs/`
- [ ] Mode-selection notes (when GCM, when CTR+CBC) — `notes/`

## Links
- [CryptoPals (sets 1-2)](https://cryptopals.com/)
- [Padding-oracle attack writeup](https://robertheaton.com/2013/07/29/padding-oracle-attack/)
