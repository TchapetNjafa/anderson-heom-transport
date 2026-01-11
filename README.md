# Anderson Model HEOM Implementation

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18213732.svg)](https://doi.org/10.5281/zenodo.18213732)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Hierarchical Equations of Motion (HEOM) implementation for the Anderson impurity model in Python (QuTiP) and Julia (HierarchicalEOM.jl).

## Paper

This code accompanies the manuscript:

> T. Goumaï Vedekoï, J.-P. Tchapet Njafa, S. G. Nana Engo  
> "Comparative Study of the Anderson Model in Weak and Strong Interaction Regimes"  
> Physical Review B (2026)

**Data/Code**: https://doi.org/10.5281/zenodo.18213732

## Features

- ✅ Python implementation using QuTiP
- ✅ Julia implementation using HierarchicalEOM.jl
- ✅ Temperature-dependent simulations
- ✅ Complete documentation
- ✅ All figures reproducible

## Installation

### Python
```bash
pip install -r requirements.txt
```

### Julia
```julia
using Pkg
Pkg.add("HierarchicalEOM")
Pkg.add("JLD2")
```

## Quick Start

### Run Temperature Study
```bash
# Julia (fast - 2 minutes)
julia code/temperature_study_julia.jl

# Python
python code/temperature_study_python.py

# Analyze
python code/analyze_results.py
```

### Run Notebooks
```bash
jupyter notebook code/DOS_py.ipynb
```

## Documentation

See `documentation/` folder for:
- Installation guide
- Code documentation
- Temperature study guide
- Quick start tutorial

## Citation

If you use this code, please cite:

```bibtex
@article{goumai2026anderson,
  title={Comparative Study of the Anderson Model in Weak and Strong Interaction Regimes: 
         Implementations in Julia (HierarchicalEOM.jl) and Python (QuTiP)},
  author={Goumaï Vedekoï, T. and Tchapet Njafa, J.-P. and Nana Engo, S. G.},
  journal={Physical Review B},
  year={2026},
  note={Code and data: \url{https://doi.org/10.5281/zenodo.18213732}}
}
```

## Data Availability

All code and data are archived at Zenodo: https://doi.org/10.5281/zenodo.18213732

## License

MIT License - see LICENSE.md file

## Contact

J.-P. Tchapet Njafa: jean-pierre.tchapet@facsciences-uy1.cm

## Acknowledgments

This code uses:
- QuTiP (https://qutip.org)
- HierarchicalEOM.jl (https://github.com/NCKU-QFort/HierarchicalEOM.jl)
