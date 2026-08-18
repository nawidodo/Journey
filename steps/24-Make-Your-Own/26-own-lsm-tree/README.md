# 24-26 · Own LSM-tree — memtable → SST → compaction, the hot DB structure (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../25-own-compression`](../25-own-compression/README.md) · **Next:** [`../27-own-png-decoder`](../27-own-png-decoder/README.md)

## Objective
Your 24-02 DB used a B-tree. The other half of modern storage is the LSM-tree (RocksDB, LevelDB, Cassandra, every write-heavy store): memtable (skip-list) → sorted string tables → leveled compaction → bloom filters. Build it, benchmark vs your 24-02 B-tree: the write-amplification vs read-amplification tradeoff becomes hands-on.

## Tasks
- [ ] Memtable: skip-list (your first nontrivial data structure beyond B-tree) with WAL append for crash safety
- [ ] SST: sorted runs, block format, index + bloom filters (the "definitely-not-in-this-file" check)
- [ ] Compaction: level-0 → leveled, size-tiered options; measure write/read amplification on a workload
- [ ] Benchmark: LSM vs your 24-02 B-tree (write-heavy vs read-heavy) — the tradeoff graph; also vs SQLite WAL
- [ ] Writeup: why BigTable/RocksDB chose LSM, when B-tree wins — `notes/`

## Resources
- the LSM paper (O'Neil et al.); RocksDB docs; LevelDB source (peer); your 24-02/24-11 notes

## Exit Criteria
- [ ] LSM store with memtable/SST/compaction/bloom — `labs/`
- [ ] Amplification benchmark + tradeoff writeup — `labs/` + `notes/`

## Links
- [LSM paper](https://www.cs.umb.edu/~poneil/lsmtree.pdf)
- [RocksDB](https://rocksdb.org/)
