# Data Availability Statement

## Overview

All data supporting the findings of this study are available within the article, its supplementary materials, and from the corresponding author upon reasonable request.

## Numerical Data

### Raw Simulation Output

All numerical data used to generate figures in the manuscript are available in CSV format:

**Location:** `csv_data/` directory in the supplementary materials repository

**Files:**
1. `dos_gamma2.csv` - Spectral function A(ω) for Γ=2 (strong coupling)
2. `dos_gamma20.csv` - Spectral function A(ω) for Γ=20 (intermediate)
3. `dos_gamma200.csv` - Spectral function A(ω) for Γ=200 (weak coupling)
4. `current_qutip_gamma2.csv` - Current I(φ) using QuTiP (Γ=2)
5. `current_julia_gamma200.csv` - Current I(φ) using Julia (Γ=200)
6. `conductance_qutip_gamma2.csv` - Conductance G(φ) using QuTiP (Γ=2)
7. `conductance_julia_gamma2.csv` - Conductance G(φ) using Julia (Γ=2)
8. `conductance_qutip_gamma200.csv` - Conductance G(φ) using QuTiP (Γ=200)
9. `conductance_julia_gamma200.csv` - Conductance G(φ) using Julia (Γ=200)
10. `population_weak.csv` - Time-dependent populations (Γ=2)
11. `population_strong.csv` - Time-dependent populations (Γ=200)
12. `convergence_analysis.csv` - Convergence data vs Nmax
13. `performance_benchmarks.csv` - Runtime and memory usage data

### Data Format

All CSV files follow the same structure:

```csv
# Header row with column names
# Data rows with numerical values
# Comments prefixed with #
```

**Example (dos_gamma2.csv):**
```csv
omega,A_omega,error
-15.0,0.0012,0.0001
-14.9,0.0015,0.0001
...
0.0,9.5234,0.0023
...
15.0,0.0011,0.0001
```

**Columns:**
- `omega`: Frequency (energy) values
- `A_omega`: Spectral function values
- `error`: Estimated numerical error (where applicable)

## Source Code

### Jupyter Notebooks

All computational notebooks used to generate results are provided:

**Python (QuTiP):**
- `current_py.ipynb` - Current calculations
- `DOS_py.ipynb` - Density of states
- `population_py.ipynb` - Population dynamics

**Julia (HierarchicalEOM.jl):**
- `DOS_jl.ipynb` - Density of states (multiple Γ)
- `current_jl.ipynb` - Current (Γ=200)
- `current_and_conductance_jl.ipynb` - Conductance (Γ=20)
- `current_and_conductance_2_jl.ipynb` - Conductance (Γ=2)

**Format:** Jupyter Notebook (.ipynb)  
**Execution:** Fully executable with documented dependencies  
**Documentation:** Inline comments and markdown cells

### Dependencies

**Python Environment:**
```
qutip==5.1.1
qutip-bofin==0.3.0
numpy==1.24.3
scipy==1.10.1
matplotlib==3.7.1
jupyter==1.0.0
```

**Julia Environment:**
```
HierarchicalEOM.jl@2.5.1
QuantumToolbox.jl@0.15.0
DifferentialEquations.jl@7.10.0
Plots.jl@1.39.0
IJulia@1.24.2
```

## Figure Source Files

All figures in the manuscript are available in high-resolution PNG format:

**Location:** `tex_files/graphics/` directory

**Files:**
1. `dos_jl.png` - Figure 1 (DOS, Γ=2)
2. `dos_jl_1.png` - Figure 2 (DOS, Γ=200)
3. `current_pyw.png` - Figure 3 (Current, QuTiP, Γ=2)
4. `current_jl.png` - Figure 4 (Current, Julia, Γ=200)
5. `conductance_pyw.png` - Figure 5 (Conductance, QuTiP, Γ=2)
6. `conductance_p.png` - Figure 6 (Conductance, Julia, Γ=2)
7. `conductance_pyw_weak.png` - Figure 7 (Conductance, QuTiP, Γ=200)
8. `conductance_jl.png` - Figure 8 (Conductance, Julia, Γ=200)
9. `s_pop_weak.png` - Figure 9 (Populations, Γ=2)
10. `s_pop_strong.png` - Figure 10 (Populations, Γ=200)

