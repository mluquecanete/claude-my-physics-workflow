---
paths:
  - "Slides/**/*.tex"
  - "papers/**/*.tex"
  - "scripts/**/*.py"
  - "notebooks/**/*.ipynb"
---

# Project Knowledge Base: Phonon-Limited Transport in Bi₂Se₃

<!-- Claude reads this before creating or modifying any content. -->

## Notation Registry

| Rule | Convention | Example | Anti-Pattern |
|------|-----------|---------|-------------|
| Electron wavevector | Bold **k** in prose, `\mathbf{k}` in LaTeX | $\mathbf{k} = (k_x, k_y)$ | plain `k` for 2D vector |
| Phonon wavevector | Bold **q** in prose, `\mathbf{q}` in LaTeX | $\mathbf{q}$ | using `k` for phonon |
| Scattering rate | $\Gamma_{\mathbf{k}}$ or $1/\tau$ | $\Gamma = 2\pi/\hbar \sum_{\mathbf{q}} |M|^2 \delta(...)$ | $\gamma$ (conflicts with other uses) |
| Fermi energy | $E_F$ | $E_F = \hbar v_F k_F$ | $\epsilon_F$ |
| Dirac Hamiltonian | $H_{SS}$ | $H_{SS} = \hbar v_F (\boldsymbol{\sigma} \times \mathbf{k}) \cdot \hat{z}$ | $H_0$, $H_{surf}$ |
| Phonon frequency | $\omega_{\mathbf{q},s}$ (branch $s$) | $\omega_{LA}$, $\omega_{LO}$ | $\nu$, $f$ |
| Dielectric function | $\varepsilon(q, \omega)$ | $\varepsilon_\infty$ (high-freq), $\varepsilon_0$ (static) | $\epsilon$, $\kappa$ |
| Matrix element squared | $|M_{\mathbf{k},\mathbf{k}'}|^2$ | Fermi's golden rule input | $|V|^2$ without indices |
| Mobility | $\mu$ | $\mu = e \tau / m^*$ | $\mu_e$ (reserved for chemical potential) |
| Chemical potential | $\mu_c$ or $E_F$ | keep consistent per document | $\mu$ (clashes with mobility) |

## Symbol Reference

| Symbol | Meaning | Introduced |
|--------|---------|------------|
| $v_F$ | Fermi velocity of surface state (~5×10⁵ m/s for Bi₂Se₃) | Surface state Hamiltonian |
| $k_F$ | Fermi wavevector | Fermi surface definition |
| $\mathbf{k}$ | 2D electron wavevector | Hamiltonian |
| $\mathbf{q}$ | 2D phonon wavevector | Phonon coupling |
| $\Gamma$ | Scattering rate (1/τ) | Boltzmann equation |
| $\omega_{LO}$ | Longitudinal optical phonon frequency | Fröhlich interaction |
| $\omega_{LA}$ | Longitudinal acoustic phonon branch | Deformation potential |
| $\varepsilon_\infty$ | High-frequency dielectric constant | Dielectric-continuum model |
| $\varepsilon_s$ | Static dielectric constant | Dielectric-continuum model |
| $D$ | Deformation potential constant | Acoustic phonon coupling |
| $g_s, g_v$ | Spin and valley degeneracy | Density of states |
| $\rho$ | Mass density of the material | Phonon coupling |
| $c_s$ | Speed of sound | Acoustic phonon dispersion |
| $\ell$ | Film thickness (confined geometry) | Confined phonon model |
| $\theta$ | Scattering angle (between **k** and **k'**) | Boltzmann integral |

## Paper / Chapter Progression

| # | Title / Topic | Core Question | Key Method | Key Output |
|---|--------------|--------------|-----------|------------|
| 1 | Surface state phonon coupling | How does electron-phonon coupling differ for TSS vs bulk? | Dielectric-continuum model | Matrix element |M|² |
| 2 | Acoustic phonon scattering | What is the T-dependence of mobility from acoustic phonons? | Deformation potential + Boltzmann | μ(T) power law |
| 3 | Optical phonon scattering | How do optical phonons limit mobility at high T? | Fröhlich interaction | Γ(T) crossover |
| 4 | Confined phonons | Does phonon confinement in thin films change transport? | Dielectric-continuum confined modes | Γ vs film thickness |

## Computational Applications

| Application | Script | Dataset/Model | Purpose |
|------------|--------|--------------|---------|
| Scattering rates | `scripts/scattering_rates.py` | Dielectric-continuum model | Compute Γ(k,T) |
| Mobility vs T | `scripts/mobility.py` | Boltzmann transport | μ(T) curves |
| Phonon dispersion | `scripts/phonon_dispersion.py` | Bulk + confined modes | ω(q) for Bi₂Se₃ |

## Physical Parameters (Bi₂Se₃)

| Parameter | Value | Source |
|-----------|-------|--------|
| Fermi velocity $v_F$ | ~5×10⁵ m/s | ARPES |
| Static dielectric $\varepsilon_s$ | ~113 | Bulk measurement |
| High-freq dielectric $\varepsilon_\infty$ | ~29 | |
| LO phonon energy $\hbar\omega_{LO}$ | ~17 meV | Raman |
| Mass density $\rho$ | 7690 kg/m³ | |
| Speed of sound $c_s$ | ~2900 m/s | |

## Anti-Patterns (Don't Do This)

| Anti-Pattern | What Happened | Correction |
|-------------|---------------|-----------|
| Using `k` for both electron and phonon wavevectors | Symbol clash | `k` = electron, `q` = phonon always |
| Omitting (1 − cos θ) factor in Boltzmann integral | Overestimates backscattering contribution | Include explicitly |
| Using bulk dielectric in surface scattering | Ignores image-charge effects | Use dielectric-continuum interface result |
| Magic-number physical constants in scripts | Wrong values, not reproducible | Use `scipy.constants` |

## Python/NumPy Pitfalls

| Pitfall | Impact | Fix |
|---------|--------|-----|
| Broadcasting over wrong axis in 3D arrays (k, branch, T) | Wrong Γ(T) result | Document axis order, use named variables |
| `np.arange` with float step near zone boundary | Missing or double-counting q-points | Use `np.linspace` |
| Missing `axis=` in `np.sum` for angular integral | Sum over wrong dimension | Always explicit |
| Not checking wavevector grid convergence | Inaccurate scattering rates | Add convergence test at bottom of script |
