# 28-04 · Red-team Wi-Fi hosting — rogue AP + MITM + pivot

**Week:** W34–36 · **Track:** T · **Prev:** [`../03-enterprise-client-attacks`](../03-enterprise-client-attacks/README.md) · **Next:** [`../05-detection-mirror-wids`](../05-detection-mirror-wids/README.md)

## Objective
Operate the full rogue-AP stack like an operator: bring clients in, take their traffic, pivot into the wired side. Own lab only.

## Tasks
- [ ] Full rogue-AP stack: hostapd (SSID + crypto) + dnsmasq (DHCP/DNS) + iptables NAT; client-less vs client-joined operation
- [ ] MITM: bettercap HTTP/HTTPS interception; where SSLstrip-era tricks die (HSTS, cert pinning) and what still works (plaintext, captive portal, EAP)
- [ ] WPA2-Enterprise rogueAP: your own cert + domain capture for PEAP (pairs 28-03)
- [ ] Pivot: Wi-Fi foothold → wired network access (NAT bridge, ARP on the upstream LAN) — cross-ref Track N `05-network-adapter-pivot`; the radio twin of the LAN-Turtle
- [ ] Runbook: SSID choice, channel, interference, staying up under deauth; logging for the detection round-trip (28-05/06)
- [ ] Threat-model what defenders will see (the 28-05 syllabus, inverted) — `notes/`

## Resources
- hostapd/dnsmasq docs; bettercap docs; prior step outputs

## Exit Criteria
- [ ] Rogue AP with working MITM + one captured credential + pivot to the wired side — `labs/`
- [ ] Runbook with the detection signals you accepted — `notes/`

## Links
- [bettercap](https://www.bettercap.org/)
- [hostapd](https://w1.fi/hostapd/)
