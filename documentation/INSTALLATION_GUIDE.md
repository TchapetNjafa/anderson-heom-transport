# Installation Guide

## Quick Start

### For Python (QuTiP) Users

```bash
# 1. Create virtual environment
python3 -m venv heom_env
source heom_env/bin/activate  # On Windows: heom_env\Scripts\activate

# 2. Install dependencies
pip install --upgrade pip
pip install qutip==5.1.1 qutip-bofin numpy scipy matplotlib jupyter

# 3. Launch Jupyter
jupyter notebook

# 4. Open and run: current_py.ipynb, DOS_py.ipynb, population_py.ipynb
```

### For Julia (HierarchicalEOM.jl) Users

```bash
# 1. Install Julia from https://julialang.org/downloads/
# Download Julia 1.10.0 or later

# 2. Start Julia
julia

# 3. Install packages (in Julia REPL)
using Pkg
Pkg.add("HierarchicalEOM")
Pkg.add("QuantumToolbox")
Pkg.add("DifferentialEquations")
Pkg.add("Plots")
Pkg.add("IJulia")

# 4. Build IJulia (for Jupyter integration)
using IJulia
notebook()

# 5. Open and run: DOS_jl.ipynb, current_jl.ipynb, etc.
```

---

## Detailed Installation

### System Requirements

**Minimum:**
- CPU: 4 cores, 2.0 GHz
- RAM: 16 GB
- Storage: 5 GB free space
- OS: Linux, macOS, or Windows 10+

**Recommended:**
- CPU: 24 cores, 3.0 GHz
- RAM: 64 GB
- Storage: 20 GB free space
- OS: Ubuntu 22.04 LTS or macOS 13+

---

### Python Environment Setup

#### Option 1: Using pip (Recommended)

```bash
# Create and activate virtual environment
python3 -m venv heom_env
source heom_env/bin/activate

# Upgrade pip
pip install --upgrade pip setuptools wheel

# Install core dependencies
pip install numpy==1.24.3
pip install scipy==1.10.1
pip install matplotlib==3.7.1

# Install QuTiP
pip install qutip==5.1.1

# Install QuTiP-BoFiN (fermionic extension)
pip install qutip-bofin

# Install Jupyter
pip install jupyter ipykernel

# Register kernel
python -m ipykernel install --user --name=heom_env
```

#### Option 2: Using conda

```bash
# Create conda environment
conda create -n heom_env python=3.11

# Activate environment
conda activate heom_env

# Install dependencies
conda install numpy scipy matplotlib jupyter

# Install QuTiP via pip (not available in conda)
pip install qutip==5.1.1 qutip-bofin
```

#### Verification

```python
# Test installation
python -c "import qutip; print(qutip.__version__)"
# Should print: 5.1.1

python -c "import qutip_bofin; print('BoFiN OK')"
# Should print: BoFiN OK
```

---

### Julia Environment Setup

#### Step 1: Install Julia

**Linux:**
```bash
wget https://julialang-s3.julialang.org/bin/linux/x64/1.10/julia-1.10.0-linux-x86_64.tar.gz
tar -xvzf julia-1.10.0-linux-x86_64.tar.gz
sudo mv julia-1.10.0 /opt/
sudo ln -s /opt/julia-1.10.0/bin/julia /usr/local/bin/julia
```

**macOS:**
```bash
# Using Homebrew
brew install julia

# Or download from https://julialang.org/downloads/
```

**Windows:**
```
Download installer from https://julialang.org/downloads/
Run julia-1.10.0-win64.exe
Add to PATH during installation
```

#### Step 2: Install Julia Packages

```julia
# Start Julia
julia

# In Julia REPL:
using Pkg

# Add required packages
Pkg.add("HierarchicalEOM")
Pkg.add("QuantumToolbox")
Pkg.add("DifferentialEquations")
Pkg.add("Plots")
Pkg.add("CSV")
Pkg.add("DataFrames")
Pkg.add("IJulia")

# Build IJulia for Jupyter integration
using IJulia
IJulia.installkernel("Julia")
```

#### Step 3: Verify Installation

```julia
# Test packages
using HierarchicalEOM
using QuantumToolbox
using DifferentialEquations
using Plots

println("All packages loaded successfully!")
```

---

### Jupyter Setup

#### Launch Jupyter

**Python notebooks:**
```bash
source heom_env/bin/activate  # Activate Python environment
jupyter notebook
```

