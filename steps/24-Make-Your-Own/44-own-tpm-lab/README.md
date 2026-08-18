# 24-44 · Own TPM lab — PCRs, sealing, remote attestation (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../43-systems-gauntlet`](../43-systems-gauntlet/README.md)

## Objective
Your 24-31 bootloader and 18-04 bootkit work shaped the boot-integrity problem; the TPM is the hardware answer. Run a TPM 2.0 (software `swtpm` + real device if you have one): PCR extend/quote, sealed keys (unseal only when measurements match), and a remote-attestation-lite flow. The security lesson: what TPMs actually guarantee (tamper evidence, not tamper proof) and the boot-measurement chain that pairs 24-31 Secure Boot.

## Tasks
- [ ] Stack: swtpm + tpm2-tools; PCR extend semantics (why extend, not overwrite); read your boot PCRs
- [ ] Seal/unseal: create a sealed key tied to PCR state — unseals only under the right measurements; change state → unseal fails
- [ ] Attestation: quote (PCRs + nonce) with an AIK, verify signature; the challenger/verifier flow (pairs 20-02 keys, 24-39 signing)
- [ ] Lab: boot-chain measurement — your own 24-31 bootloader extends PCRs (or simulate); tamper one stage → attestation fails — `labs/`
- [ ] Writeup: TPM vs "the attacker owns the OS" reality, Windows Secure Boot/Measured Boot, BitLocker's TPM use — `notes/`

## Resources
- TPM 2.0 spec + tpm2-tools docs (the manual); the "TPM in practice" tutorials; your 24-31/18-04/20-02 notes

## Exit Criteria
- [ ] PCR quote + sealed key unseal/unseal-fail flow working — `labs/`
- [ ] Boot-chain tamper → attestation fail + writeup — `labs/` + `notes/`

## Links
- [tpm2-tools](https://github.com/tpm2-software/tpm2-tools)
- [Trusted Computing spec](https://trustedcomputinggroup.org/resource/tpm-library-specification/)
