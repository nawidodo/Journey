# 24-60 · Own spam filter — naive Bayes over your mail server (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../59-own-qr-codec`](../59-own-qr-codec/README.md) · **Next:** [`../61-own-p2p-sync`](../61-own-p2p-sync/README.md) · **Pairs:** 24-34, 10-15, 10-16

## Objective
Your 24-34 mail server delivers mail; now decide what's junk. Build a spam filter: tokenize (the MIME/header minefield — pairs 24-34), feature extraction, naive Bayes classification (the math — pairs 10-15 probabilities), and the evaluation discipline (precision/recall curves — pairs 10-16 accuracy tables). The adversarial loop is the security lesson: spam is an adaptive attacker (obfuscated tokens, base64, image spam) — every evasion is a mini adversarial-ML case (pairs 10-15 FGSM).

## Tasks
- [ ] Corpus: your own 24-34 server logs + hand-labeled ham/spam sets (local only, no real data)
- [ ] Tokenizer: word + character n-grams, base64/HTML decoding (the evasion surface), header features
- [ ] Classifier: naive Bayes with smoothing; log-space scoring; the threshold decision (false-positive cost — the mail-user contract)
- [ ] Adversarial lab: obfuscate spam (leet-speak, split tokens, HTML entities) → filter evasion; counter (normalize tokens — pairs 24-25) — the loop — `labs/`
- [ ] Self-check: precision/recall table vs threshold; the FPR you'd accept on your own inbox

## Resources
- The classic "A Plan for Spam" (the manual); SpamAssassin docs (peer); your 24-34/10-15 notes

## Exit Criteria
- [ ] Filter with measured precision/recall on own corpus — `labs/`
- [ ] Evasion-counter loop writeup — `labs/` + `notes/`

## Links
- [A Plan for Spam](https://www.paulgraham.com/spam.html)
- [SpamAssassin](https://spamassassin.apache.org/)
