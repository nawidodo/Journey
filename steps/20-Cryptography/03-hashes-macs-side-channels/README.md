# 20-03 · Hashes, MACs, and Side Channels

**Week:** W16–17 · **Track:** L · **Prev:** [`../02-asymmetric-rsa-ecc`](../02-asymmetric-rsa-ecc/README.md) · **Next:** [`../04-tls-internals`](../04-tls-internals/README.md)

## Objective
Hash/MAC internals and the attacks that survive modern designs — plus timing: the side channel you'll meet in real kernels and key checks.

## Tasks
- [ ] Merkle–Damgård vs SHA-3; why SHA-1/MD5 collisions broke; length-extension attack (SHA-1/2) — build it — `labs/`
- [ ] MACs: HMAC, CBC-MAC pitfalls; keyed vs unkeyed hashing mistakes
- [ ] Timing side channels: why `==` on hashes leaks; implement a timing-safe compare and a timing attack on a victim — `labs/`
- [ ] Password storage: PBKDF2/bcrypt/Argon2; salt; "hashcat mindset" (weak schemes die fast)
- [ ] CryptoPals Set 4 (SHA-1 length extension, HMAC timing leak)

## Resources
- *Serious Cryptography* ch.7 (hashes), ch.9 (secure randomness)
- Cryptopals set 4; "Attacking Crypto" course notes (Filić)
- Kernel link: `crypto/` in Linux — constant-time memcmp (`crypto_memneq`)

## Exit Criteria
- [ ] Length-extension + timing attack working — `labs/`
- [ ] Notes: hash/MAC choice matrix for your future implants — `notes/`

## Links
- [CryptoPals set 4](https://cryptopals.com/sets/4)
- [constant-time compare (Linux crypto_memneq)](https://github.com/torvalds/linux/blob/master/lib/crypto/memneq.c)
