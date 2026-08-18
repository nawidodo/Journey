# 24-16 · Own memory allocator — malloc from scratch, heap-exploitation payoff (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../15-own-filesystem`](../15-own-filesystem/README.md) · **Next:** [`../17-own-http-server`](../17-own-http-server/README.md)

## Objective
Write malloc: bump allocator → free-list (first-fit/best-fit) → segregated bins, mmap for large sizes, arena/reuse patterns. The security payoff is the point: heap exploitation (03-xx UAF/overflow classes) becomes *your* metadata you're corrupting — tcache poisoning, freelist corruption, unlink attacks against your own allocator.

## Tasks
- [ ] v1: bump allocator (no free); v2: free-list with coalescing, first/best-fit; v3: size-segregated bins (tcache-ish)
- [ ] Overhead: header layout, alignment, chunk sizes; measure fragmentation vs glibc on a workload
- [ ] Attack lab: against your own allocator — overflow into next-chunk header, freelist poisoning, double-free; each as a working exploit vs your code (safe, own lab) — `labs/`
- [ ] Defense: hardened allocator — free-list integrity checks (glibc safe-linking shape), guard pages; re-run attacks, now blocked
- [ ] Writeup: how glibc/musl differ, what exploits target (03-xx cross-ref) — `notes/`

## Resources
- glibc malloc internals (the classic writeups); dlmalloc source; your 03 exploitation notes

## Exit Criteria
- [ ] Working allocator + fragmentation measurement — `labs/`
- [ ] Attack → hardened → re-attack blocked, writeup — `labs/` + `notes/`

## Links
- [glibc malloc internals](https://sourceware.org/glibc/wiki/MallocInternals)
- [dlmalloc](https://gee.cs.oswego.edu/dl/html/malloc.html)
