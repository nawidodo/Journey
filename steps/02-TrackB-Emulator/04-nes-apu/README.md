# 02-04 · NES Core — APU

**Week:** W10–14 · **Track:** B · **Prev:** [`../03-nes-ppu`](../03-nes-ppu/README.md) · **Next:** [`../05-nes-cart-mappers-rom-loader`](../05-nes-cart-mappers-rom-loader/README.md)

## Objective
Audio Processing Unit: pulse, triangle, noise, DMC channels.

## Tasks
- [ ] Pulse channels (duty cycles, sweep, envelope, length counter)
- [ ] Triangle (linear counter, low pass)
- [ ] Noise (LFSR, mode)
- [ ] DMC samples (delta modulation, DMA-style reads)
- [ ] Frame counter + APU registers (`$4000`–`$4017`); mix to output buffer

## Resources
- NESDev wiki — APU pages
- OneLoneCoder NES series, part 5 (APU)

## Exit Criteria
- [ ] Audio output (test ROMs with tone/SFX) — `labs/`
