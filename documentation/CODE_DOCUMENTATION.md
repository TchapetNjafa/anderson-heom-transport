# Code Documentation Guide

## Notebook Organization

### Python Notebooks (QuTiP)

#### 1. `current_py.ipynb` - Current-Voltage Characteristics
**Purpose:** Calculate steady-state current vs bias voltage using QuTiP

**Parameters:**
- ε = -5.0
- U = 10.0
- Γ = 2.0 (strong coupling regime)
- W = 10.0
- T = 0.025
- Nmax = 5

**Key Functions:**
```python
# Bath setup
bath_L = LorentzianEnvironment(T, mu_L, gamma, W)
bath_R = LorentzianEnvironment(T, mu_R, gamma, W)

# HEOM solver
solver = HEOMSolver(H_sys, bath_list, max_depth=Nmax)

# Current calculation
current = calculate_current(rho_ss, bath_L, bath_R)
```

**Output:** `current_pyw.png`

**Runtime:** ~15 minutes (Nmax=5)

---

#### 2. `DOS_py.ipynb` - Density of States
**Purpose:** Calculate spectral function A(ω) using QuTiP

**Parameters:**
- Same as current_py.ipynb
- Frequency range: ω ∈ [-15, 15]
- Resolution: 0.1

**Key Functions:**
```python
# Spectral function
A_omega = calculate_spectral_function(omega, solver)

# Sum rule check
integral = np.trapz(A_omega, omega)
print(f"Sum rule: {integral:.3f} (should be ~2)")
```

**Output:** `dos_py.png`

**Runtime:** ~3 minutes (Nmax=5)

---

#### 3. `population_py.ipynb` - Population Dynamics
**Purpose:** Time evolution of impurity occupation probabilities

**Parameters:**
- Γ = 2.0 or 200.0 (weak/strong regime)
- φ = 2.0 (bias voltage)
- Time range: t ∈ [0, 100]

**Key Functions:**
```python
# Time evolution
result = solver.run(rho0, tlist)

# Extract populations
rho_11 = [rho.diag()[0] for rho in result.states]  # Empty
rho_22 = [rho.diag()[1] for rho in result.states]  # Spin-up
rho_33 = [rho.diag()[2] for rho in result.states]  # Spin-down
rho_44 = [rho.diag()[3] for rho in result.states]  # Doubly occupied
```

**Output:** `s_pop_weak.png`, `s_pop_strong.png`

**Runtime:** ~10 minutes per regime

---

### Julia Notebooks (HierarchicalEOM.jl)

#### 4. `DOS_jl.ipynb` - Density of States
**Purpose:** Calculate spectral function using HierarchicalEOM.jl

**Parameters:**
- ε = -5.0
- U = 10.0
- Γ = 2.0, 20.0, or 200.0
- W = 10.0
- T = 0.025
- Nmax = 5

**Key Functions:**
```julia
# Bath setup
bath_L = Fermion_Lorentz_Pade(T, μ_L, Γ, W, N_exp)
bath_R = Fermion_Lorentz_Pade(T, μ_R, Γ, W, N_exp)

# HEOM Liouvillian
L_heom = M_Fermion([bath_L, bath_R], H_sys, Nmax)

# Spectral function
A_ω = PowerSpectrum(L_heom, d_up, ωlist)
```

**Output:** `dos_jl.png`, `dos_jl_1.png`

**Runtime:** ~17 seconds (Nmax=5)

---

#### 5. `current_jl.ipynb` - Current (Weak Coupling)
**Purpose:** Calculate current vs bias for Γ=200

**Parameters:**
- Γ = 200.0 (weak coupling)
- φ range: [0, 4]
- Nmax = 5

**Key Functions:**
```julia
# Steady state
ρ_ss = SteadyState(L_heom)

# Current calculation
I = expect(J_op, ρ_ss)
```

**Output:** `current_jl.png`

**Runtime:** ~2 minutes

---

#### 6. `current_and_conductance_jl.ipynb` - Conductance (Intermediate)
**Purpose:** Calculate conductance for Γ=20

**Parameters:**
- Γ = 20.0 (intermediate regime)
- φ range: [0, 4]
- Nmax = 5

