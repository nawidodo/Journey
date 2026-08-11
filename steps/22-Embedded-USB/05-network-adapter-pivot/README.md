# 22-05 · Network Adapter Mode: WiFi Client→AP Bridge (LAN-Turtle class)

**Week:** W34 · **Track:** N · **Prev:** [`../04-stealth-evasion-hardware`](../04-stealth-evasion-hardware/README.md) · **Next:** [`../06-advanced-firmware`](../06-advanced-firmware/README.md)

## Objective
Add the plugandpwn/LAN-Turtle capability your 22-03 softAP lacks: the device joins the *victim's* WiFi as a client and simultaneously exposes an attacker AP, NAT-bridging traffic so you get on the target network from outside. This is the "network pivot" half of an attack-USB device.

## Tasks
- [ ] ESP32 WiFi station mode: join a WPA2/WPA3 network; persist credentials in NVS
- [ ] Concurrent softAP + station (APSTA mode); bridge the two netifs
- [ ] IP layer: NAT + forwarding between interfaces (lwIP NAT or ESP-NAT router example); attacker devices get DHCP from the AP and reach the victim LAN
- [ ] Pivot validation: from an attacker laptop on the AP, hit victim-network hosts (ping/HTTP/smb) with the device as router; capture the path in `notes/`
- [ ] Exfil over victim network: device (as client) pulls victim LAN resources / uploads stolen data to attacker infrastructure over the *joined* network — not just back over the AP
- [ ] Stealth notes: coexistence with HID mode (inject + pivot in one plug-in), LED/indicator hygiene, physical scoping for authorized testing only

## Resources
- ESP-IDF: esp_wifi (APSTA), esp_netif, lwIP NAT, DHCP server; the `softap_sta` + NAT examples
- Hak5 LAN Turtle docs (the network-attack-device benchmark); your Track E C2 patterns
- Scoping: plugandpwn's own disclaimers — network pivot is high-blast-radius, lab/authorized targets only

## Exit Criteria
- [ ] Attacker laptop on the device's AP reaches the victim LAN through the device — `labs/` (traffic capture + ping/HTTP proof)
- [ ] HID injection and pivot coexist in one session; exfil over victim network demonstrated — `notes/`

## Links
- [ESP32 softAP+station example (esp-idf)](https://github.com/espressif/esp-idf/tree/master/examples/wifi/softap_sta)
- [esp_netif docs (NAT/DHCP)](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_netif.html)
- [LAN Turtle (Hak5) — feature reference](https://shop.hak5.org/products/lan-turtle)
