# 24-39 · Own package manager — format, deps, signing, supply-chain lab (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../38-own-text-editor`](../38-own-text-editor/README.md)

## Objective
Supply chain is the era's #1 software security problem (xz, SolarWinds, left-pad). Build a package manager: archive format, dependency resolution + versioning, integrity hashes + signing/verification (your 20-02 keys), a local registry. Then attack your own chain: a malicious "upgrade" — your verifier must catch it (hash mismatch, bad signature, dependency confusion).

## Tasks
- [ ] Format: package archive (tar-like — or reuse your 24-25 deflate), metadata (name/version/deps), local registry + index
- [ ] Resolver: dependency graph, version ranges, lockfile (reproducibility — pairs 24-03 git); the resolution algorithm
- [ ] Integrity: per-file hashes + signed manifest (Ed25519 from 20-02); verify before install, refuse mismatch
- [ ] Attack lab: malicious package (tampered bytes), dependency confusion (same name, wrong registry), downgrade attack — your chain rejects all; writeup on real incidents — `labs/`
- [ ] Self-check: install → verify → run; tamper → refused; the supply-chain mindset is the deliverable

## Resources
- The "supply chain attacks" writeups (xz backdoor analyses); pacman/dpkg design docs (peer); your 20-02 + 24-25 notes

## Exit Criteria
- [ ] Manager installs verified packages, lockfile reproducible — `labs/`
- [ ] Tamper/confusion/downgrade all rejected, writeup — `labs/` + `notes/`

## Links
- [xz backdoor analysis (GitHub Advisory)](https://github.com/advisories/GHSA-5r4g-2vjp-v37c)
- [The Dependency Confusion paper](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)
