# Supplementary Materials for "Comparative Analysis of QuTiP and HierarchicalEOM.jl"

## Overview

This repository contains all code, data, and supplementary materials for reproducing the results presented in our manuscript comparing QuTiP and HierarchicalEOM.jl implementations of the Hierarchical Equations of Motion (HEOM) method applied to the Anderson impurity model.

## Repository Structure

```
article_1/
├── tex_files/
│   ├── article_theodore2_humanized.tex    # Main manuscript (LaTeX)
│   ├── article_theodore2_humanized.pdf    # Compiled manuscript
│   └── graphics/                          # All figures used in manuscript
├── Codes_python/
│   ├── current_py.ipynb                   # QuTiP: Current calculations (Γ=2)
│   ├── DOS_py.ipynb                       # QuTiP: Density of states
│   ├── population_py.ipynb                # QuTiP: Population dynamics
│   ├── current_jl.ipynb                   # Julia: Current (Γ=200)
│   ├── current_and_conductance_jl.ipynb   # Julia: Conductance (Γ=20)
│   ├── current_and_conductance_2_jl.ipynb # Julia: Conductance (Γ=2)
│   └── DOS_jl.ipynb                       # Julia: Density of states
├── csv_data/                              # Exported numerical data
└── README.md                              # This file
```

## System Requirements

### Python Environment (QuTiP)
- Python 3.11+
- QuTiP 5.1.1
- QuTiP-BoFiN (fermionic extension)
- NumPy 1.24+
- SciPy 1.10+
- Matplotlib 3.7+

### Julia Environment (HierarchicalEOM.jl)
- Julia 1.10.0+
- HierarchicalEOM.jl 2.5.1
- QuantumToolbox.jl
- DifferentialEquations.jl
- Plots.jl

### Hardware
- CPU: Intel Xeon Gold 6248R (24 cores, 3.0 GHz) or equivalent
- RAM: 128 GB (minimum 32 GB for reduced hierarchy depth)
- OS: Ubuntu 22.04 LTS (or compatible Linux/macOS)

## Installation

### Python Setup

```bash
# Create virtual environment
python3 -m venv heom_env
source heom_env/bin/activate

# Install dependencies
pip install qutip==5.1.1
pip install qutip-bofin
pip install numpy scipy matplotlib jupyter
```

### Julia Setup

```bash
# Install Julia from https://julialang.org/downloads/

# Start Julia and install packages
julia
```

```julia
using Pkg
Pkg.add("HierarchicalEOM")
Pkg.add("QuantumToolbox")
Pkg.add("DifferentialEquations")
Pkg.add("Plots")
Pkg.add("IJulia")  # For Jupyter integration
```

## Simulation Parameters

All simulations use the following parameters (in units where ℏ = kB = 1):

| Parameter | Symbol | Value(s) | Description |
|-----------|--------|----------|-------------|
| Impurity level | ε | -5.0 | Single-particle energy |
| Coulomb repulsion | U | 10.0 | On-site interaction |
| Hybridization | Γ | 2, 20, 200 | System-bath coupling |
| Bath bandwidth | W | 10.0 | Lorentzian width |
| Temperature | T | 0.025 | Thermal energy |
| Bias voltage | φ | 0-4 | Applied voltage |
| Hierarchy depth | Nmax | 3, 5, 8 | HEOM truncation |

### Coupling Regimes

| Γ | U/Γ | Regime | Kondo Temperature (TK) |
|---|-----|--------|------------------------|
| 2 | 5.0 | Strong coupling | ~0.1 |
| 20 | 0.5 | Intermediate | ~1.0 |
| 200 | 0.05 | Weak coupling | ~10.0 |

## Reproducing Results

### Figure 1: Density of States (Strong Coupling, Γ=2)
```bash
cd Codes_python
jupyter notebook DOS_jl.ipynb
# Run all cells
# Output: dos_jl.png
```

### Figure 2: Density of States (Weak Coupling, Γ=200)
```bash
# Same notebook, modify Γ parameter to 200
# Output: dos_jl_1.png
```

### Figure 3: Current vs Bias (QuTiP, Γ=2)
```bash
jupyter notebook current_py.ipynb
# Run all cells
# Output: current_pyw.png
```

### Figure 4: Current vs Bias (Julia, Γ=200)
```bash
jupyter notebook current_jl.ipynb
# Run all cells
# Output: current_jl.png
```

