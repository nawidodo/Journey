# 22-03 · ESP32 Port: WiFi C2, Web UI, Exfil

**Week:** W31–32 · **Track:** N · **Prev:** [`../02-duckyscript-engine`](../02-duckyscript-engine/README.md) · **Next:** [`../04-stealth-evasion-hardware`](../04-stealth-evasion-hardware/README.md)

## Objective
Move the injection engine onto a WiFi-capable board (ESP32-S3/C3) — now the device is *reachable*: load payloads over the air, get results back, no physical access needed. This is the step that turns a ducky into an O.MG cable.

## Tasks
- [ ] Port 22-01/22-02 to ESP32 (ESP-IDF or Arduino-ESP32); USB HID + WiFi coexisting
- [ ] SoftAP mode: the cable appears as a WiFi network; connect from a phone/laptop
- [ ] HTTP server + web UI: list/edit/trigger payloads, view exfil output (HTML/JS on LittleFS)
- [ ] Payload store: flash filesystem (LittleFS/SPIFFS) with multiple payloads, auto-run on plug-in
- [ ] Exfil channel: victim-side results (keylog, files, screenshots if possible) uploaded to the AP's server
- [ ] Ops-realistic: HTTPS/credentialed API on the AP; prevent bystander WiFi attackers from driving your cable (the "Evil Maid grabs the cable" problem)

## Resources
- Espressif ESP-IDF docs (usb_serial_jtag, esp_http_server, esp_wifi softAP, nvs/LittleFS)
- O.MG product page + docs — your feature target; AP + keylog + payload select
- From Track E (C2) and 22-02: payload engine; from Track F: the host side you're now attacking

## Exit Criteria
- [ ] Payload uploaded over WiFi and executed on a victim machine; output returned to the UI — `labs/`
- [ ] Multi-payload store + trigger-by-plug-in; network capture of the C2 session annotated — `notes/`

## Links
- [ESP-IDF docs](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/index.html)
- [LittleFS (esp_littlefs)](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/storage/spiffs.html)
- [O.MG Cable (Hak5) — feature reference](https://shop.hak5.org/products/omg-cable)
