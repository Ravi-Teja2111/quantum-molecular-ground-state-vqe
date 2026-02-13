# Quantum Simulation of Molecular Ground States

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Qiskit](https://img.shields.io/badge/Qiskit-1.0+-purple.svg)](https://qiskit.org/)

A quantum computing project that demonstrates the use of Variational Quantum Eigensolver (VQE) algorithms to estimate molecular ground-state energies, showcasing the potential of quantum simulation for drug discovery, materials science, and quantum chemistry research.

## 📋 Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Solution Approach](#solution-approach)
- [Technology Stack](#technology-stack)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Results](#results)
- [Advantages](#advantages)
- [Challenges](#challenges)
- [Future Scope](#future-scope)
- [Contributing](#contributing)
- [Authors](#authors)
- [Acknowledgments](#acknowledgments)
- [References](#references)

## 🔬 Overview

Drug discovery and molecular analysis are complex processes that require extensive computational power. Classical computational methods face limitations in accurately modeling quantum systems due to the exponential growth of the Hilbert space with system size.

This project explores the application of **Variational Quantum Eigensolver (VQE)** algorithms on quantum computing platforms to estimate the ground-state energies of molecular systems. By leveraging parameterized quantum circuits like EfficientSU2 and qubit-mapped molecular Hamiltonians, we demonstrate how quantum algorithms can capture electronic correlation beyond classical mean-field approximations.

### Key Features

- ✨ Implementation of VQE for molecular ground-state energy estimation
- 🔄 Comparison with classical Hartree-Fock calculations
- 🎯 Support for various molecular systems using SMILES/PDB formats
- 📊 Visualization of quantum circuits and energy convergence
- 🧪 Modular architecture for easy experimentation

## ❓ Problem Statement

Accurately determining the ground-state energy of molecules is one of the most fundamental and computationally expensive problems in quantum chemistry. Classical methods face several challenges:

- **Exponential scaling**: Full Configuration Interaction (FCI) and Coupled Cluster methods scale exponentially with system size
- **Correlation effects**: Hartree-Fock neglects electron correlation, limiting accuracy
- **Computational cost**: Post-HF methods (CCSD, CCSDT) are impractical for larger molecules
- **Strongly correlated systems**: Traditional approximations break down for complex electron interactions

### Research Question

**Can hybrid quantum-classical algorithms (specifically VQE) provide an efficient and scalable approach for estimating molecular ground-state energies using currently available quantum simulation platforms?**

## 💡 Solution Approach

Our solution implements a hybrid quantum-classical workflow:

### Workflow

```
1. Molecular Input Data (.xyz, .pdb, SMILES)
         ↓
2. Hamiltonian Construction
   - Second quantization
   - Qubit mapping (JW/BK/Parity)
         ↓
3. Ansatz Selection (EfficientSU2/UCCSD)
         ↓
4. VQE Optimization Loop
   - Quantum state preparation
   - Energy expectation measurement
   - Classical parameter optimization
         ↓
5. Ground-State Energy Output
   - Compare with HF/FCI
   - Analysis and visualization
```

### Mathematical Foundation

The electronic Hamiltonian in second quantization:

```
H = Σ h_pq a†_p a_q + (1/2) Σ h_pqrs a†_p a†_q a_r a_s
```

Where:
- `p,q,r,s` are spin-orbital indices
- `h_pq` are one-electron integrals
- `h_pqrs` are two-electron integrals
- `a†, a` are fermionic creation/annihilation operators

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Quantum Framework | **Qiskit** | Quantum circuit construction and simulation |
| Quantum Chemistry | **PySCF** | Molecular integrals and Hartree-Fock calculations |
| Optimization | **SciPy** | Classical optimization (SLSQP, COBYLA) |
| Simulation Backend | **Qiskit Aer** | High-performance quantum circuit simulation |
| Programming Language | **Python 3.8+** | Implementation language |

## 📦 Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager
- Virtual environment (recommended)

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/quantum-molecular-simulation.git
cd quantum-molecular-simulation

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Requirements.txt

```
qiskit>=1.0.0
qiskit-aer>=0.13.0
qiskit-nature>=0.7.0
pyscf>=2.3.0
scipy>=1.10.0
numpy>=1.24.0
matplotlib>=3.7.0
jupyter>=1.0.0
```

## 🚀 Usage

### Basic Example

```python
from qiskit_nature.units import DistanceUnit
from qiskit_nature.second_q.drivers import PySCFDriver
from qiskit_nature.second_q.mappers import JordanWignerMapper
from qiskit.algorithms.minimum_eigensolvers import VQE
from qiskit.algorithms.optimizers import SLSQP
from qiskit.primitives import Estimator
from qiskit.circuit.library import EfficientSU2

# Define molecule
driver = PySCFDriver(
    atom="H 0 0 0; H 0 0 0.735",
    basis="sto3g",
    charge=0,
    spin=0,
    unit=DistanceUnit.ANGSTROM
)

# Get molecular problem
problem = driver.run()

# Map to qubits
mapper = JordanWignerMapper()
qubit_op = mapper.map(problem.second_q_ops()[0])

# Setup VQE
ansatz = EfficientSU2(num_qubits=qubit_op.num_qubits)
optimizer = SLSQP(maxiter=100)
estimator = Estimator()

vqe = VQE(estimator, ansatz, optimizer)

# Run calculation
result = vqe.compute_minimum_eigenvalue(qubit_op)

print(f"VQE Energy: {result.eigenvalue}")
print(f"HF Energy: {problem.reference_energy}")
```

### Running Jupyter Notebooks

```bash
jupyter notebook examples/
```

## 📁 Project Structure

```
quantum-molecular-simulation/
├── src/
│   ├── hamiltonian/       # Hamiltonian construction utilities
│   ├── vqe/               # VQE implementation
│   ├── ansatz/            # Quantum circuit ansätze
│   └── utils/             # Helper functions
├── examples/
│   ├── h2_molecule.ipynb  # H2 molecule example
│   ├── lih_molecule.ipynb # LiH molecule example
│   └── water.ipynb        # H2O molecule example
├── tests/                 # Unit tests
├── docs/                  # Documentation
├── results/               # Simulation results
├── requirements.txt       # Python dependencies
├── README.md             # This file
└── LICENSE               # MIT License
```

## 📊 Results

### Example: H2 Molecule Simulation

| Method | Energy (Hartree) | Computation Time |
|--------|------------------|------------------|
| Hartree-Fock | -1.1168 | < 1s |
| VQE (EfficientSU2) | -1.0614 | ~30s |
| Correlation Energy | -1.0614 | - |

*Energy difference captures electron correlation beyond mean-field approximation*

### Circuit Visualization

```
q_0: ─ Rx(θ[0]) ─ Y ─■─ Rx(θ[4]) ──── Y ────
q_1: ─ Rx(θ[1]) ─ Y ─X─■─ Rx(θ[5]) ─ Y ────
q_2: ─ Rx(θ[2]) ─ Y ──── X ─■─ Rx(θ[6]) ─ Y
q_3: ─ Rx(θ[3]) ─ Y ──────── X ─ Rx(θ[7]) ─ Y
```

## ✅ Advantages

### Computational Benefits
- **Scalability**: Better scaling than classical FCI/CCSD(T) methods
- **Correlation**: Naturally captures electron correlation effects
- **Flexibility**: Modular architecture allows easy customization
- **Future-ready**: Compatible with emerging quantum hardware

### Real-World Impact
- 💊 **Faster drug discovery** - reducing time to market for new medicines
- 🔋 **Better batteries** - longer life and faster charging for EVs
- 🏭 **Cleaner manufacturing** - efficient catalysts and sustainable processes
- 🌾 **Improved agriculture** - better fertilizers for food production
- 🔬 **Advanced materials** - stronger, lighter materials for various applications

## 🚧 Challenges

### Implementation Challenges
- **NISQ limitations**: Current quantum hardware is noisy and limited in qubit count
- **Software instability**: Rapid evolution of quantum frameworks causes compatibility issues
- **Learning curve**: Requires deep understanding of quantum mechanics, chemistry, and programming
- **Resource intensive**: Large molecules require significant computational resources

### Lessons Learned
- Every quantum concept requires extensive research and understanding
- Open-source examples often outdated due to rapid framework updates
- AI tools helpful but not always reliable for quantum code generation
- Strong fundamentals essential to avoid frustration and project abandonment

## 🔮 Future Scope

### Market Opportunities
- Global quantum computing market growing exponentially
- High-impact domains: pharmaceuticals, energy, aerospace, materials science
- Massive career opportunities in quantum algorithms, software, and hardware

### Future Developments
- **AI Integration**: Hybrid quantum-AI drug design pipelines
- **Larger Biomolecules**: Advanced ansätze for protein modeling
- **Personalized Medicine**: Tailored drug discovery for genetic structures
- **Quantum Data Marketplaces**: Secure institutional data sharing
- **Open Collaboration**: University-industry quantum API development

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 👥 Authors

**Dasari Ravi Teja** - R210225  
**Thirumalasetty Lavanya** - R211003

**Supervisor**: Dr. Priyanka Sahu, PhD  
Department of Electronics and Communication Engineering  
Rajiv Gandhi University of Knowledge Technologies (RGUKT)  
RK Valley, Kadapa, Andhra Pradesh - 516330

## 🙏 Acknowledgments

We express our gratitude to:
- Dr. A.V.S.S. Kumara Swami Gupta, Director, RGUKT RK Valley
- Dr. B.V. Sudhakar Reddy, Head of Department, ECE
- Dr. Priyanka Sahu, Project Guide
- Faculty members of ECE Department for their support
- The Qiskit and PySCF communities for excellent documentation

## 📚 References

1. [Qiskit Nature Documentation](https://qiskit-community.github.io/qiskit-nature/)
2. [IBM Quantum Learning](https://learning.quantum.ibm.com/)
3. [PySCF Documentation](https://pyscf.org/)
4. [SciPy Optimization](https://docs.scipy.org/doc/scipy/reference/optimize.html)
5. Peruzzo, A., et al. (2014). "A variational eigenvalue solver on a photonic quantum processor." Nature Communications.
6. Shor, P. W. (1994). "Algorithms for quantum computation: discrete logarithms and factoring."
7. Schrödinger, E. (1926). "An Undulatory Theory of the Mechanics of Atoms and Molecules."

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📧 Contact

For questions and feedback:
- Email: raviteja.r210225@rguktrkv.ac.in
- Project Link: [https://github.com/yourusername/quantum-molecular-simulation](https://github.com/yourusername/quantum-molecular-simulation)

---

**Note**: This project was completed as part of the B.Tech degree requirements in Electronics and Communication Engineering at RGUKT RK Valley (2025-2026).

**⚛️ Quantum computing is reshaping scientific discovery - one molecule at a time.**
