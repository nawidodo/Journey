# 26-07 · Capstone — BLE HID turbo gamepad, full device 🚩 M26

**Week:** W42–46 · **Track:** R · **Prev:** [`../06-security-pairing-attacks`](../06-security-pairing-attacks/README.md)

## Objective
Ship the device the whole track was building toward: a real **Bluetooth HID gamepad with hardware turbo** — 14-07's turbo logic and 03's HOG gamepad merged into one product-grade peripheral, with secure pairing, working on all your OSes. This is the "build a Bluetooth device" ask, end to end.

## Tasks
- [ ] Hardware: nRF52840 (or ESP32-C3) + buttons/analog sticks + battery power path (or USB); enclosured prototype — actual device, not dev board if you can manage a perfboard/3D print
- [ ] Firmware: HOG gamepad (03) + firmware turbo (03) + secure pairing (06: MITM-capable where host supports, bonding, privacy) + low-power (connection interval tuning, sleep)
- [ ] OTA/DFU as the "real product" requirement — update firmware over the air, verify rollback safety
- [ ] Host matrix: play a real game on Linux + macOS + Windows; latency measured on each; turbo rate verified over the air via sniffer
- [ ] Docs: wiring + BOM, report map, pairing instructions, per-OS quirks — `notes/`
- [ ] Security review of your own device with step 06 checklist: what's protected, what isn't, what a local attacker can do — honest writeup in `notes/`
- [ ] Hand off: sniffed capture + firmware to Track M as detection practice target (feeds Track M)

## Resources
- Steps 01–06 outputs; Nordic/Zephyr or ESP-IDF docs; Track G 14-07 (report map + turbo core)

## Exit Criteria
- [ ] Working BLE HID gamepad with hardware turbo, plays on ≥2 OSes, secure pairing on — `labs/`
- [ ] OTA firmware update demonstrated — `labs/`
- [ ] Full writeup: architecture, security review, per-OS results — `notes/`
- [ ] Explain in ≤5 lines the full data path of a turbo'd button press, button → air → host → game — `notes/`

## Links
- [Nordic HID samples](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/bluetooth/peripheral_hids_keyboard/README.html)
- [Zephyr Bluetooth docs](https://docs.zephyrproject.org/latest/connectivity/bluetooth/index.html)
- [ESP-IDF BLE](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/bluetooth/index.html)
