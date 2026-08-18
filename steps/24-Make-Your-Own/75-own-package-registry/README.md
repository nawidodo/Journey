# 24-75 · Own package registry — npm-lite: publish, resolve, supply chain (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../74-own-ci-runner`](../74-own-ci-runner/README.md) · **Pairs:** 24-39, 24-58, 24-17

## Objective
Your 24-39 package manager is a client born for a server. Build the other half: a registry — publish (tarball store — reuse 24-58 object storage ideas), versioned metadata (semver resolution — the resolver you debugged client-side), the public API (your 24-17 HTTP server), and the supply-chain security layer: integrity (pairs 24-39 tamper/confusion/downgrade lessons server-side), signing-lite (20-10), and a **typosquat/prototype-pollution lab** (publish a poisoned package, watch your own manager resolve it, then fix: allowlist + verification).

## Tasks
- [ ] Store: package tarball + metadata (24-58 layout), version listing, publish/delete flows
- [ ] Resolve: dependent-resolution server-side (semver ranges — pairs 24-09 parsing), lockfile handoff
- [ ] Auth/signing: publish tokens (20-12), package signature + verify (20-10), owner ACL
- [ ] Security lab: typosquat package (own packagename-vs-packagename) → your 24-39 resolves it → detection (allowlist/hash-pin) fixes; prototype-pollution package test — `labs/`
- [ ] Interop: your 24-39 installs from your registry (the oracle)

## Resources
- npm registry API docs (the manual); your 24-39/24-58/24-17 code

## Exit Criteria
- [ ] Publish → resolve → install roundtrip with own registry — `labs/`
- [ ] Typosquat/prototype-pollution detection + fix — `labs/` + `notes/`

## Links
- [npm registry API](https://github.com/npm/registry/blob/main/docs/REGISTRY-API.md)
- [Verdaccio](https://verdaccio.org/)