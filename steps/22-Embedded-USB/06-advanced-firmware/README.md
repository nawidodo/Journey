# 22-06 · Advanced Firmware: Multi-Interface, Exfil Queue, OTA, Self-Destruct

**Week:** W35 · **Track:** N · **Prev:** [`../05-network-adapter-pivot`](../05-network-adapter-pivot/README.md) · **Next:** [`../07-fault-injection-glitching`](../07-fault-injection-glitching/README.md)

## Objective
Turn the 22-01…05 feature pile into a *product-grade* attack device — the firmware hardening plugandpwn-class tools ship: composite USB, reliable exfil, remote updates, and operational failure modes. This is where a hack becomes a tool.

## Tasks
- [ ] Composite USB device: HID keyboard + CDC (serial console) + vendor class in one descriptor set (TinyUSB composite); verify each interface enumerates
- [ ] Exfil queue: results buffered to flash (LittleFS) with retry + ack, so a plug-in that ends too early still reports later
- [ ] Payload/config management: multiple payloads, trigger selection (plug-in, WiFi command, time), encrypted config blob (e.g. AES keyed at first boot)
- [ ] OTA firmware updates over the AP; signed update images (verify before flashing — brick-prevention)
- [ ] Failure modes: panic/self-destruct (wipe config/payloads on unauthorized access attempt — remote lock + local trigger), brownout recovery, watchdog
- [ ] Ops docs: deployment checklist, build reproducibility (idf.py + sdkconfig tracked in `code/`)

## Resources
- TinyUSB composite docs; ESP-IDF OTA (esp_ota) + esp_secure_boot basics; LittleFS
- plugandpwn docs — feature list to match (self-destruct, expandable interfaces, payload management)
- Your 22-02 engine + 22-03 C2, now under one firmware image

## Exit Criteria
- [ ] One firmware image runs HID + pivot + exfil + OTA; interface matrix verified on a real OS — `labs/`
- [ ] Self-destruct + OTA + exfil-queue all demonstrated and documented — `notes/`

## Links
- [ESP-IDF OTA docs](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/system/ota.html)
- [ESP32 TinyUSB composite example](https://github.com/espressif/esp-idf/tree/master/examples/peripherals/usb/device/tinyusb)
- [plugandpwn — feature reference](https://plugandpwn.com/)
