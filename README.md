# quantum-molecular-ground-state-vqe
This project implements a Variational Quantum Eigensolver (VQE) workflow to estimate molecular ground-state energies using hybrid quantum–classical computation.
🚀 Implementation Overview
1️⃣ Hamiltonian Construction
Generated molecular integrals using PySCF
Constructed second-quantized Hamiltonian
Applied Jordan–Wigner mapping to transform fermionic operators into qubit operators

2️⃣ Ansatz Design
Used EfficientSU2 ansatz
Hardware-efficient layered structure
Parameterized single-qubit rotations + entangling gates
Alternative ansätze studied:
UCCSD (Unitary Coupled Cluster)
Hardware-efficient ansätze variants

3️⃣ Optimization
Classical optimizer: SLSQP
Minimized ⟨ψ(θ)|H|ψ(θ)⟩ iteratively

4️⃣ Simulation Backend
Simulated quantum circuits using Qiskit Aer
Benchmarked results against classical Hartree–Fock energy

🛠 Tech Stack
Python
Qiskit
Qiskit Nature
PySCF
SciPy
Qiskit Aer

📊 Objective
Classical quantum chemistry methods (FCI, CCSD(T), MP2) scale exponentially with system size.
This project demonstrates how VQE can approximate molecular ground-state energies using hybrid quantum–classical workflows.

🔎 Explored alternative mappings:
Bravyi–Kitaev
Parity Mapping
Other optimizers evaluated:
COBYLA
SPSA
L-BFGS-B

📚 References
https://quantum.cloud.ibm.com/learning/en/modules/computer-science/vqe

https://quantum.cloud.ibm.com/learning/en/courses/quantum-chem-with-vqe

https://pyscf.org/quickstart.html

https://qiskit-community.github.io/qiskit-nature/tutorials/01_electronic_structure.html

https://docs.scipy.org/doc/scipy/reference/optimize.html#module-scipy.optimize

Hybrid quantum–classical VQE implementation for molecular ground-state energy simulation using Qiskit and PySCF.
