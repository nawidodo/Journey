# 26-05 · Bluetooth classic — audio (A2DP) + serial (SPP), the other radio

**Week:** W40–42 · **Track:** R · **Prev:** [`../04-host-side-sniffing`](../04-host-side-sniffing/README.md) · **Next:** [`../06-security-pairing-attacks`](../06-security-pairing-attacks/README.md)

## Objective
BLE covers control; classic BT covers audio and legacy serial. Build one classic-profile device so you know both radios — a speaker (A2DP sink) and a serial bridge (SPP), and understand why the two stacks coexist in every phone.

## Tasks
- [ ] Classic stack: BR/EDR radio (79 channels, FHSS), piconet roles, SCO/eSCO for audio, the HCI/ACL paths; how classic and BLE share one antenna on modern chips (dual-mode controllers)
- [ ] A2DP sink: ESP32-A2DP or nRF5340 classic sample — board plays audio streamed from your phone; decode the SBC codec transport; why A2DP is one-directional and where AVRCP (metadata/control) fits
- [ ] SPP bridge: UART → Bluetooth serial (ESP32 SPP or nRF UART service) — phone terminal ↔ board; the profile that powers Arduino BT modules and legacy barcode scanners
- [ ] Classic pairing: legacy PIN pairing vs SSP (Numeric Comparison) — do both, observe in Wireshark (BR/EDR capture: Ubertooth or supported dongle)
- [ ] Latency/quality tradeoffs: A2DP latency vs BLE isochronous channels (LE Audio — what it is, why it replaces A2DP eventually); note where LE Audio stands in 2026
- [ ] `notes/`: decision table — when to pick classic vs BLE (audio, HID, data streaming, battery, pairing UX), and the dual-mode reality on consumer devices

## Resources
- ESP32-A2DP examples, Nordic `audio` samples (LE Audio), BlueZ `bluetoothctl` classic pairing flow
- Bluetooth Core Spec A2DP/AVRCP/SPP profiles

## Exit Criteria
- [ ] A2DP speaker + SPP bridge both working — `labs/`
- [ ] One classic pairing captured and the SSP method identified — `labs/`
- [ ] Explain in ≤5 lines why audio stayed on BR/EDR and what LE Audio changes — `notes/`

## Links
- [ESP32-A2DP](https://github.com/pschatzmann/ESP32-A2DP)
- [Nordic LE Audio samples](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/bluetooth/audio/README.html)
- [A2DP spec](https://www.bluetooth.com/specifications/specs/advanced-audio-distribution-profile-1-4/)
- [HackTricks Bluetooth classic](https://book.hacktricks.xyz/wireless-technologies/bluetooth)
