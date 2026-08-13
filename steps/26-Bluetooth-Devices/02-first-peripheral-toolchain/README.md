# 26-02 · Toolchain + first peripheral — a GATT server you can poke from every OS

**Week:** W37–39 · **Track:** R · **Prev:** [`../01-bt-stack-theory`](../01-bt-stack-theory/README.md) · **Next:** [`../03-ble-hid-gamepad`](../03-ble-hid-gamepad/README.md)

## Objective
Get a real board advertising and serving GATT. Primary hardware: **nRF52840 DK** (Nordic — the best BLE docs/SDK in the industry, Zephyr-based nRF Connect SDK); cheap alternative: ESP32-C3 (ESP-IDF/Arduino). First device = your own custom service with read/write/notify characteristics.

## Tasks
- [ ] Toolchain up: nRF Connect SDK (VS Code extension or CLI) or ESP-IDF; flash a board and blink — hardware works end to end
- [ ] Advertising: name, service UUID, advertising interval; verify with your phone (nRF Connect / LightBlue app)
- [ ] Custom GATT service: one characteristic with read, one with write, one with notify + CCCD; button on board → notify; phone writes → LED
- [ ] Host-side from every OS (the "all OSes" half): `bluetoothctl`/`gatttool` on Linux (BlueZ), CoreBluetooth sample on macOS, WinRT `BluetoothLEAdvertisementWatcher`/GATT client on Windows, Android BLE scanner — connect and exercise read/write/notify from each; note each OS's API shape in `notes/`
- [ ] Wire protocol check: run Wireshark with a BLE sniffer (nRF Connect desktop, or cheap nRF52840 sniffer dongle) — capture your own advertising + a notification, decode the ATT/GATT packets, verify they match the theory
- [ ] Connection parameters: try connection-interval negotiation; observe it in the sniffer and via `bluetoothctl info`

## Resources
- Nordic academy (Zephyr/Bluetooth), nRF Connect SDK samples (`peripheral_*`, `bluetooth` samples)
- ESP-IDF BLE examples if going ESP32 route

## Exit Criteria
- [ ] Custom GATT server advertising + read/write/notify, poked from all 4 OSes — `labs/`
- [ ] Your own notification captured and decoded in Wireshark — `labs/`
- [ ] One-sheet `notes/`: each OS's BLE client API (name, connect flow, characteristic read/write calls) — the reference for every later step

## Links
- [nRF Connect SDK](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/index.html)
- [nRF Connect for Desktop (incl. sniffer)](https://www.nordicsemi.com/Products/Development-tools/nrf-connect-for-desktop)
- [ESP32-C3](https://www.espressif.com/en/products/socs/esp32-c3)
- [BlueZ bluetoothctl](https://manpages.debian.org/testing/bluez/bluetoothctl.1.en.html)
- [Wireshark BTLE capture guide](https://wiki.wireshark.org/Bluetooth)
