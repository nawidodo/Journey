# 26-01 · Bluetooth stack — BR/EDR vs BLE, GAP/GATT, pairing, HCI

**Week:** W36–38 · **Track:** R · **Prev:** — · **Next:** [`../02-first-peripheral-toolchain`](../02-first-peripheral-toolchain/README.md)

## Objective
Understand the stack you'll build on before touching a board: the two radios, the profile/attribute model, the security model, and where the host/controller split lives. Same discipline as Track F's protocol reading.

## Tasks
- [ ] Radio/physical: 2.4 GHz ISM, 40 BLE channels (3 advertising + 37 data), adaptive frequency hopping, why BR/EDR (79 channels) survives for audio — vs BLE for data/control
- [ ] BR/EDR vs BLE: piconet/scatternet vs connectionless advertising; classic profiles (A2DP/AVRCP/SPP/HSP) vs BLE profiles (GATT-based); power/latency tradeoffs — which you'd pick for a gamepad vs a speaker
- [ ] BLE protocol layers: LL → HCI → L2CAP → SMP → ATT → GATT → GAP; advertising packet anatomy (AD structures, flags, service UUIDs), connection events
- [ ] GATT: server/client, characteristics (read/write/notify/indicate), services + UUIDs (16-bit vs 128-bit), descriptors, CCCD — the data model every BLE device you build is expressed in
- [ ] GAP: roles (peripheral/central, broadcaster/observer), advertising interval + duty cycle, connection parameters (interval/latency/timeout)
- [ ] Security: legacy pairing vs **LE Secure Connections**, Just Works vs Passkey vs OOB, MITM protection levels, bonding + keys, privacy (address randomization), why many real devices pair insecure (and what an attacker does with that — notes for step 06)
- [ ] HCI: controller vs host split — where firmware ends and the OS BT stack begins; what a sniffer actually sees at LL vs what the host sees at HCI
- [ ] Diagram in `notes/`: one packet from button press on a BLE gamepad → HCI → L2CAP → GATT → app, with the stack layers labeled

## Resources
- Bluetooth Core Spec (v5.x, chapters 1–6 skim), learn from: *Getting Started with Bluetooth LE* (Heikki Ala-Laurila/Kevin Townsend), Nordic's "Bluetooth Fundamentals" academy
- Microchip/Nordic protocol whitepapers

## Exit Criteria
- [ ] Stack diagram from memory: BLE layers + one packet's path — `notes/`
- [ ] Explain in ≤5 lines the difference between classic Bluetooth and BLE at the *radio and profile* level — `notes/`
- [ ] Table in `notes/`: 6 common devices → which radio/profile they use and why (gamepad, speaker, earbuds, fitness band, OBD dongle, smart lock)

## Links
- [Bluetooth Core Spec](https://www.bluetooth.com/specifications/specs/core-specification-5-4/)
- [Nordic Bluetooth academy](https://academy.nordicsemi.com/courses/bluetooth-low-energy-fundamentals/)
- [Getting Started with Bluetooth LE](https://github.com/amzn/blue-the-book)
- [HackTricks Bluetooth](https://book.hacktricks.xyz/wireless-technologies/bluetooth)
