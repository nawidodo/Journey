# 26-03 · BLE HID gamepad — HID-over-GATT, real hardware for your turbo idea

**Week:** W38–40 · **Track:** R · **Prev:** [`../02-first-peripheral-toolchain`](../02-first-peripheral-toolchain/README.md) · **Next:** [`../04-host-side-sniffing`](../04-host-side-sniffing/README.md)

## Objective
Build a **Bluetooth HID gamepad** — the physical device that makes Track G's virtual-controller/turbo work real. HID-over-GATT (HOG) profile: the same HID report model you parsed in 14-07, now as a peripheral.

## Tasks
- [ ] HOG profile anatomy: HID Service (0x1812) + Report Map/Report/Protocol Mode characteristics, report map for a gamepad (buttons bitmask, axes), boot vs report mode
- [ ] Report map by hand: buttons (A/B/X/Y, shoulders, start/select) as a bitmap + 2 analog axes — reuse the button-layout analysis from 14-07
- [ ] Firmware: input report generation on button presses (your board's GPIOs as buttons), notify on change, connection + pairing handled by the host
- [ ] Connect to each OS (the full matrix from step 02): Linux (BlueZ + evdev — gamepad appears as `/dev/input/jsX`), macOS (IOHIDManager sees it), Windows (HID stack + ViGEm-style visibility), Android/iOS (Web Bluetooth or Gamepad API test page)
- [ ] Verify with a real game or a joystick tester page on every OS; measure input latency (button → screen) roughly — note where the delay is (notify interval, host HID stack, polling)
- [ ] Turbo on hardware: extend 14-07's turbo state machine into the firmware — button hold → firmware synthesizes N Hz button presses in the report stream (no host software involved)
- [ ] Docs: wiring diagram, report map, per-OS connection quirks — `notes/`

## Resources
- USB HID spec (report format is shared with HOG), Bluetooth HID Profile 1.1, Nordic `hid_gamepad` sample (Zephyr), ESP-IDF HID examples
- 14-07 outputs (report parsing, turbo core)

## Exit Criteria
- [ ] BT HID gamepad plays a real game on at least 2 OSes, appears as normal controller — `labs/`
- [ ] Firmware turbo works (measured auto-fire rate over the air, sniffer-verified) — `labs/`
- [ ] Explain in ≤5 lines how HID-over-GATT differs from USB HID (transport + report map over ATT) — `notes/`

## Links
- [Bluetooth HID Profile spec](https://www.bluetooth.com/specifications/specs/human-interface-device-profile-1-1/)
- [Nordic HID sample](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/bluetooth/peripheral_hids_keyboard/README.html)
- [USB HID spec (report descriptor)](https://www.usb.org/hid)
- [Web Bluetooth gamepad demo](https://webbluetoothcg.github.io/web-bluetooth/)
