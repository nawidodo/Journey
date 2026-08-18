# 28-03 · Enterprise + client attacks — EAP relay, captive portal, evil twin

**Week:** W32–34 · **Track:** T · **Prev:** [`../02-wpa2-wpa3-attacks`](../02-wpa2-wpa3-attacks/README.md) · **Next:** [`../04-redteam-wifi-hosting`](../04-redteam-wifi-hosting/README.md)

## Objective
Attack where networks are weakest: WPA2-Enterprise (802.1X) trust, and the human at the captive portal.

## Tasks
- [ ] WPA2-Enterprise: 802.1X/EAP model, PEAP/MSCHAPv2, certificate validation — and why most deployments validate nothing (the relay/credential-capture window)
- [ ] EAP relay + credential theft: hostapd-wpe / EAPhammer against your own RADIUS-lab; MSCHAPv2 → NetNTLMv2 → offline crack; cert-validation bypass in practice
- [ ] Captive portal: wifiphisher / own portal — phishing a real login on a spoofed SSID, in your lab only
- [ ] Client-based attacks: probe-triggered targeted evil twin (client connects to the SSID it probes — no deauth needed); PMKID clientless recap
- [ ] WPA3 transition-mode: forcing a WPA3 client down to WPA2 (Dragonblood application)
- [ ] Lab network only; document target config — `notes/`

## Resources
- EAPhammer docs; hostapd-wpe; wifiphisher; airbase-ng

## Exit Criteria
- [ ] EAP relay + one captured-and-cracked MSCHAPv2 credential — `labs/`
- [ ] Captive portal with working credential capture — `labs/`

## Links
- [EAPhammer](https://github.com/s0lst1c3/eaphammer)
- [wifiphisher](https://github.com/wifiphisher/wifiphisher)
