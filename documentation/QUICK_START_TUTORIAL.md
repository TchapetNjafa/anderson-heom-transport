# Quick Start Tutorial - Anderson Impurity Model

## Python (QuTiP) - 5 Minute Start

### Installation
```bash
pip install qutip==5.1.1 qutip-bofin numpy matplotlib jupyter
```

### Minimal Example
```python
from anderson_qutip import *

# 1. Setup parameters
params = AndersonParameters()
params.gamma = 2.0  # Strong coupling regime

# 2. Build Hamiltonian
H_sys = build_system_hamiltonian(params)

# 3. Setup HEOM solver
solver = setup_heom_solver(H_sys, params)

# 4. Calculate steady state
rho_ss = calculate_steady_state(solver)

# 5. Calculate current
I = calculate_current(rho_ss, params)
print(f"Current: {I:.6f} e/ℏ")
```

### Calculate Spectral Function
```python
import numpy as np
import matplotlib.pyplot as plt

# Frequency range
omega = np.linspace(-15, 15, 300)

# Calculate A(ω)
A_omega = calculate_spectral_function(omega, params)

# Plot
plt.plot(omega, A_omega)
plt.xlabel('ω')
plt.ylabel('A(ω)')
plt.title('Spectral Function')
plt.show()

# Check sum rule
integral = check_sum_rule(A_omega, omega)
print(f"Sum rule: {integral:.3f} (should be ~2)")
```

### Scan Bias Voltage
```python
phi_list = np.linspace(0, 4, 20)
current_list = []

for phi in phi_list:
    params.phi = phi
    solver = setup_heom_solver(H_sys, params, verbose=False)
    rho_ss = calculate_steady_state(solver, verbose=False)
    I = calculate_current(rho_ss, params)
    current_list.append(I)

# Plot I-V curve
plt.plot(phi_list, current_list)
plt.xlabel('Bias voltage φ')
plt.ylabel('Current I (e/ℏ)')
plt.show()
```

---

## Julia (HierarchicalEOM.jl) - 5 Minute Start

### Installation
```julia
using Pkg
Pkg.add("HierarchicalEOM")
Pkg.add("QuantumToolbox")
Pkg.add("DifferentialEquations")
Pkg.add("Plots")
```

### Minimal Example
```julia
include("anderson_julia.jl")

# 1. Setup parameters
params = AndersonParameters(Γ=2.0)  # Strong coupling

# 2. Build Hamiltonian
H_sys = build_system_hamiltonian(params)

# 3. Setup HEOM Liouvillian
L_heom = setup_heom_liouvillian(H_sys, params)

# 4. Calculate steady state
ρ_ss = calculate_steady_state(L_heom)

# 5. Calculate current
I = calculate_current(ρ_ss, params)
println("Current: $I e/ℏ")
```

### Calculate Spectral Function
```julia
using Plots

# Frequency range
ωlist = range(-15, 15, length=300)

# Get coupling operator
d_up, _ = get_coupling_operators()

# Calculate A(ω)
A_ω = calculate_spectral_function(ωlist, L_heom, d_up)

# Plot
plot(ωlist, A_ω, xlabel="ω", ylabel="A(ω)", 
     title="Spectral Function", legend=false)

# Check sum rule
integral = check_sum_rule(A_ω, ωlist)
println("Sum rule: $integral (should be ~2)")
```

### Scan Bias Voltage
```julia
φ_list = range(0, 4, length=20)
I_list = Float64[]

for φ in φ_list
    params.φ = φ
    L_heom = setup_heom_liouvillian(H_sys, params, verbose=false)
    ρ_ss = calculate_steady_state(L_heom, verbose=false)
    I = calculate_current(ρ_ss, params)
    push!(I_list, I)
end

# Plot I-V curve
plot(φ_list, I_list, xlabel="Bias voltage φ", 
     ylabel="Current I (e/ℏ)", legend=false)
```

---

## Common Tasks

### Change Coupling Regime

**Strong coupling (U/Γ = 5.0):**
```python
params.gamma = 2.0
```

**Intermediate (U/Γ = 0.5):**
```python
params.gamma = 20.0
```

**Weak coupling (U/Γ = 0.05):**
```python
params.gamma = 200.0
```

### Increase Accuracy

**Higher hierarchy depth:**
```python
params.Nmax = 8  # More accurate, slower
```

**More Padé terms:**
```python
params.Nexp = 7  # Better bath decomposition
```

### Convergence Test

**Python:**
```python
Nmax_values = [3, 5, 8]
results = {}

for Nmax in Nmax_values:
    params.Nmax = Nmax
    solver = setup_heom_solver(H_sys, params, verbose=False)
    rho_ss = calculate_steady_state(solver, verbose=False)
    results[Nmax] = rho_ss[3,3].real  # Double occupation

# Check convergence
for Nmax in Nmax_values:
    print(f"Nmax={Nmax}: ρ_44 = {results[Nmax]:.6f}")
```

**Julia:**
```julia
Nmax_values = [3, 5, 8]
results = convergence_test(params, Nmax_values)

# Check convergence
for Nmax in Nmax_values
    println("Nmax=$Nmax: ρ_44 = $(results[Nmax]["double_occ"])")
end
```

---

## Troubleshooting

### Out of Memory
```python
# Reduce Nmax
params.Nmax = 3

# Or reduce time resolution
tlist = np.linspace(0, 100, 500)  # Fewer points
```

### Slow Convergence
```python
# Increase Padé terms
params.Nexp = 7

# Or adjust tolerances
solver = HEOMSolver(H_sys, baths, max_depth=Nmax,
                    options={'rtol': 1e-5, 'atol': 1e-7})
```

### Negative Populations
```python
# Increase Nmax
params.Nmax = 8

# Or check bath decomposition quality
bath_L.check_decomposition()
```

---

## Performance Tips

### Python
- Use NumPy vectorization
- Enable parallel processing: `qutip.settings.num_cpus = 24`
- Use sparse matrices for large systems

### Julia
- Precompile: Run once to compile, then time
- Use multiple threads: `export JULIA_NUM_THREADS=24`
- Enable BLAS threading: `using LinearAlgebra; BLAS.set_num_threads(24)`

---

## Next Steps

1. **Explore notebooks:** See full examples in `Codes_python/`
2. **Read documentation:** Check `CODE_DOCUMENTATION.md`
3. **Modify parameters:** Experiment with different regimes
4. **Compare frameworks:** Run same calculation in both

---

## Getting Help

**QuTiP:**
- Docs: https://qutip.org/docs/latest/
- Forum: https://groups.google.com/g/qutip

**HierarchicalEOM.jl:**
- Docs: https://qutip.github.io/HierarchicalEOM.jl/
- GitHub: https://github.com/qutip/HierarchicalEOM.jl

**This Project:**
- Email: theodore.goumai@facsciences-uy1.cm
