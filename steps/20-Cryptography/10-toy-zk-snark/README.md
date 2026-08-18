# 20-10 · Toy zk-SNARK — prover + verifier from first principles (stretch)

**Week:** W20 stretch · **Track:** L · **Prev:** [`../09-ml-kem-kyber`](../09-ml-kem-kyber/README.md)

## Objective
Zero-knowledge is the era's hot crypto. Build the smallest honest SNARK: statement → arithmetic constraint system → polynomial commitment → proof, and a verifier that accepts only true proofs. One constrained problem (e.g., "I know x such that x³+2x=42" or a Merkle inclusion proof) — soundness, not performance.

## Tasks
- [ ] Constraint system: flatten a computation to R1CS (the circuit model behind every modern zk stack); the "prover sends, verifier checks" shape
- [ ] Polynomial backend: QAP construction (R1CS → polynomials), KZG-style polynomial commitment (pairs 20-02 pairing/group theory); where the Fiat-Shamir transform turns interactive → non-interactive
- [ ] Proof + verify: prover computes the three polynomials' commitments; verifier checks the pairing equation; reject a tampered proof (the honesty check)
- [ ] Optional: a second statement (Merkle inclusion — the chain-era use case) to prove generality
- [ ] Writeup: where real systems diverge (plonk vs groth16, lookup tables) — `notes/`

## Resources
- "Why and How zk-SNARK Works" (the classic explainer); Vitalik's R1CS/QAP posts; your 20-02/03 notes

## Exit Criteria
- [ ] Toy SNARK: prove + verify one statement; tampered proof rejected — `labs/`
- [ ] Second statement verified (optional) — `labs/`

## Links
- [Why and How zk-SNARK Works](https://arxiv.org/abs/1906.07221)
- [Vitalik — QAPs](https://vitalik.ca/general/2017/01/14/exploring_ecfp_parsing.html)
