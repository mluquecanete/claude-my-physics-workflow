# CLAUDE.MD -- PhD Research Workflow

**Project:** Phonon-Limited Transport of Topological Surface States in Bi₂Se₃
**Institution:** King's College London
**Branch:** main

---

## Core Principles

- **Plan first** -- enter plan mode before non-trivial tasks; save plans to `quality_reports/plans/`
- **Verify after** -- compile/run and confirm output at the end of every task
- **Single source of truth** -- Beamer `.tex` is authoritative for slides; Python scripts are authoritative for numerics
- **Quality gates** -- nothing ships below 80/100
- **[LEARN] tags** -- when corrected, save `[LEARN:category] wrong → right` to [MEMORY.md](MEMORY.md)

Cross-session context lives in [MEMORY.md](MEMORY.md); past plans, specs, and session logs are in [quality_reports/](quality_reports/).

---

## Folder Structure

```
phonon-transport/
├── CLAUDE.md                    # This file
├── .claude/                     # Rules, skills, agents, hooks
├── Bibliography_base.bib        # Centralized bibliography
├── Figures/                     # Figures and images (PDF + PNG)
├── Preambles/header.tex         # LaTeX headers (Beamer + article)
├── Slides/                      # Beamer .tex files (talks, posters)
├── papers/                      # Manuscript drafts (.tex)
├── notebooks/                   # Jupyter notebooks (exploration)
├── scripts/                     # Python scripts (production numerics)
├── output/                      # Computed arrays (.npy/.npz), figures
├── quality_reports/             # Plans, session logs, merge reports
├── explorations/                # Research sandbox
├── templates/                   # Session log, quality report templates
└── master_supporting_docs/      # Reference papers and existing slides
```

---

## Commands

```bash
# LaTeX (3-pass, XeLaTeX only)
cd Slides && TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
BIBINPUTS=..:$BIBINPUTS bibtex file
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex

# Run Python analysis script
python scripts/script_name.py

# Run Jupyter notebook (headless)
jupyter nbconvert --to notebook --execute notebooks/notebook.ipynb

# Quality score
python scripts/quality_score.py Slides/file.tex
```

---

## Quality Thresholds (advisory)

| Score | Checkpoint | Meaning |
|-------|------|---------|
| 80 | Commit | Good enough to save |
| 90 | PR | Ready for deployment |
| 95 | Excellence | Aspirational |

Enforced by `/commit` (halts + asks for override); not enforced by a git pre-commit hook.

---

## Skills Quick Reference

| Command | What It Does |
|---------|-------------|
| `/compile-latex [file]` | 3-pass XeLaTeX + bibtex |
| `/new-diagram [snippet] [output.tex]` | Scaffold a TikZ diagram from the gallery |
| `/extract-tikz [file]` | TikZ → PDF → SVG |
| `/proofread [file]` | Grammar/typo/overflow review |
| `/visual-audit [file]` | Slide layout audit |
| `/pedagogy-review [file]` | Narrative, notation, pacing review |
| `/slide-excellence [file]` | Combined multi-agent review |
| `/validate-bib` | Cross-reference citations |
| `/devils-advocate` | Challenge slide design |
| `/create-lecture` | Full lecture/talk creation |
| `/commit [msg]` | Stage, commit, PR, merge |
| `/lit-review [topic]` | Literature search + synthesis |
| `/research-ideation [topic]` | Research questions + strategies |
| `/interview-me [topic]` | Interactive research interview |
| `/review-paper [file]` | Manuscript review (single-pass / `--adversarial` / `--peer <journal>`) |
| `/respond-to-referees [report] [manuscript]` | R&R cross-reference + response draft |
| `/seven-pass-review` | Seven-pass adversarial manuscript review |
| `/verify-claims [file]` | Chain-of-Verification fact-check |
| `/learn [skill-name]` | Extract discovery into persistent skill |
| `/context-status` | Show session health + context usage |
| `/deep-audit` | Repository-wide consistency audit |
| `/permission-check` | Diagnose permission layers |

**Not applicable (tools not installed):** `/review-r`, `/data-analysis`, `/qa-quarto`, `/translate-to-quarto`, `/deploy`

---

## Beamer Custom Environments

| Environment | Effect | Use Case |
| --- | --- | --- |
| `keybox` | Gold background box | Key results, takeaways |
| `definitionbox[Title]` | Blue-bordered titled box | Formal definitions |

---

## Current Project State

| Output | File | Topic |
| --- | --- | --- |
| Talk 1 | `Slides/Talk01_PhononTransport.tex` | Phonon-limited mobility in Bi₂Se₃ surface states |
| Paper draft | `papers/paper_phonon_transport.tex` | Main manuscript |
| Numerics | `scripts/scattering_rates.py` | Scattering-rate calculations |
