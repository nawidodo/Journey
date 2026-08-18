# 22-11 · DIY radio gadgets — sub-GHz replay, RFID cloner, ultrasonic link (stretch)

**Week:** W36+ stretch · **Track:** N · **Prev:** [`../10-embedded-signals-dsp`](../10-embedded-signals-dsp/README.md)

## Objective
Build three radio gadgets from scratch on boards you already own. New radio domains the plan never touched — sub-GHz OOK (keyfobs, gates), 13.56 MHz (badges, cards), and inaudible acoustic (air-gap exfil). Own hardware, own lab only.

## Tasks
- [ ] **Sub-GHz RF replay (CC1101 + ESP32/Arduino):** 433/315 MHz OOK — demodulate a captured keyfob/gate signal by hand first (pairs 22-10 DSP), then build the capture → replay loop; brute-force fixed-code, then rolling-code: capture, analyze, demonstrate why fixed-code dies and where rolling-code fails (replay window, sync)
- [ ] **RFID/NFC cloner (ESP32 + PN532):** read Mifare Classic, dump sectors, clone to NTAG215; PN532 tag-emulation mode; reverse one crypto-1 round (pairs Track L 20 crypto — the broken stream cipher that made Mifare Classic dead)
- [ ] **Ultrasonic covert channel (two ESP32s):** 18–25 kHz inaudible link, FFT decode (pairs 22-10), file exfil across a desk — the air-gap argument made physical
- [ ] Writeup per gadget: block diagram, RF/radio path, what defenders detect (the WIDS/DFIR mirror — keyfob replay and NFC cloning are both detectable at the reader) — `notes/`

## Resources
- CC1101 datasheet + radioLib/rc-switch libs; PN532 docs; Mifare Classic crypto-1 writeups; your 22-10 signals notes

## Exit Criteria
- [ ] ≥2 of 3 gadgets working end-to-end — `labs/`
- [ ] Detection note per gadget: what a defender sees at the target reader — `notes/`

## Links
- [radioLib](https://github.com/jgromes/RadioLib)
- [PN532](https://www.nxp.com/docs/en/user-guide/141520.pdf)
- [Mifare Classic security (crypto-1)](https://eprint.iacr.org/2015/321)
