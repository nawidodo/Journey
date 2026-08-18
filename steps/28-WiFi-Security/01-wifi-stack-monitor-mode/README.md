# 28-01 · 802.11 stack + monitor mode — learn to read the air

**Week:** W28–30 · **Track:** T · **Prev:** — · **Next:** [`../02-wpa2-wpa3-attacks`](../02-wpa2-wpa3-attacks/README.md)

## Objective
Read the air like you read a pcap on the wire. 802.11 architecture, frame types, monitor mode, and a decoded association + 4-way handshake from your own capture.

## Tasks
- [ ] 802.11 architecture: PHY/MAC split, DCF/CSMA-CA, 2.4/5/6 GHz, channels; frame types (management/control/data) and the association state machine (probe → auth → assoc)
- [ ] Monitor mode + the aircrack-ng suite: `airmon-ng`, `airodump-ng`, `aireplay-ng`, `aircrack-ng`; what monitor mode changes at the driver/MAC layer (no 802.11 header stripping, no ACKs)
- [ ] Hardware: one monitor-mode-capable USB adapter (Alfa-class or equivalent); note chipset (Atheros/Ralink/MediaTek) and driver quirks
- [ ] Own capture: beacon, probe request/response, auth, assoc, and the full 4-way handshake from your own AP/client — decode each frame in Wireshark
- [ ] Hidden SSIDs and client probing: why probes leak client history, and what a defender sees
- [ ] Channel hopping + 5 GHz scan; save labeled pcaps in `labs/`

## Resources
- aircrack-ng docs; IEEE 802.11-2020 spec (frame format chapters); Wireshark 802.11 dissector docs

## Exit Criteria
- [ ] Full association + 4-way handshake decoded frame-by-frame from your own capture — `notes/` + `labs/` pcap
- [ ] Explain in ≤5 lines why WPA2-PSK offline cracking is feasible but WPA3-SAE is not (what the handshake change is) — `notes/`

## Links
- [aircrack-ng](https://www.aircrack-ng.org/)
- [Wireshark 802.11](https://wiki.wireshark.org/WLAN)
