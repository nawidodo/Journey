# 28-02 · WPA2/WPA3 attack classes — PMKID, KRACK, Dragonblood

**Week:** W30–32 · **Track:** T · **Prev:** [`../01-wifi-stack-monitor-mode`](../01-wifi-stack-monitor-mode/README.md) · **Next:** [`../03-enterprise-client-attacks`](../03-enterprise-client-attacks/README.md)

## Objective
Attack the authentication itself: offline PSK cracking, PMKID, deauth, and the protocol-level breaks (KRACK, Dragonblood). Your own APs only.

## Tasks
- [ ] WPA2-PSK offline cracking: capture 4-way handshake → dictionary/rule-based (hashcat, GPU); PMKID capture (RSN IE, clientless — no handshake needed); when each works
- [ ] Deauth attacks: why a spoofed deauth works (no integrity), how it forces handshake capture; deauth detection angle
- [ ] KRACK (CVE-2017-13077): the theory — WPA2 key reinstallation, nonce reuse in CCMP — and a lab demo against a vulnerable AP if you can patch one in
- [ ] WPA3 SAE: Dragonblood (CVE-2019-9494+) — SAE downgrade, transition-mode downgrade to WPA2, side-channel on the password element; what actually got fixed
- [ ] Write up each class: root cause → exploit → fix — `notes/`

## Resources
- Vanhoef: KRACK and Dragonblood papers; hashcat wifi mode docs; hostapd (to build vulnerable test APs)

## Exit Criteria
- [ ] PMKID **and** handshake crack on your own AP — `labs/`
- [ ] KRACK + Dragonblood root-cause writeup (what the protocol change is, why it broke) — `notes/`

## Links
- [KRACK paper](https://papers.mathyvanhoef.com/ccs2017.pdf)
- [Dragonblood paper](https://papers.mathyvanhoef.com/asiaccs2020.pdf)
- [hashcat](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2)
