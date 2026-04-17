# Workflow Quick Reference

**Model:** Contractor (you direct, Claude orchestrates)

---

## The Loop

```
Your instruction
    ↓
[PLAN] (if multi-file or unclear) → Show plan → Your approval
    ↓
[EXECUTE] Implement, verify, done
    ↓
[REPORT] Summary + what's ready
    ↓
Repeat
```

---

## I Ask You When

- **Method forks:** "Approach A (dielectric-continuum) vs Approach B (ab initio phonons). Which?"
- **Numerical ambiguity:** "Spec unclear on wavevector grid density. Use N=200?"
- **Convergence edge case:** "Scattering integral just missed tolerance. Investigate?"
- **Scope question:** "Also refactor phonon branch selection while here, or focus on current task?"

---

## I Just Execute When

- Code fix is obvious (bug, pattern application)
- Verification (tolerance checks, compilation, numerical convergence)
- Documentation (logs, commits)
- Plotting (per established standards below)
- Deployment (after you approve, I ship automatically)

---

## Quality Gates (No Exceptions)

| Score | Action |
|-------|--------|
| >= 80 | Ready to commit |
| < 80  | Fix blocking issues |

---

## Non-Negotiables

- **Paths:** always relative to repository root (`pathlib.Path(__file__).parent`)
- **Reproducibility:** `np.random.seed(YYYYMMDD)` once at top of stochastic scripts
- **Figures:** `savefig(path, dpi=300, bbox_inches='tight')`; save as `.pdf` (LaTeX) + `.png` (preview)
- **Heavy outputs:** save computed arrays as `.npy` or `.npz` to `output/`; scripts and notebooks load pre-computed data
- **Tolerance:** numerical convergence checked to `rtol=1e-6` by default; document when looser tolerance is justified
- **Units:** all physical quantities carry explicit unit comments

---

## Preferences

**Visual:** matplotlib with explicit `figsize`; axis labels with units; physical constants in legends
**Reporting:** concise bullets; detailed derivations on request
**Session logs:** always (post-plan, incremental, end-of-session)
**Replication:** strict — flag near-misses; document all convergence parameters

---

## Exploration Mode

For experimental/sandbox work, use the **Fast-Track** workflow:
- Work in `explorations/` or `notebooks/` folder
- 60/100 quality threshold (vs. 80/100 for production)
- No plan needed — just a research value check (2 min)
- See `.claude/rules/exploration-fast-track.md`

---

## Next Step

You provide task → I plan (if needed) → Your approval → Execute → Done.
