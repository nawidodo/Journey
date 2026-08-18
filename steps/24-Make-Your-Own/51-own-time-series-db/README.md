# 24-51 · Own time-series DB — chunked compression, downsampling (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../50-own-logic-analyzer`](../50-own-logic-analyzer/README.md) · **Next:** [`../52-own-message-queue`](../52-own-message-queue/README.md)

## Objective
Every sensor, metric, and EDR event stream lands in a TSDB (Prometheus, InfluxDB, Timescale). Build one: timestamped series, chunked storage with delta + Gorilla-style compression (pairs 24-25 compression, 24-42 storage), downsampling/retention, query (time-range, aggregates). Feed it your own 05-14 eBPF events and 24-50 logic-analyzer captures — then graph them (your 24-41 or 10-05 renderer).

## Tasks
- [ ] Model: series + tags (the label model), chunk layout, WAL (pairs 24-42/24-02), compaction
- [ ] Compression: delta-of-delta timestamps + Gorilla XOR floats — measure ratio vs raw on real event data (your eBPF stream)
- [ ] Query: range scans, downsampling (min/max/avg buckets), retention policy; a tiny query language
- [ ] Lab: ingest 05-14 eBPF events (exec/connect), query "exec rate per minute", graph it; 24-50 captures as series — `labs/`
- [ ] Writeup: TSDB vs relational (why time is special), the Prometheus design (pairs 21-02 SIEM hunting) — `notes/`

## Resources
- Gorilla paper (the manual); Prometheus source (peer); your 24-25/24-42/05-14 notes

## Exit Criteria
- [ ] TSDB ingests real event streams, compresses, queries + downsamples — `labs/`
- [ ] Compression-ratio + design writeup — `labs/` + `notes/`

## Links
- [Gorilla paper](https://www.vldb.org/pvldb/vol8/p1816-teller.pdf)
- [Prometheus](https://prometheus.io/)
