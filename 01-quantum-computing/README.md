# Quantum Computing

Course: https://pennylane.ai/codebook/learning-paths


## Foundations of Quantum Computing

- [IQC: Introduction to Quantum Computing](./01-iqc-introduction-to-quantum-computing/)
- [SQ: Single-Qubit Gates](./02-sq-single-qubit-gates/)
- [MQ: Circuits with Many Qubits](./03-mq-circuits-with-many-qubits/)

## Foundations of Quantum Algorithms

- [BA: Basic Quantum Algorithms](./04-ba-basic-quantum-algorithms/)
- [GA: Grover's Algorithm](./05-ga-grovers-algorithm/)
- [QFT: Quantum Fourier Transform](./06-qft-quantum-fourier-transform/)
- [QPE: Quantum Phase Estimation](./07-qpe-quantum-phase-estimation/)
- [SH: Shor's Algorithm](./08-sh-shors-algorithm/)

## Quantum Fault Tolerance

- [NT: Noisy Quantum Theory](./09-nt-noisy-quantum-theory/)
- [DM: Distance Measures](./10-dm-distance-measures/)
- [EC: Quantum Error Correction](./11-ec-quantum-error-correction/)

## Hamiltonian Simulation and its Applications

- [TE: Hamiltonian Time Evolution](./12-te-hamiltonian-time-evolution/)
- [HS: Hamiltonian Simulation](./13-hs-hamiltonian-simulation/)

## PennyLane Fundamentals (toolkit)

- [PF: PennyLane Fundamentals](./14-pf-pennylane-fundamentals/)

<!-- notion-import -->
## Notes from Notion

_Copied from the [Quantum Computing](https://app.notion.com/p/2c244de2a65680bab773c5187402da7a) overview page. The chapter summaries on that page are copied into each unit's README._

**Progress checklist**

- [ ] Chapter
- [ ] Exercises
- [ ] Cheat sheet

### Single-qubit gate reference

- Z = HXH, Y = S†XS (S† = S adjoint)
- RX: `qml.RX`, RY: `qml.RY`, RZ: `qml.RZ`
- X: `qml.X` or `qml.PauliX`; H: `qml.Hadamard` or `qml.H`; Y: `qml.Y` or `qml.PauliY`; Z: `qml.Z` or `qml.PauliZ`
- S: `qml.S`; T: `qml.T`
- *[Gate diagrams are images in Notion](https://app.notion.com/p/2c244de2a65680bab773c5187402da7a)*

### Multi-qubit gate reference

- CNOT: `qml.CNOT`; CZ: `qml.CZ`; CRZ: `qml.CRZ`
- Toffoli: `qml.Toffoli`; CCZ: `qml.CCZ`
- *[Gate diagrams are images in Notion](https://app.notion.com/p/2c244de2a65680bab773c5187402da7a)*

