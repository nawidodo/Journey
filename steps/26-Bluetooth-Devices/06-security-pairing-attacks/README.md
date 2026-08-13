# 26-06 · Bluetooth security — pairing attacks, spoofing, and the defender's view

**Week:** W41–43 · **Track:** R · **Prev:** [`../05-bt-classic-audio-spp`](../05-bt-classic-audio-spp/README.md) · **Next:** [`../07-capstone-turbo-pad`](../07-capstone-turbo-pad/README.md)

## Objective
Everything you built, from the attacker's seat — on your own hardware only. Why BLE devices get pwned at the pairing layer, and what the blue team sees (feeds Track M).

## Tasks
- [ ] Pairing attacks: **Passkey/Just Works MITM** (why Just Works has no MITM protection — an active relay between your gamepad and PC can inject or eavesdrop), legacy PIN brute force on classic; **pairing confusion** (attacker pairs first — "pairing race")
- [ ] Impersonation: MAC randomization spoofing — clone your own device's advertisement (same name/UUID/random MAC) and observe which OSes connect to the clone; the HID injection angle: fake BT keyboard
- [ ] Data exposure: sniff unprotected notify streams (your own devices from step 02–03 with `no security` mode) — see the report bytes in clear text
- [ ] Tools (all on owned targets): bettercap, Ubertooth, ESP32-BLE-Sniffer, `btlejack` (LL-level injection basics); gattacker-style GATT MITM if feasible
- [ ] Firmware side: what you must do as a device builder — mandatory security modes, MITM=True where possible, bonding with key confirmation, address randomization, "no security" only for public broadcast data
- [ ] Detection mirror: `notes/` — what a defender sees (pairing storms, unknown device names, HID-proxy signatures, RSSI anomalies); 3 Sigma-style rules for BT attacks (feeds Track M)
- [ ] BlueSmack/BlueBorne as history: why classic-stack RCE mattered (BlueBorne, 2017) and what it teaches about attack surface in dual-stack chips

## Resources
- HackTricks Bluetooth section, bettercap docs, Ubertooth docs
- NCC Group / LaNMaSteR53 BT security posts

## Exit Criteria
- [ ] Pairing MITM or spoof demo against your own device, documented — `labs/`
- [ ] Unprotected stream sniffed and decoded (own device) — `labs/`
- [ ] Explain in ≤5 lines why Just Works pairing is MITM-able and what the fix is — `notes/`
- [ ] 3 BT detection rules drafted — `code/`

## Links
- [HackTricks Bluetooth](https://book.hacktricks.xyz/wireless-technologies/bluetooth)
- [bettercap](https://www.bettercap.org/)
- [Ubertooth](https://github.com/greatscottgadgets/ubertooth)
- [btlejack](https://github.com/virtualabs/btlejack)
- [BlueBorne (Armis)](https://www.armis.com/blueborne/)
