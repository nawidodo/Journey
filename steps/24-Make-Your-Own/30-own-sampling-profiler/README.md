# 24-30 · Own sampling profiler — perf-lite from scratch (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../29-own-kademlia-dht`](../29-own-kademlia-dht/README.md)

## Objective
The performance gap: nobody else in the plan builds measurement tools. Write a sampling profiler: SIGPROF/`perf_event_open` periodic sampling, stack-walking (frame pointers + unwind), symbolization, flamegraph output. The syssec tie: profiling is the same skill as RE-instrumentation (pairs 15-07 debugger, 19 hooking) — and EDRs do stack-walking for detection (21-06).

## Tasks
- [ ] Sampler: timer (SIGPROF or perf_event_open) interrupting the target; PC + stack capture (frame-pointer walk; DWARF unwind stretch)
- [ ] Symbolization: map addresses → symbols (ELF symtab; dladdr); demangle
- [ ] Output: call-tree aggregates, flamegraph SVG — find the real hotspot of a test program (a known-slow function you planted)
- [ ] Accuracy: sampling error, signal bias, inlining lies — cross-check with Instruments/`perf`
- [ ] Security tie-in: stack-walk as detection primitive — a mini EDR-style hook that flags deep-known-stack patterns (pairs 21-06) — `labs/`

## Resources
- `perf`/Instruments docs; Brendan Gregg's flamegraph tooling; your 15-07 + 19-01 notes

## Exit Criteria
- [ ] Profiler finds the planted hotspot; flamegraph renders — `labs/`
- [ ] Cross-check vs perf + EDR-stack-walk demo — `labs/` + `notes/`

## Links
- [perf_event_open man](https://man7.org/linux/man-pages/man2/perf_event_open.2.html)
- [Brendan Gregg — flamegraphs](https://github.com/brendangregg/FlameGraph)
