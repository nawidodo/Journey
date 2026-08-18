# 24-58 · Own object storage — S3-lite: REST API over your KV + filesystem (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../57-own-ids-engine`](../57-own-ids-engine/README.md) · **Next:** [`../59-own-qr-codec`](../59-own-qr-codec/README.md) · **Pairs:** 24-17, 24-42, 24-15

## Objective
Every cloud disk is an object store. Build an S3-lite: REST API (your 24-17 HTTP server) over a metadata KV (24-42) + blob storage (24-15), with the S3 model — buckets, objects, keys, multipart upload, versioning-lite, ACLs. The security layer is the point: authn/authz (sigv4-lite — pairs 20-07 hashing), bucket-permission abuse (the misconfigured-bucket writeup), and why object stores are the #1 cloud breach surface.

## Tasks
- [ ] Model: bucket/object/key namespace, metadata (KV) + data (files), the split-brain design
- [ ] API: PUT/GET/DELETE/LIST via 24-17 HTTP, range reads, multipart upload (part ETags — pairs 20-12 hashing)
- [ ] Auth: sigv4-lite (canonical request + HMAC — pairs 20-07), per-bucket ACLs, anonymous access flag
- [ ] Security lab: public bucket → list/enumeration abuse (your own scanner 24-56); misconfig → write to public bucket; fix ACLs — `labs/`
- [ ] Self-check: s3cmd/aws-cli-lite client interoperates (real client, the oracle)

## Resources
- S3 REST API docs (the manual); MinIO source (peer); your 24-17/24-42/24-15 notes

## Exit Criteria
- [ ] Object store with auth + ACLs, real client interop — `labs/`
- [ ] Misconfiguration-abuse lab + writeup — `labs/` + `notes/`

## Links
- [S3 API](https://docs.aws.amazon.com/AmazonS3/latest/API/Welcome.html)
- [MinIO](https://min.io/)
