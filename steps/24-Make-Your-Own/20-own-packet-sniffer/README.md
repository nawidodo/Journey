# 24-20 · Own packet sniffer/crafter — tcpdump-lite from raw sockets (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../19-own-dns-resolver`](../19-own-dns-resolver/README.md)

## Objective
The syssec core skill: seeing packets. Build tcpdump-lite — raw sockets (AF_PACKET / BPF), Ethernet/IP/TCP/UDP decode, filter language (BPF-lite), pcap file writer; then a crafter — forge packets (ARP spoof, SYN, malformed flags). Pairs 21-07 Zeek parsing, 28 Wi-Fi captures, 05 network recon.

## Tasks
- [ ] Capture: raw socket → parse Ethernet/ARP/IPv4/IPv6/TCP/UDP/ICMP; hexdump + field view
- [ ] Filter: a mini-BPF (proto, src/dst, port) — compile to your own bytecode; pcap writer (files Zeek/tcpdump can read)
- [ ] Crafter: build + inject packets — ARP request/poison (own lab), SYN scan probe, malformed-length/checksum cases
- [ ] Attack/decode lab: capture your own 28-xx Wi-Fi test traffic, your 24-19 DNS queries — parse them with your own decoder
- [ ] Writeup: how BPF filters work (kernel-side copy), pcap file format RE (pairs 12-01) — `notes/`

## Resources
- tcpdump source (peer); libpcap docs; RFC 791/793; your 21-07 + 28 notes

## Exit Criteria
- [ ] Sniffer decodes own traffic, pcap readable by tcpdump — `labs/`
- [ ] Crafter injects ARP/SYN in own lab, writeup — `labs/` + `notes/`

## Links
- [libpcap/tcpdump](https://www.tcpdump.org/)
- [BPF docs](https://www.kernel.org/doc/Documentation/networking/filter.txt)
