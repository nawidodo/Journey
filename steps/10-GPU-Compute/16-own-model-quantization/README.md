# 10-16 · Own model quantization — int8 inference, the deployment gap (stretch)

**Week:** W28+ stretch · **Track:** C · **Prev:** [`../15-own-neural-network`](../15-own-neural-network/README.md)

## Objective
Your 10-15 NN trained in float; every deployed model runs quantized (int8 on phones, NPUs, edge). Build the quantization pipeline: post-training quantization (scale/zero-point, per-tensor vs per-channel), int8 GEMM (reuse 10-02 SIMD), accuracy-vs-size measurement. The security tie: quantization is where ML attacks live too (adversarial transfer, pairs 10-15 FGSM) — plus you'll read every NPU datasheet with real understanding.

## Tasks
- [ ] Math: symmetric vs asymmetric quantization, scale/zero-point calibration (min/max, percentile); the error model
- [ ] GEMM: float→int8 matrix multiply (SIMD, reuse 10-02), requantization (the fixed-point discipline — pairs 24-25 compression math, 24-40 DSP)
- [ ] Pipeline: quantize your 10-15 MNIST model → int8 weights + activations; measure accuracy (the table) and speedup (24-30 profiler)
- [ ] Variants: per-channel weights, dynamic vs static activation quant; GPTQ-lite (2-bit) stretch
- [ ] Writeup: where quantization breaks (outliers — the LLM problem), adversarial-transfer note — `notes/`

## Resources
- the "Quantization and Training of NN" papers (the manuals); llama.cpp's quant code (peer); your 10-15/10-02/24-30 notes

## Exit Criteria
- [ ] int8 MNIST within ~1% of float; speedup measured — `labs/`
- [ ] Quantization-error + outliers writeup — `labs/` + `notes/`

## Links
- [Quantization papers (Google)](https://arxiv.org/abs/1806.08342)
- [llama.cpp](https://github.com/ggerganov/llama.cpp)
