# 24-116 · Own BLE scanner — CoreBluetooth on your Mac, no extra hardware (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Pairs:** 26-01/02, 24-110, 24-103

## Objective
Phase 26 studies BT without building; your Mac already has a radio: build a BLE scanner-lite natively — CoreBluetooth scan (peripheral discovery, RSSI + advertisement data parse — the AD structure: flags/name/service UUIDs, the format RE), connect + service/characteristic enumeration (the GATT walk — 26-02 theory made concrete), read/write/notify (the attribute protocol dance), and a target: your own phone or a low-cost BLE beacon you own — map its advertisement + GATT tree (lab-only, own devices). The security payload: advertisement spoofing demo (replay a captured AD structure — pairs 24-20/80 replay thinking), and the writeup on BLE attack surface (26-BT read steps: fuzzing/interface hijacks) grounded in your own observed packets.

## Tasks
- [ ] Scan: CBCentralManager, discovery + RSSI log, advertisement parse (company ID, service UUIDs, manufacturer data — the bytes)
- [ ] GATT: connect once, service/characteristic/descriptor walk, the UUID/type mapping, read/write/notify demo
- [ ] Target lab: your own phone/beacon — the full map (advertising + services + values) as an artifact — `labs/`
- [ ] Spoof lab: replay your target's advertising payload from your Mac and see it on your own scanner — the impersonation demo; mitigation note (bonding/user consent) — `labs/`
- [ ] Writeup: BLE attack surface reality check (26 steps: fuzzing, MITM) vs what your scanner observed; iOS/macOS limiting factors (pairing/bond persistence) — `notes/`

## Resources
- CoreBluetooth docs + Bluetooth Core Spec v5.x AD structure (the manual); your 26-01/02 notes

## Exit Criteria
- [ ] Live scan + GATT map of own device on own Mac — `labs/` + `code/`
- [ ] Ad-playload spoof demo + BLE-security writeup — `labs/` + `notes/`

## Links
- [CoreBluetooth](https://developer.apple.com/documentation/corebluetooth)
- [Bluetooth AD structure (Bluetooth SIG)](https://www.bluetooth.com/specifications/specs/core-specification/)