# 10-15 · Own neural network — MNIST from scratch, no frameworks (stretch)

**Week:** W28+ stretch · **Track:** C · **Prev:** [`../14-cuda-ai-compute`](../14-cuda-ai-compute/README.md)

## Objective
ML is the era's compute workload — and it's attack surface (evasion, poisoning). Build a neural net from scratch: forward pass, backprop, SGD, MNIST classifier — no PyTorch/TF. Then the security angle: adversarial example (FGSM) against your own net, the gradient you computed yourself. Pairs 10-14 CUDA kernels and the GPU architecture you already understand.

## Tasks
- [ ] Core: matrix ops (reuse 10-02 SIMD where useful), forward pass (fc + ReLU/softmax), loss
- [ ] Backprop: gradients by hand-derived chain rule, SGD + mini-batches; train to ~97% on MNIST (the oracle)
- [ ] Speed: profile → vectorize/parallelize (OpenMP or SIMD; GPU port optional via 10-14 style kernels)
- [ ] Attack lab: FGSM adversarial example — a perturbed digit that misclassifies; your own gradients, your own attack; writeup on why ML systems need the adversarial mindset (pairs 08 browser, evasive-malware 12-10) — `notes/`
- [ ] Self-check: eval accuracy, confusion matrix, adversarial success rate before/after a tiny defense (adversarial training)

## Resources
- Nielsen's *Neural Networks and Deep Learning* (the manual); CS231n notes; your 10-14 notes

## Exit Criteria
- [ ] MNIST ~97% from scratch, no ML framework — `labs/`
- [ ] FGSM attack + defense writeup — `labs/` + `notes/`

## Links
- [Nielsen's book](http://neuralnetworksanddeeplearning.com/)
- [CS231n](https://cs231n.github.io/)
