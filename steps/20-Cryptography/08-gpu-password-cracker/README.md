# 20-08 · GPU password cracker — PBKDF2/bcrypt on CUDA + Metal (stretch)

**Week:** W20+ stretch (pairs 10-14) · **Track:** L/C · **Prev:** [`../07-own-tls`](../07-own-tls/README.md)

## Objective
Hashcat on GPU is magic until you write the kernel. Implement PBKDF2 (SHA-256) and bcrypt kernels — CUDA on Colab T4 (10-14) + Metal on your Mac — benchmark vs hashcat on the same hashes, and learn why bcrypt's design (memory-independent, 1KB state, many rounds) makes it GPU-hostile by construction.

## Tasks
- [ ] PBKDF2-SHA256: hash chain + key-stretching math (pairs 20-01/03); CUDA kernel: one thread per candidate password, incremental SHA-256 update (the not-naive way)
- [ ] bcrypt: 1KB Blowfish state init (EksBlowfishSetup), 64 rounds; why it's memory-light but round-heavy — GPU vs CPU throughput gap measurement
- [ ] Cracking harness: candidate generator (mask + dictionary + mangling), comparison logic; small test hashset with known answers (the runnable check)
- [ ] Benchmark: your kernels vs hashcat (`-b` / same hashes) — honest table; where you lose and why (the lesson: hashcat is a decade of optimization)
- [ ] Self-check: your cracker recovers the test-set passwords (known list); Metal vs CUDA timing compared

## Resources
- RFC 8018 (PBKDF2); bcrypt paper; hashcat benchmark docs; your 10-14 CUDA + 20-01 crypto notes

## Exit Criteria
- [ ] PBKDF2 + bcrypt kernels crack the test set — `labs/`
- [ ] Benchmark table vs hashcat, divergence explained — `notes/`

## Links
- [hashcat](https://hashcat.net/hashcat/)
- [RFC 8018](https://www.rfc-editor.org/rfc/rfc8018)
