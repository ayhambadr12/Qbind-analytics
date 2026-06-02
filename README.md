# Q-Bind Analytics

> Quantum Computing as a Service (QCaaS) for pharmaceutical drug discovery.

## What is Q-Bind Analytics?

Q-Bind Analytics is a deep-tech startup based in Alexandria, Egypt, building a hybrid quantum-classical pipeline for computational drug discovery. We use the Variational Quantum Eigensolver (VQE) algorithm to calculate exact molecular ground state energies a critical step in predicting whether a drug candidate will bind to its target protein.

Classical computers must approximate electron behavior due to exponential memory requirements. Our approach maps electrons directly onto qubits, enabling precision that classical architecture cannot achieve.

## What This Repository Demonstrates

This notebook implements a complete VQE pipeline using Qiskit Nature:

1. Defines a molecule using PySCF
2. Maps the electronic structure to a qubit Hamiltonian via Jordan-Wigner transformation
3. Runs VQE with a UCCSD ansatz to find the ground state energy
4. Compares results against exact reference values

## Results

| Molecule | Qubits | VQE Energy (Hartree) | Reference (Hartree) | Error (Hartree) | Time |
|----------|--------|----------------------|---------------------|-----------------|------|
| H₂       | 4      | -1.137306            | -1.137270           | 0.000036 ✅     | 0.9s |
| LiH      | 10     | -7.881995            | -7.882362           | 0.000367 ✅     | 42min |

Both molecules achieve **chemical accuracy** (threshold: 0.001 Hartree) on a classical simulator.
LiH required FreezeCoreTransformer + ParamShiftEstimatorGradient to converge efficiently.
On real quantum hardware, larger drug molecules become feasible without approximations.

## Convergence Graphs

Both graphs show VQE energy converging to the exact reference value (red dashed line):

- **H₂**: Converges in 10 evaluations in under 1 second
- **LiH**: Converges in 140 evaluations using FreezeCoreTransformer to reduce computational cost

## How to Run

```bash
# Clone the repository
git clone https://github.com/ayhambadr12/qbind-analytics.git
cd qbind-analytics

# Create virtual environment
python3 -m venv qbind-env
source qbind-env/bin/activate

# Install dependencies
pip install qiskit qiskit-nature qiskit-algorithms qiskit-aer pyscf matplotlib

# Launch Jupyter
jupyter notebook
```

Open `h2_lih_vqe.ipynb` and run all cells.

## System Requirements

> ⚠️ **Important**: This project requires a Linux environment to run.

- **OS**: Ubuntu / Debian Linux (or WSL2 on Windows)
- **Python**: 3.10+
- **RAM**: 8GB minimum, 16GB+ recommended (LiH simulation is memory intensive)
- **PySCF** does not build on Windows natively. Use Ubuntu, WSL2, or Google Colab

### Running on Windows via WSL2

If you are on Windows, enable WSL2 and install Ubuntu from the Microsoft Store, then follow the installation steps above inside the Ubuntu terminal.

### Running on Google Colab or Local Device

```python
!pip install qiskit qiskit-nature qiskit-algorithms qiskit-aer pyscf matplotlib
```

Then upload the notebook and run all cells.

## Tech Stack

- Python 3.14
- Qiskit Nature 0.7.2
- Qiskit Algorithms 0.4.0
- PySCF 2.13.0
- Qiskit Aer 0.17.2

## Key Technical Decisions

- **Jordan-Wigner Mapping**: Converts fermionic molecular Hamiltonian to qubit operators
- **UCCSD Ansatz**: Unitary Coupled Cluster with Singles and Doubles chemically motivated parameterized circuit
- **FreezeCoreTransformer**: Freezes inner core electrons that don't participate in bonding, reducing qubit count
- **ParamShiftEstimatorGradient**: Exact gradient calculation using parameter shift rule, significantly faster than finite differences
- **Early Stopping**: Halts optimization when no improvement detected for 50 consecutive evaluations

## Roadmap

- [x] Local VQE simulation achieving chemical accuracy on H₂ and LiH
- [ ] Cloud deployment on IBM Quantum hardware
- [ ] API interface for enterprise pharmaceutical clients
- [ ] Pilot project with MENA pharmaceutical manufacturer
- [ ] Expand to larger molecules relevant to drug discovery

## Team

- **Ayham Badr** — Co-founder, CS/AI specialist, Alexandria National University
- **Rana Saad** — Co-founder, CS/AI specialist, Zewail City of Science and Technology

---

*Q-Bind Analytics — Alexandria, Egypt*