**Julia notebooks:**
```bash
julia
# In Julia REPL:
using IJulia
notebook()
```

#### Configure Kernels

**List available kernels:**
```bash
jupyter kernelspec list
```

**Should show:**
```
Available kernels:
  heom_env    /path/to/heom_env/share/jupyter/kernels/heom_env
  julia-1.10  /path/to/julia/kernels/julia-1.10
  python3     /path/to/python3/kernels/python3
```

---

### Troubleshooting

#### Issue: "ModuleNotFoundError: No module named 'qutip'"

**Solution:**
```bash
# Ensure virtual environment is activated
source heom_env/bin/activate

# Reinstall QuTiP
pip install --force-reinstall qutip==5.1.1
```

#### Issue: "Julia package not found"

**Solution:**
```julia
# In Julia REPL
using Pkg
Pkg.update()
Pkg.add("PackageName")
```

#### Issue: "Jupyter kernel not found"

**Solution:**
```bash
# Python kernel
python -m ipykernel install --user --name=heom_env

# Julia kernel
julia -e 'using IJulia; IJulia.installkernel("Julia")'
```

#### Issue: "Out of memory during simulation"

**Solution:**
- Reduce Nmax (try 3 instead of 5)
- Close other applications
- Increase system swap space
- Use machine with more RAM

#### Issue: "Julia compilation slow"

**Solution:**
- First run is slow (JIT compilation) - this is normal
- Subsequent runs are fast
- Use `@time` to measure actual runtime (excludes compilation)

---

### Performance Optimization

#### Python (QuTiP)

**1. Use NumPy optimizations:**
```bash
pip install numpy[mkl]  # Intel MKL for faster linear algebra
```

**2. Enable parallel processing:**
```python
import qutip
qutip.settings.num_cpus = 24  # Use all cores
```

**3. Use sparse matrices:**
```python
H_sys = qutip.Qobj(H, type='oper', dims=[[4], [4]])
```

#### Julia

**1. Precompile packages:**
```julia
using Pkg
Pkg.precompile()
```

**2. Use multiple threads:**
```bash
# Set before starting Julia
export JULIA_NUM_THREADS=24
julia
```

**3. Enable optimizations:**
```julia
# In Julia REPL
using LinearAlgebra
BLAS.set_num_threads(24)
```

---

### Testing Installation

#### Run Test Notebooks

**Python:**
```bash
cd Codes_python
jupyter notebook DOS_py.ipynb
# Run all cells (Ctrl+Enter)
# Should complete in ~3 minutes
```

**Julia:**
```bash
cd Codes_python
jupyter notebook DOS_jl.ipynb
# Run all cells
# Should complete in ~20 seconds
```

#### Expected Output

**DOS_py.ipynb:**
- Figure showing spectral function with Kondo peak
- Sum rule check: ~2.0
- Runtime: ~3 minutes

**DOS_jl.ipynb:**
- Similar figure with sharper features
- Sum rule check: ~2.0
- Runtime: ~20 seconds

---

### Updating Packages

#### Python

```bash
source heom_env/bin/activate
pip install --upgrade qutip qutip-bofin numpy scipy matplotlib
```

#### Julia

```julia
using Pkg
Pkg.update()
```

---

### Uninstallation

#### Python

```bash
# Deactivate environment
deactivate

# Remove environment
rm -rf heom_env

# Remove Jupyter kernel
jupyter kernelspec uninstall heom_env
```

#### Julia

```julia
# Remove packages
using Pkg
Pkg.rm("HierarchicalEOM")
Pkg.rm("QuantumToolbox")
# etc.

# Or remove entire Julia installation
# Linux/macOS:
sudo rm -rf /opt/julia-1.10.0
sudo rm /usr/local/bin/julia
```

---

### Getting Help

**QuTiP:**
- Documentation: https://qutip.org/docs/latest/
- Forum: https://groups.google.com/g/qutip
- GitHub: https://github.com/qutip/qutip

**HierarchicalEOM.jl:**
- Documentation: https://qutip.github.io/HierarchicalEOM.jl/
- GitHub: https://github.com/qutip/HierarchicalEOM.jl

**Julia:**
- Documentation: https://docs.julialang.org/
- Forum: https://discourse.julialang.org/

**Contact:**
- Theodore Goumaï: theodore.goumai@facsciences-uy1.cm