### Figure 5: Conductance (QuTiP, Γ=2)
```bash
jupyter notebook current_py.ipynb
# Calculate numerical derivative of current
# Output: conductance_pyw.png
```

### Figure 6: Conductance (Julia, Γ=2)
```bash
jupyter notebook current_and_conductance_2_jl.ipynb
# Run all cells
# Output: conductance_p.png
```

### Figure 7: Conductance (QuTiP, Γ=200)
```bash
# Modify current_py.ipynb with Γ=200
# Output: conductance_pyw_weak.png
```

### Figure 8: Conductance (Julia, Γ=200)
```bash
jupyter notebook current_and_conductance_jl.ipynb
# Run all cells (Γ=20 by default, modify to 200)
# Output: conductance_jl.png
```

### Figure 9: Population Dynamics (Weak Regime)
```bash
jupyter notebook population_py.ipynb
# Set Γ=2, φ=2
# Output: s_pop_weak.png
```

### Figure 10: Population Dynamics (Strong Regime)
```bash
# Same notebook, set Γ=200
# Output: s_pop_strong.png
```

## Expected Runtime

| Simulation | Framework | Nmax | Runtime | Memory |
|------------|-----------|------|---------|--------|
| DOS | Julia | 5 | ~17s | ~2 GB |
| DOS | QuTiP | 5 | ~3 min | ~10 GB |
| Current | Julia | 5 | ~2 min | ~3 GB |
| Current | QuTiP | 5 | ~15 min | ~15 GB |
| Conductance | Julia | 5 | ~5 min | ~4 GB |
| Conductance | QuTiP | 5 | ~30 min | ~18 GB |

*Note: Times measured on Intel Xeon Gold 6248R (24 cores, 3.0 GHz)*

## Data Availability

All numerical data used to generate figures is available in `csv_data/` directory:
- `dos_data_gamma2.csv` - Spectral function (Γ=2)
- `dos_data_gamma200.csv` - Spectral function (Γ=200)
- `current_data_qutip.csv` - Current vs bias (QuTiP)
- `current_data_julia.csv` - Current vs bias (Julia)
- `conductance_data.csv` - Differential conductance
- `population_dynamics.csv` - Time-dependent populations

## Convergence Analysis

To verify convergence with hierarchy depth:

```julia
# In any Julia notebook
Nmax_values = [3, 5, 8]
results = []
for Nmax in Nmax_values
    # Run simulation with current Nmax
    push!(results, calculate_observable(Nmax))
end

# Calculate relative error
ε_rel = abs(results[3] - results[2]) / abs(results[3])
println("Relative error: ", ε_rel)
# Should be < 5% for convergence
```

## Performance Benchmarking

To reproduce performance comparisons:

```python
# Python (QuTiP)
import time
start = time.time()
# Run simulation
elapsed = time.time() - start
print(f"Runtime: {elapsed:.2f}s")
```

```julia
# Julia
@time begin
    # Run simulation
end
```

## Troubleshooting

### Common Issues

**1. Out of Memory Error**
- Reduce Nmax (try 3 instead of 5)
- Reduce time resolution
- Use smaller bias voltage range

**2. Julia Compilation Overhead**
- First run is slow (JIT compilation)
- Subsequent runs are fast
- Use `@time` to exclude compilation

**3. QuTiP Convergence Issues**
- Increase Padé terms (Nexp)
- Reduce relative/absolute tolerances
- Check bath decomposition quality

**4. Figure Mismatch**
- Verify exact parameter values
- Check random seed (if applicable)
- Ensure same Nmax used

## Citation

If you use this code or data, please cite:

```bibtex
@article{goumai2026comparative,
  title={Comparative Analysis of QuTiP and HierarchicalEOM.jl for Simulating the Anderson Impurity Model},
  author={Goumaï Vedekoï, T. and Tchapet Njafa, J.-P. and Nana Engo, S. G.},
  journal={Physical Review B},
  year={2026},
  note={In preparation}
}
```

## License

This code is released under the MIT License. See LICENSE file for details.

## Contact

For questions or issues:
- Theodore Goumaï: theodore.goumai@facsciences-uy1.cm
- Jean-Pierre Tchapet Njafa: jean-pierre.tchapet@facsciences-uy1.cm

## Acknowledgments

We thank the developers of QuTiP and HierarchicalEOM.jl for creating and maintaining these excellent open-source tools.

## Version History

- v1.0 (2026-01-11): Initial release with manuscript submission
