# 18-04 · UEFI Bootkit

**Week:** W49–51 · **Track:** J · **Prev:** [`../03-windows-driver-rootkit`](../03-windows-driver-rootkit/README.md) · **Next:** [`../05-macos-rootkit-kext-dext`](../05-macos-rootkit-kext-dext/README.md)

## Objective
Persistence below the OS — ESP/NVRAM bootkit (BlackLotus/LoJax class). **OVMF/QEMU VM only** — SPI/NVRAM persistence on real hardware can brick the machine.

## Tasks
- [ ] UEFI boot chain: SEC → PEI → DXE → BDS; where a bootkit lands
- [ ] ESP payload + NVRAM variables; how BlackLotus bypassed Secure Boot
- [ ] Build a LoJax-style NVRAM/ESP persistence bootkit in QEMU/OVMF
- [ ] Defensive mirror: Secure Boot audit, UEFI event log, boot-time scan

## Resources
- BlackLotus writeups (ESET, SentinelOne); UEFI spec; efi-mimikatz; QEMU/OVMF

## Exit Criteria
- [ ] Working UEFI bootkit in OVMF VM — `labs/`

## Links
- [BlackLotus analysis (ESET)](https://www.welivesecurity.com/en/eset-research/blacklotus/)
- [UEFI spec](https://uefi.org/specifications)
