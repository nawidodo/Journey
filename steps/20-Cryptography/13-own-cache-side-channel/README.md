# 20-13 · Own cache side-channel — Flush+Reload / Prime+Probe lab (stretch)

**Week:** W20 stretch · **Track:** L · **Prev:** [`../12-own-password-manager`](../12-own-password-manager/README.md)

## Objective
Side channels are where crypto dies quietly. Build the measurement lab: Flush+Reload and Prime+Probe against *your own* victim (a constant-time-correct vs constant-time-broken AES/S-box implementation, or a password compare — reuse your 20-12). Extract a key byte / guess count from timing. Then the defense: constant-time code + why compiler optimizations keep re-breaking it.

## Tasks
- [ ] Primitive: precise timing (rdtsc), Flush+Reload (shared memory line), Prime+Probe (own cache set); calibration on your CPU
- [ ] Victim: write a deliberately non-constant-time compare (early-exit — the 20-12 lesson reversed); recover secret position/bytes via timing
- [ ] Variant: cache-timing AES S-box — recover a byte of the round key (the classic 2005 Bernstein attack shape)
- [ ] Defense: constant-time compare (bitwise OR chains), show timing flat; `ctgrind`-style static note
- [ ] Writeup: which crypto actually leaks (pairs 20-01/04), hardware counters, remote vs local timing — `notes/`

## Resources
- Bernstein's AES timing paper (the manual); "Cache attacks" (Yarom's Flush+Reload paper); your 20-12/24-30 notes

## Exit Criteria
- [ ] Flush+Reload recovers secret from own victim; constant-time fix flattens timing — `labs/`
- [ ] Key-recovery + writeup — `labs/` + `notes/`

## Links
- [Flush+Reload (Yarom)](https://eprint.iacr.org/2013/448.pdf)
- [Bernstein AES timing](https://cr.yp.to/antiforgery/cachetiming-20050414.pdf)
