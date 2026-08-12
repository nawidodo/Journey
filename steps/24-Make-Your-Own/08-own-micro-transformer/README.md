# 24-08 · Own "What's that LLM doing" (micro transformer)

**Week:** W46+ (parallel, optional, stretch) · **Track:** P · **Prev:** [`../07-own-assembler`](../07-own-assembler/README.md) · **Next:** —

## Objective
Build a tiny GPT-style transformer from scratch (tokens → embeddings → attention → MLP → logits) and backprop-demo it. You don't need to be an ML engineer for syssec, but every vendor in this space (ALCHERA, etc.) ships "AI" claims — you should be literate enough to critique a model, and deepfake/GenAI detection (Phase 8/genAI-adjacent, your ALCHERA deck) is exactly this machinery. This is the last piece of "make your own" worth the time; skip if you never touch AI.

## Tasks
- [ ] Tokenizer + embedding; implement multi-head self-attention + position encodings — `code/`
- [ ] FFN/MLP block, layer norm, residual; a forward pass that predicts — `code/`
- [ ] Backprop + train on a tiny corpus (char-level) in NumPy/PyTorch — `code/`
- [ ] Debrief: how a deepfake/GenAI "detector" model is structurally the same thing flipped — `notes/`

## Resources
- Andrej Karpathy: *nanoGPT* / *makemore* (the canonical from-scratch path)
- "Attention Is All You Need"; 3Blue1Brown attention series

## Exit Criteria
- [ ] Tiny model trained end-to-end predicting a small corpus — `code/`
- [ ] Debrief note on the detector-angle — `notes/`

## Links
- [Karpathy — makemore](https://github.com/karpathy/makemore)
- [Karpathy — nanoGPT](https://github.com/karpathy/nanoGPT)
- [build-your-own-x: own neural network](https://github.com/codecrafters-io/build-your-own-x?tab=readme-ov-file#build-your-own-neural-network)