# 24-36 · Own BitTorrent client — bencode, peers, piece selection (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../35-own-jpeg-decoder`](../35-own-jpeg-decoder/README.md) · **Next:** [`../37-own-chess-engine`](../37-own-chess-engine/README.md)

## Objective
The classic P2P build. A BitTorrent client: bencode, .torrent parsing, tracker announce, peer wire protocol, piece selection (rarest-first), choking/unchoking, download a real small torrent end-to-end. The security tie: your 24-29 DHT handles peer discovery, the swarm is your P2P lab, and malicious-torrent / fake-peer thinking pairs 24-29's Sybil work.

## Tasks
- [ ] bencode: encode/decode (the format discipline — pairs 24-11 varint, 24-25); .torrent parse (info hash, piece hashes)
- [ ] Tracker + peers: HTTP/UDP tracker announce, peer handshake, wire protocol (messages: interested/bitfield/piece)
- [ ] Transfer: piece selection (rarest-first), pipelining, choke/unchoke + tit-for-tat policy (the incentive design), verify SHA-1 per piece
- [ ] Lab: download a real small legal torrent (Linux ISO / your own seeded file); run against your own 24-29 DHT for discovery — `labs/`
- [ ] Writeup: the swarm economy (why tit-for-tat works), what poisoning/fake-peers would break — `notes/`

## Resources
- BEP 3/5/23 (the manuals); libtorrent source (peer); your 24-05/24-29 notes

## Exit Criteria
- [ ] Client downloads a real torrent piece-verified — `labs/`
- [ ] Choking policy + poisoning writeup — `labs/` + `notes/`

## Links
- [BEP 3](https://www.bittorrent.org/beps/bep_0003.html)
- [BEP 5 (DHT)](https://www.bittorrent.org/beps/bep_0005.html)
