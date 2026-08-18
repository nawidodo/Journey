# 24-65 · Own audio modem — text over sound, the analog-to-digital bridge (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../64-own-hd-wallet`](../64-own-hd-wallet/README.md) · **Next:** [`../66-own-load-balancer`](../66-own-load-balancer/README.md) · **Pairs:** 24-40, 22-10, 24-25

## Objective
Before Wi-Fi, computers talked over phone lines with modems; your 24-40 synth can make tones, your 22-10 DSP can analyze them. Build a modem: encode text → FSK/DTMF tones (your synth), transmit speaker-to-microphone (loopback + real audio), demodulate (Goertzel filter — the DSP — pairs 22-10), error control (pairs 24-25 CRC/RS ideas, 24-59). The payoff: you've built the physical-layer-to-bits bridge that made the internet, and every IoT/legacy protocol still uses it (pairs 22-11 radio).

## Tasks
- [ ] Modulation: FSK (two tones, mark/space) + DTMF decode; the symbol/baud math (pairs 24-40 sampling, 22-10 windows)
- [ ] Demod: Goertzel algorithm (the DSP classic), threshold + timing recovery; SNR/error curve vs volume
- [ ] Framing: header/CRC (pairs 24-25), packetization, byte stuffing — detect + reject corrupt frames
- [ ] Lab: text "Journey" through speaker→mic (loopback + real distance), error rate table vs volume/noise; the distance-capacity curve — `labs/`
- [ ] Writeup: why modems died (and where they remain: DTMF in telephony, acoustic couplers in air-gaps, SSTV) — `notes/`

## Resources
- The DTMF spec + Goertzel writeups (the manual); Audacity as the spectral oracle (pairs 24-40); your 24-40/22-10 code

## Exit Criteria
- [ ] Text encodes→transmits→decodes through real audio — `labs/` + `code/`
- [ ] Error-rate curve + legacy-whereabouts writeup — `labs/` + `notes/`

## Links
- [Goertzel algorithm](https://en.wikipedia.org/wiki/Goertzel_algorithm)
- [DTMF](https://en.wikipedia.org/wiki/DTMF)