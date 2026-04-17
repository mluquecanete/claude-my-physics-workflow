---
paths:
  - "**/*.py"
  - "notebooks/**/*.ipynb"
  - "scripts/**/*.py"
---

# Python Code Standards

**Standard:** research-grade scientific Python — correct, reproducible, readable.

---

## 1. Reproducibility

- `np.random.seed(YYYYMMDD)` called ONCE at top of any stochastic script
- All imports at top of file (PEP 8)
- All paths relative to repository root via `pathlib.Path`
- Create output directories explicitly: `Path("output/figures").mkdir(parents=True, exist_ok=True)`

## 2. Imports and Dependencies

```python
# Standard order: stdlib → numpy/scipy → project modules
import numpy as np
import scipy.constants as const
from pathlib import Path
```

- Use `scipy.constants` for physical constants (`const.hbar`, `const.k`, `const.e`) — never magic numbers
- Never `from numpy import *` or `from scipy import *`

## 3. Function Design

- `snake_case` naming, verb-noun pattern (`compute_scattering_rate`, `plot_mobility_vs_temp`)
- NumPy-style docstrings for any function >5 lines
- No magic numbers — define named constants at module top with unit comments
- Type-annotate function signatures for clarity

## 4. Units Convention

- All physical quantities carry an inline unit comment:
  ```python
  v_fermi = 5e5   # m/s, Bi2Se3 surface state Fermi velocity
  omega_LO = 3e13 # rad/s, longitudinal optical phonon frequency
  ```
- Use SI throughout; convert to display units only at plot time

## 5. Figure Standards

```python
fig, ax = plt.subplots(figsize=(6, 4))
# ... plotting ...
ax.set_xlabel("Temperature (K)")
ax.set_ylabel(r"Mobility (cm$^2$ V$^{-1}$ s$^{-1}$)")
fig.tight_layout()
fig.savefig("output/figures/mobility_vs_T.pdf", dpi=300, bbox_inches="tight")
fig.savefig("output/figures/mobility_vs_T.png", dpi=300, bbox_inches="tight")
```

- Save as **both** `.pdf` (LaTeX inclusion) and `.png` (preview/notebook)
- Axis labels always include units; LaTeX math in labels via raw strings
- No `plt.show()` in production scripts (use `plt.savefig` only)

## 6. Heavy-Computation Output Pattern

**Heavy computations saved as `.npy`/`.npz`; slides/notebooks load pre-computed data.**

```python
np.save("output/scattering_rates.npy", gamma_array)
# Later:
gamma_array = np.load("output/scattering_rates.npy")
```

- Document array shape and axis meanings in a comment above the save call
- `.npz` for multiple named arrays: `np.savez("output/results.npz", gamma=..., T_grid=...)`

## 7. Numerical Discipline

- **No float equality.** Never `a == b` on floats. Use `np.isclose(a, b, rtol=1e-6)`.
- **Wavevector grid convergence.** Document the grid density used and any convergence test performed.
- **Pre-allocate arrays** before loops: `result = np.zeros((N_k, N_T))`, never `np.append` in a loop.
- **Avoid silent broadcasting errors.** Use explicit `axis=` arguments in `np.sum`, `np.mean`, etc.
- **Document convergence parameters** as named constants at top of script.

## 8. Line Length & Mathematical Exceptions

**Standard:** keep lines ≤ 100 characters.

**Exception:** physics formula implementations may exceed 100 chars **if and only if:**
1. Breaking the line harms readability of the formula
2. An inline comment identifies the physical expression:
   ```python
   # Fröhlich matrix element squared |M_q|^2, Eq. (3) of [Author YYYY]
   M_sq = (e**2 * omega_LO) / (2 * eps0 * V) * (1/eps_inf - 1/eps_static) / q**2
   ```

## 9. Code Quality Checklist

```
[ ] Imports at top (stdlib, then numpy/scipy, then project)
[ ] np.random.seed() once at top for stochastic scripts
[ ] All paths via pathlib, relative to repo root
[ ] Physical constants from scipy.constants, not magic numbers
[ ] Unit comments on all physical quantities
[ ] Functions documented (NumPy docstring style)
[ ] Figures: both PDF and PNG, explicit dpi, tight_layout
[ ] Heavy outputs: saved as .npy/.npz with shape/axis comments
[ ] No float equality (use np.isclose)
[ ] No np.append in loops (pre-allocate)
```

## 10. Common Pitfalls

| Pitfall | Impact | Prevention |
|---------|--------|------------|
| Broadcasting over wrong axis | Silent wrong result | Always specify `axis=` |
| Magic-number physical constants | Wrong units/values, breaks portability | Use `scipy.constants` |
| Missing `bbox_inches='tight'` | Clipped labels in PDF | Always include |
| 3D array indexing confusion (k, branch, T) | Wrong scattering rate | Document axis order with comments |
| `np.arange` with float step | Unpredictable endpoint | Use `np.linspace` instead |
