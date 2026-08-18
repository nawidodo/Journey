# 24-80 · Own packet replayer — tcpreplay-lite for your IDS lab (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../79-own-malware-ml-detector`](../79-own-malware-ml-detector/README.md) · **Next:** [`../81-own-unikernel`](../81-own-unikernel/README.md) · **Pairs:** 24-20, 24-57, 24-56

## Objective
Your 24-57 IDS needs attack traffic on demand; your 24-20 sniffer records it. Build the test tool: pcap reader (24-20 format), replay at configurable rate + timing (inter-packet gap — the honest replay problem: timing fidelity), loop with scenarios, and tight integration — replay your captured scanner sweep / reverse-shell session whenever you modify rules (the regression harness for 24-57). Also the fun part: replay at 10x and watch your IDS engine lose packets (the capacity measurement, pairs 24-30).

## Resources
- tcpreplay design notes (the manual); libpcap/tcpdump format (your 24-20); your 24-20/24-57/24-56 code

## Tasks
- [ ] Pcap: read/write (24-20 format), filter streams by flow (the scenario builder)
- [ ] Replay: raw-socket send (24-20), per-packet delay for rate control, loop + scripted scenarios
- [ ] Harness: `replay scenario → check IDS alerts` regression script for your 24-57 rules
- [ ] Capacity lab: 1x vs 10x replay against your IDS engine — packet-loss + alert-miss curve (pairs 24-30)
- [ ] Writeup: timing fidelity + why replay ≠ live (NIC features, offload) — `notes/`

## Exit Criteria
- [ ] Scenario replay drives your IDS consistently; capacity curve measured — `labs/` + `code/`
- [ ] Timing-fidelity writeup — `notes/`

## Links
- [tcpreplay](https://tcpreplay.appneta.com/)
- [pcap-savefile format](https://wiki.wireshark.org/Development/LibpcapFileFormat)