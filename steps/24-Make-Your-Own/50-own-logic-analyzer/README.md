# 24-50 · Own logic analyzer — UART/SPI/I2C decode from raw samples (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../49-own-font-renderer`](../49-own-font-renderer/README.md)

## Objective
The hardware-hacking companion: decode UART/SPI/I2C from raw sample traces (recorded with a cheap logic analyzer — Saleae-class or your own). Build the decoder: sample-rate math, edge detection, protocol state machines (UART baud/framing, SPI CS/clock/data, I2C start/stop/ACK). The payoff pairs 22-09 firmware RE and 22-10 signals: sniff your ESP32's debug UART, decode a sensor's I2C — the interface you can actually see.

## Tasks
- [ ] Core: parse raw sample dumps (VCD/CSV), edge detection, glitch filtering; the timing math (baud vs sample rate)
- [ ] UART: start bit, data bits, parity, stop; decode real captures (your ESP32 debug console at 115200 — record with an analyzer)
- [ ] SPI/I2C: CS/SCLK framing, MOSI/MISO, 8-bit transactions; I2C start/stop, ACK/NACK, address + register reads
- [ ] Protocol stack: on top of SPI — flash read commands (SFDP/JEDEC — pairs 22-09 firmware extraction); on I2C — a sensor register map
- [ ] Writeup: logic-analyzer forensics (firmware extraction pipeline: UART → shell, SPI → flash dump) — `notes/`

## Resources
- Saleae/pulseview decode docs (peer); the "logic analyzer for firmware extraction" writeups; your 22-09/22-10 notes

## Exit Criteria
- [ ] Decoder handles UART + SPI + I2C traces — `labs/`
- [ ] ESP32 UART + flash-read decode, firmware-extraction writeup — `labs/` + `notes/`

## Links
- [sigrok/pulseview](https://sigrok.org/)
- [Logic analyzer protocols](https://support.saleae.com/hc/en-us/articles/360052719493-Protocol-Analyzers)
