# 24-114 · Own image stabilizer — feature tracking, the shake-to-still math (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../113-own-noise-reduction`](../113-own-noise-reduction/README.md) · **Next:** [`../115-own-ios-app-analyzer`](../115-own-ios-app-analyzer/README.md) · **Pairs:** 24-101, 24-107, 24-63

## Objective
Handheld video wobbles; every phone fixes it invisibly: build a stabilizer-lite — frame differences (24-76/101 readers give you frames), feature detection (corner-like interest points — the luminance-gradient math 24-63 reuses), motion estimation per frame (translation model — the least-squares fit, pairs 24-106 timing discipline), trajectory smoothing (the moving-average/EMA path — the "crop+shift" compensation), and render stabilized output through your 24-101 export. The lab is the payoff: shoot/take-your-own shaky clip (or synthesize shake on a stable 24-76 test clip), stabilize, and measure the residual motion curve (24-30).

## Tasks
- [ ] Frames: decode with your 24-54/98 readers (grayscale downscale for speed — 24-63 practice)
- [ ] Features: gradient-based interest points (Sobel-lite — 22-10 convolution reuse), feature matching across frames (SSD windows)
- [ ] Motion: translation/affine-lite model fit (least squares — 10-15 gradient thinking), outlier drop (RANSAC-lite)
- [ ] Smooth: trajectory low-pass (EMA/SAVIZKY-lite), crop+transform compensation (24-63 texture sampling), output
- [ ] Lab: stabilize your shaky clip (or synthesized shake on a 24-76 clip), residual-motion before/after curve — `labs/`
- [ ] Writeup: optical stabilization vs digital (hardware sensors + gyro fusion — 22-12/26-BT), where it fails (parallax) — `notes/`

## Resources
- Motion-stabilization papers (the manual — the classic sparse-optical-flow approach); OpenCV's VideoStab code (peer, the dance); your 24-101/24-63/24-76 code

## Exit Criteria
- [ ] Shaky clip stabilized with measured residual curve — `labs/` + `code/`
- [ ] Motion-model writeup — `notes/`

## Links
- [Video stabilization survey](https://arxiv.org/abs/2102.07278)
- [OpenCV video stabilization](https://docs.opencv.org/4.x/d7/d58/tutorial_js_video_stabilization.html)