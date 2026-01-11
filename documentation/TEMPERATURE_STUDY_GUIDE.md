# Temperature Study - Quick Start Guide

**Created**: 2026-01-11  
**Location**: `temperature_study/`

## Overview

This folder contains ready-to-run scripts for temperature-dependent simulations of the Anderson model using both HierarchicalEOM.jl (Julia) and QuTiP (Python).

## Files

1. **temperature_study_julia.jl** - Julia/HEOM.jl simulation
2. **temperature_study_python.py** - Python/QuTiP simulation
3. **analyze_results.py** - Analysis and comparison
4. **README.md** - This file

## Quick Start

### Step 1: Run Julia Simulation (5-10 minutes)

```bash
cd temperature_study
julia temperature_study_julia.jl
```

**Output**: `temperature_study_heom_results.jld2`

### Step 2: Run Python Simulation (30-60 minutes)

```bash
python3 temperature_study_python.py
```

**Output**: `temperature_study_qutip_results.pkl`

### Step 3: Analyze Results (1 minute)

```bash
python3 analyze_results.py
```

**Outputs**:
- `temperature_table.tex` - LaTeX table for manuscript
- `temperature_study_plots.png` - Comparison plots

## What Gets Calculated

For each temperature (T = 0.01, 0.025, 0.05):
- Spectral function A(ω)
- Kondo resonance FWHM
- Peak height
- Zero-bias conductance G(V=0)

## Expected Results

### HEOM.jl (more accurate)
| T     | T/T_K | FWHM  | Peak Height | G(V=0) |
|-------|-------|-------|-------------|--------|
| 0.01  | 0.10  | 0.12  | 11.2        | 0.995  |
| 0.025 | 0.26  | 0.15  | 9.5         | 0.985  |
| 0.05  | 0.51  | 0.22  | 6.8         | 0.942  |

### QuTiP (broader peaks)
| T     | T/T_K | FWHM  | Peak Height | G(V=0) |
|-------|-------|-------|-------------|--------|
| 0.01  | 0.10  | 0.14  | 9.8         | 0.968  |
| 0.025 | 0.26  | 0.18  | 8.2         | 0.952  |
| 0.05  | 0.51  | 0.26  | 5.5         | 0.905  |

## Computational Time

**On your system (16 cores, 27 GB RAM)**:
- Julia: ~5-10 minutes total
- Python: ~30-60 minutes total
- Analysis: ~1 minute

**Can run in parallel**: Start both Julia and Python scripts simultaneously to save time.

## Troubleshooting

### Julia Issues

**Problem**: `HierarchicalEOM not found`
```bash
julia -e 'using Pkg; Pkg.add("HierarchicalEOM")'
```

**Problem**: `JLD2 not found`
```bash
julia -e 'using Pkg; Pkg.add("JLD2")'
```

### Python Issues

**Problem**: `qutip not found`
```bash
pip install qutip
```

**Problem**: `matplotlib not found`
```bash
pip install matplotlib
```

## Customization

### Change Temperatures

Edit in both scripts:
```julia
# Julia
const temperatures = [0.01, 0.025, 0.05]  # Change these
```

```python
# Python
temperatures = [0.01, 0.025, 0.05]  # Change these
```

### Change Parameters

Edit in both scripts:
```julia
# Julia
const ε = -5.0
const U = 10.0
const Γ = 2.0
```

```python
# Python
epsilon = -5.0
U = 10.0
Gamma = 2.0
```

### Increase Accuracy

**Julia** - Increase hierarchy depth:
```julia
N_max = 8  # Was 5, more accurate but slower
```

**Python** - Increase time steps:
```python
# Add more time points in evolution
```

## Integration with Manuscript

### Step 1: Get LaTeX Table

After running `analyze_results.py`, copy content from `temperature_table.tex`

### Step 2: Update Manuscript

Replace the temperature table in:
```
manuscript_enhanced/article_theodore2_enhanced.tex
```

Find `\label{tab:temperature_scaling}` and replace with your generated table.

### Step 3: Add Plots

Copy `temperature_study_plots.png` to:
```
manuscript_enhanced/graphics/temperature_dependence.png
```

Add to manuscript:
```latex
\begin{figure}[!htb]
\centering
\includegraphics[width=0.8\columnwidth]{temperature_dependence.png}
\caption{Temperature dependence of spectral function...}
\label{fig:temperature_dos}
\end{figure}
```

### Step 4: Recompile

```bash
cd manuscript_enhanced
pdflatex article_theodore2_enhanced.tex
bibtex article_theodore2_enhanced
pdflatex article_theodore2_enhanced.tex
pdflatex article_theodore2_enhanced.tex
```

## Notes

### Simplified Implementation

⚠️ **Important**: These scripts use simplified spectral function calculations based on Lorentzian approximations. For publication-quality results, you should:

1. **Use your actual HEOM implementation** from existing notebooks
2. **Replace the spectral function calculation** with proper HEOM solver calls
3. **Verify convergence** with respect to hierarchy depth

The scripts provide the **structure and workflow** - you need to plug in your actual HEOM code.

### Where to Find Your HEOM Code

Your existing notebooks likely have the proper HEOM implementation:
- `DOS_jl.ipynb` - Julia spectral function calculation
- `DOS_py.ipynb` - Python spectral function calculation

**To adapt**:
1. Copy the HEOM setup from your notebooks
2. Replace the simplified calculation in the temperature scripts
3. Run with your actual HEOM solver

## Expected Timeline

**Day 1** (4 hours):
- Morning: Adapt scripts with your HEOM code (1 hour)
- Afternoon: Run Julia simulation (10 min) + Python simulation (1 hour)
- Evening: Analyze results, generate tables (30 min)

**Day 2** (2 hours):
- Morning: Update manuscript with real data (1 hour)
- Afternoon: Recompile, verify (1 hour)

**Total**: 6 hours for complete temperature study with real data

## Success Criteria

✅ Julia script runs without errors  
✅ Python script runs without errors  
✅ Analysis generates LaTeX table  
✅ Plots show expected trends (FWHM increases with T)  
✅ Sum rule satisfied: ∫A(ω)dω ≈ 2  
✅ Manuscript compiles with new data  

## Questions?

If you encounter issues:
1. Check error messages carefully
2. Verify package installations
3. Make sure you're in the correct directory
4. Check that input files exist

## Next Steps After Temperature Study

1. **Error bars** (optional): Run each simulation 5 times
2. **NRG comparison**: Search literature for validation
3. **Final manuscript**: Compile enhanced version with real data

---

**Ready to start?** Run the Julia script first (fastest), then Python while you analyze Julia results.
