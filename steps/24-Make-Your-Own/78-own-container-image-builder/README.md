# 24-78 · Own container image builder — layers, manifest, your runtime (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../77-own-voip`](../77-own-voip/README.md) · **Pairs:** 24-13, 24-15, 24-74, 24-39

## Objective
CI builds images, runtimes pull them: build the missing glue — an image builder-lite. Dockerfile-lite (parser — pairs 24-09): instructions → layer snapshots (your 24-15 filesystem diff), layer compression + content addressing (hashes — pairs 24-39), manifest + config (the OCI layout — a format RE, pairs 24-27 discipline), and a pull/run path into your own 24-13 runtime. The security layer: layer caching correctness (the hard invariant), image provenance (content-addressed = tamper-evident — pairs 24-58 signing), and the supply-chain story (base-image risk — pairs 24-39).

## Tasks
- [ ] Builder: Dockerfile-lite parse, build context, RUN/COPY semantics → per-layer filesystem diff (24-15)
- [ ] Layers: tar + gzip-lite (24-25), SHA-256 content addressing, dedup (same blob, one copy — the cache win)
- [ ] OCI: manifest/config JSON (format RE), the image index; your own registry (24-75) hosts it
- [ ] Runtime: your 24-13 runtime pulls + mounts the image (overlay-lite — pairs 24-15) and runs it
- [ ] Security lab: tamper a layer blob → hash mismatch rejected; base-image with a planted binary → supply-chain note; layer-cache staleness (stale base) → rebuild test — `labs/`

## Resources
- OCI image spec (the manual); Docker/buildkit source (peer — layer math); your 24-13/24-75/24-39 code

## Exit Criteria
- [ ] Dockerfile-lite → OCI image → runs in own 24-13 runtime — `labs/`
- [ ] Tamper-detection + cache-correctness writeup — `labs/` + `notes/`

## Links
- [OCI image spec](https://github.com/opencontainers/image-spec)
- [buildkit](https://github.com/moby/buildkit)