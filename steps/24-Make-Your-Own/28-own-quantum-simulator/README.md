# 24-28 · Own quantum simulator — qubits, gates, a real algorithm (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../27-own-png-decoder`](../27-own-png-decoder/README.md) · **Next:** [`../29-own-kademlia-dht`](../29-own-kademlia-dht/README.md)

## Objective
The era's wild frontier. Build a small quantum simulator: qubits as complex amplitude vectors, gates as matrices (Hadamard, CNOT, phase), measurement (Born rule), then run real algorithms on 2–8 qubits: Bell state, entanglement, phase kickback, Deutsch-Jozsa, Grover on 2 qubits. The security tie: quantum algorithms are *why* post-quantum crypto (your 20-09 ML-KEM) exists — Shor vs RSA, Grover vs key search.

## Tasks
- [ ] Core: amplitude-vector state, gate application (matrix multiply), measurement + collapse, tensor product for multi-qubit (your 10-02 SIMD skills help)
- [ ] Algorithms: Bell/entanglement, phase kickback, Deutsch-Jozsa (the "exponential speedup" first taste), Grover's search on 2–3 qubits
- [ ] Verification: run the same circuits on a real free backend (IBM Qiskit Aer) — your simulator matches (the oracle)
- [ ] Security writeup: Shor's algorithm step-by-step vs RSA (why 20-09 matters), Grover vs symmetric key size, the harvest-now-decrypt-later timeline — `notes/`

## Resources
- Nielsen & Chuang ch.1–4 (or Qiskit textbook); your 20-02 number-theory + 20-09 notes

## Exit Criteria
- [ ] Simulator runs Deutsch-Jozsa + Grover(2q), matches Qiskit — `labs/`
- [ ] Shor/Grover security writeup — `notes/`

## Links
- [Qiskit textbook](https://learning.quantum.ibm.com/)
- [Nielsen & Chuang](https://www.cambridge.org/core/books/quantum-computation-and-quantum-information/01E10196D0C682C6AEFFEA52D53BE9AE)
