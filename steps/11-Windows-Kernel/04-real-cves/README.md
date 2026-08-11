# 11-04 · Real Windows Kernel CVEs

**Week:** W36–40 · **Track:** D · **Prev:** [`../03-pool-overflow-token-stealing`](../03-pool-overflow-token-stealing/README.md) · **Next:** [`../05-mitigations-rop-ret2dir`](../05-mitigations-rop-ret2dir/README.md)

## Objective
Take the HEVD skill to shipped bugs, same re-derive rule as Linux Phase 5.

## Tasks
- [ ] CVE-2021-21551 (Dell dbutil, arbitrary read/write → EoP) — friendly intro, no heap
- [ ] CVE-2021-31956 (Windows kernel heap overflow, P0 analysis) — pool grooming in the wild
- [ ] Pick one more recent signed-driver bug (BYOVD: e.g. RTCore64 class) from loldrivers/Project Zero
- [ ] Each: patch-diff → root cause → exploit or POC → writeup

## Resources
- Project Zero blog (31956, 21551); loldrivers.io
- Reuse: patch-diffing workflow from your Linux kernel CVE steps

## Exit Criteria
- [ ] 2 real Windows CVEs re-derived (or full POC) — `labs/`
- [ ] One-pager per CVE: bug → primitive → technique — `notes/`

## Links
- [NVD search: Windows kernel](https://nvd.nist.gov/vuln/search/results?query=windows%20kernel&search_type=all)
- [ZDI advisories](https://www.zerodayinitiative.com/advisories/)
- [kernelCTF Windows track](https://github.com/google/security-research/tree/master/pocs/windows)
