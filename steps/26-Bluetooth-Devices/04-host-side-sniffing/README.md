# 26-04 · Host side + sniffing — what each OS does with your device, and watching it happen

**Week:** W39–41 · **Track:** R · **Prev:** [`../03-ble-hid-gamepad`](../03-ble-hid-gamepad/README.md) · **Next:** [`../05-bt-classic-audio-spp`](../05-bt-classic-audio-spp/README.md)

## Objective
The OS side of the pairing — every device you build gets consumed by one of these stacks. Read real packets at two levels: HCI (host↔controller) and LL/air (sniffer), and learn how each OS discovers, pairs, and serves HID.

## Tasks
- [ ] Linux/BlueZ deep: `bluetoothctl`/`btmon` — capture HCI traffic of your own gamepad connecting; find the HCI events for discovery, pairing (legacy vs SC), and GATT over L2CAP; where evdev entry happens (hidp/hidraw over BT)
- [ ] macOS CoreBluetooth: CBCentralManager/CBPeripheral flow, why Apple hides the HID stack (IOHIDManager gets it transparently), peripheral behavior with `CBPeripheralManager`
- [ ] Windows: WinRT Bluetooth LE (advertisement watcher, GATT client) vs classic Bluetooth stack; how a HOG device lands in the HID input stack; the driver question (why BT HID needs no driver but USB HID-class does)
- [ ] Android: BLE scanning/connection APIs + the `android.bluetooth` classic layer
- [ ] Sniffer lab (nRF52840 sniffer or Ubertooth + Wireshark): capture your own advertising, pairing handshake, and a notify; decode LL → L2CAP → ATT/GATT/SMP; identify the SMP pairing method from the packets
- [ ] Latency measurement: notify interval + connection interval math — verify a 15 Hz turbo stream arrives at the host at the expected rate (ties steps 03 + Track G timing work)
- [ ] `notes/`: per-OS consumption table — discovery API, pairing UX, where HID lands, what a virtual device would need to impersonate (feeds the security step)

## Resources
- btmon/Wireshark docs, Nordic sniffer user guide, BlueZ docs
- MS WinRT BT docs, Apple CoreBluetooth docs, AOSP BT docs

## Exit Criteria
- [ ] Full connection (discovery → pair → connect → notify) captured and annotated in Wireshark — `labs/`
- [ ] Explain in ≤5 lines what a sniffer sees that the host never does (and vice versa) — `notes/`
- [ ] Measured notify-stream rate matches the configured connection interval — `labs/`

## Links
- [btmon (BlueZ)](https://manpages.debian.org/testing/bluez/btmon.1.en.html)
- [nRF sniffer](https://www.nordicsemi.com/Products/Development-tools/nrf-sniffer-for-bluetooth-le)
- [CoreBluetooth](https://developer.apple.com/documentation/corebluetooth)
- [WinRT Bluetooth LE (MS)](https://learn.microsoft.com/en-us/windows/uwp/devices-sensors/bluetooth-low-energy-overview)
- [Wireshark BTLE](https://wiki.wireshark.org/Bluetooth)