**Key Functions:**
```julia
# Current array
I_array = [calculate_current(φ) for φ in φ_list]

# Numerical derivative (conductance)
G = diff(I_array) ./ diff(φ_list)
```

**Output:** `conductance_jl.png`

**Runtime:** ~5 minutes

---

#### 7. `current_and_conductance_2_jl.ipynb` - Conductance (Strong Coupling)
**Purpose:** Calculate conductance for Γ=2

**Parameters:**
- Γ = 2.0 (strong coupling)
- φ range: [0, 4]
- Nmax = 5

**Key Functions:**
```julia
# Same as current_and_conductance_jl.ipynb
# but with Γ = 2.0
```

**Output:** `conductance_p.png`

**Runtime:** ~5 minutes

---

## Modifying Parameters

### To Change Coupling Regime:

**Python (QuTiP):**
```python
# In notebook cell
gamma = 2.0    # Strong coupling
# gamma = 20.0   # Intermediate
# gamma = 200.0  # Weak coupling
```

**Julia:**
```julia
# In notebook cell
Γ = 2.0    # Strong coupling
# Γ = 20.0   # Intermediate
# Γ = 200.0  # Weak coupling
```

### To Change Hierarchy Depth:

**Python:**
```python
Nmax = 5  # Standard
# Nmax = 3  # Faster, less accurate
# Nmax = 8  # Slower, more accurate
```

**Julia:**
```julia
Nmax = 5  # Standard
# Nmax = 3  # Faster
# Nmax = 8  # Slower
```

### To Change Temperature:

**Both:**
```python/julia
T = 0.025  # Standard
# T = 0.01   # Lower temperature (requires higher Nmax)
# T = 0.05   # Higher temperature
```

---

## Common Modifications

### 1. Increase Resolution

**Frequency (DOS):**
```julia
ωlist = range(-15, 15, length=500)  # Higher resolution
```

**Bias voltage:**
```julia
φ_list = range(0, 4, length=100)  # More points
```

### 2. Extend Time Range

**Population dynamics:**
```python
tlist = np.linspace(0, 200, 1000)  # Longer time
```

### 3. Change Bath Parameters

**Bandwidth:**
```julia
W = 20.0  # Wider bath
```

**Padé terms:**
```julia
N_exp = 7  # More exponentials (better accuracy)
```

---

## Validation Checks

### 1. Sum Rule (DOS)
```julia
# Should equal 2 (spin degeneracy)
sum_rule = sum(A_ω) * (ωlist[2] - ωlist[1])
@assert abs(sum_rule - 2.0) < 0.1 "Sum rule violated!"
```

### 2. Current Symmetry
```julia
# I(-φ) = -I(φ) for symmetric system
@assert abs(I(φ) + I(-φ)) < 1e-6 "Current asymmetry!"
```

### 3. Convergence
```julia
# Compare Nmax = 5 vs Nmax = 8
ε_rel = abs(result_8 - result_5) / abs(result_8)
@assert ε_rel < 0.05 "Not converged!"
```

---

## Performance Tips

### Python (QuTiP)
1. Use sparse matrices: `H_sys = qutip.Qobj(H, type='oper')`
2. Reduce tolerance: `rtol=1e-6, atol=1e-8`
3. Limit Padé terms: `N_exp=5` (not 7)

### Julia
1. Precompile: Run once to compile, then time
2. Use `@time` not `@elapsed` for first run
3. Allocate arrays: `A_ω = zeros(length(ωlist))`

---

## Exporting Data

### Python
```python
import pandas as pd
df = pd.DataFrame({'omega': omega, 'A_omega': A_omega})
df.to_csv('dos_data.csv', index=False)
```

### Julia
```julia
using CSV, DataFrames
df = DataFrame(ω=ωlist, A=A_ω)
CSV.write("dos_data.csv", df)
```

---

## Troubleshooting

### Issue: "Out of memory"
**Solution:** Reduce Nmax or time resolution

### Issue: "Convergence failed"
**Solution:** Increase Padé terms or reduce tolerances

### Issue: "Negative populations"
**Solution:** Check bath decomposition quality, increase Nmax

### Issue: "Julia slow on first run"
**Solution:** Normal (JIT compilation), subsequent runs are fast

---

## Contact for Code Issues

Theodore Goumaï: theodore.goumai@facsciences-uy1.cm