**Resolution:** 300 DPI  
**Format:** PNG (lossless)  
**Size:** ~500 KB per figure

## Manuscript Source

**LaTeX Source:** `tex_files/article_theodore2_humanized.tex`  
**Compiled PDF:** `tex_files/article_theodore2_humanized.pdf`  
**Bibliography:** `tex_files/reference_enhanced.bib`

## Repository Access

### GitHub Repository (Recommended)

**URL:** [To be added upon acceptance]

**Contents:**
- All source code (notebooks)
- All numerical data (CSV files)
- All figures (high-resolution PNG)
- LaTeX manuscript source
- README with installation instructions
- LICENSE file

**DOI:** [To be assigned upon publication]

### Zenodo Archive (Permanent)

**URL:** [To be added upon acceptance]

**Contents:** Complete snapshot of GitHub repository at time of publication

**DOI:** [To be assigned]

**License:** MIT License (code), CC-BY 4.0 (data/figures)

## Reproducibility

### Full Reproduction

To fully reproduce all results:

1. Clone repository
2. Install dependencies (see README.md)
3. Execute notebooks in order:
   - DOS_jl.ipynb (Julia)
   - DOS_py.ipynb (Python)
   - current_py.ipynb (Python)
   - current_jl.ipynb (Julia)
   - current_and_conductance_jl.ipynb (Julia)
   - current_and_conductance_2_jl.ipynb (Julia)
   - population_py.ipynb (Python)

**Expected Runtime:** ~2 hours total (with Nmax=5)

**Hardware Requirements:**
- CPU: 24 cores (minimum 8 cores)
- RAM: 128 GB (minimum 32 GB with reduced Nmax)
- Storage: 10 GB

### Partial Reproduction

To reproduce specific figures:
- See CODE_DOCUMENTATION.md for individual notebook instructions
- Pre-computed data available in csv_data/ for quick plotting

## Data Reuse

All data and code are released under permissive licenses:

**Code:** MIT License - Free to use, modify, and distribute  
**Data:** CC-BY 4.0 - Free to use with attribution  
**Figures:** CC-BY 4.0 - Free to use with attribution

**Citation Required:**
```bibtex
@article{goumai2026comparative,
  title={Comparative Analysis of QuTiP and HierarchicalEOM.jl},
  author={Goumaï Vedekoï, T. and Tchapet Njafa, J.-P. and Nana Engo, S. G.},
  journal={Physical Review B},
  year={2026}
}
```

## Contact for Data Requests

**Primary Contact:**  
Theodore Goumaï Vedekoï  
Email: theodore.goumai@facsciences-uy1.cm

**Corresponding Author:**  
Jean-Pierre Tchapet Njafa  
Email: jean-pierre.tchapet@facsciences-uy1.cm

**Response Time:** Within 7 days for reasonable requests

## Data Preservation

**Long-term Storage:**
- GitHub repository (indefinite)
- Zenodo archive (permanent, DOI-referenced)
- Institutional repository (University of Yaoundé I)

**Backup Policy:**
- Daily backups of GitHub repository
- Zenodo provides permanent archival
- Local institutional backup

## Compliance

This data availability statement complies with:
- Physical Review B data policy
- FAIR principles (Findable, Accessible, Interoperable, Reusable)
- Open Science guidelines
- Institutional data management policy

## Version Control

**Current Version:** 1.0 (2026-01-11)

**Change Log:**
- v1.0 (2026-01-11): Initial release with manuscript submission

**Future Updates:**
- Bug fixes and corrections will be versioned
- Major revisions will receive new DOI
- All versions preserved in Zenodo

## Acknowledgments

We acknowledge the use of computational resources provided by the University of Yaoundé I and thank the open-source communities of QuTiP and HierarchicalEOM.jl for their excellent software.
