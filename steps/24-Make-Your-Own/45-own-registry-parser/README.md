# 24-45 · Own registry parser — hive format, offline NT-hash extraction (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../44-own-tpm-lab`](../44-own-tpm-lab/README.md) · **Next:** [`../46-own-mqtt-broker`](../46-own-mqtt-broker/README.md)

## Objective
Windows forensics runs on the registry: hive format (base blocks, bins, cells, nk/vk records), then the payoff — offline extraction of NT hashes from SAM (pairs 25-06 mimikatz internals, 21-04 disk artifacts, 24-32 NTFS). Every Windows artifact you'll ever hunt — persistence keys, Run keys, userassist, ShimCache — is a registry path.

## Tasks
- [ ] Hive format: base block, bins/cells, cell index math, nk (key) / vk (value) records, security/cache structures; parse a live hive copy
- [ ] Walk: key tree traversal (children, values, types, timestamps); dump to a registry-viewer-lite
- [ ] SAM payoff: parse SAM/SYSTEM hives → extract NT hashes (the bootkey derivation); verify offline (pairs 25-06, 20-08 GPU cracker)
- [ ] Forensics lab: on a Windows VM image — persistence keys (Run, services), userassist (program execution artifact), ShimCache; timeline — `labs/`
- [ ] Writeup: registry as anti-forensics target (12-08), where deleted keys leave cells — `notes/`

## Resources
- Microsoft's registry doc + the reverse-engineered hive spec (the manual); regipy source (peer); your 24-32 + 25-06 notes

## Exit Criteria
- [ ] Parser walks hives, dumps keys/values — `labs/`
- [ ] SAM NT-hash extraction + persistence/userassist forensics lab — `labs/` + `notes/`

## Links
- [Registry hive spec (woanware)](https://github.com/msuhanov/regf/blob/master/Windows%20registry%20file%20format%20specification.md)
- [regipy](https://github.com/mkorman90/regipy)
