# 22-10 · Embedded signals — audio capture + DSP (stretch)

**Week:** W36+ stretch · **Track:** N · **Prev:** [`../09-embedded-firmware-exploitation`](../09-embedded-firmware-exploitation/README.md)

## Objective
The analog + DSP layer the plan never touched. Build the capture chain a spy-gadget needs: microphone → analog front-end → ADC → FFT/filter DSP → logged output, on an ESP32. Optics as the stretch experiment.

## Tasks
- [ ] Analog front-end: electret mic + preamp (op-amp gain, biasing, decoupling) — why the noise floor decides everything; measure it on a scope before digitizing
- [ ] ESP32 I²S: MEMS mic (INMP441-class) or ADC channel, 16-bit sampling, DMA buffers; verify with a loopback playback test
- [ ] DSP: FFT (ESP-DSP lib or hand-rolled radix-2), band-pass filter, noise-floor estimation, threshold trigger; log detections to SD/over Wi-Fi (pairs 22-03 C2)
- [ ] Stretch — optical pickup: laser + photodiode + transimpedance amp (picoamp sensitivity) → reflectance vibration pickup off a glass surface → demodulate to audio (the laser-mic experiment). Own lab only; a scope is mandatory here
- [ ] Writeup: block diagram + noise budget per stage — where gain hurts and why — `notes/`

## Resources
- ESP32 I²S/ADC docs; INMP441 datasheet; ESP-DSP lib; transimpedance-amp design notes; Hackaday laser-mic builds

## Exit Criteria
- [ ] Working chain: mic → digitized → filtered → triggered → logged — `labs/`
- [ ] (Stretch) optical pickup demodulated to intelligible audio — `labs/`

## Links
- [ESP32 I2S](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/i2s.html)
- [ESP-DSP](https://github.com/espressif/esp-dsp)
- [INMP441](https://invensense.tdk.com/wp-content/uploads/2015/02/INMP441.pdf)
