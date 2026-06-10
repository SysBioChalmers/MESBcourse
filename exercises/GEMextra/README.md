# GEMextra — genome-scale model reconstruction

An **optional, open-ended** exercise: reconstruct a genome-scale metabolic model
(GEM) for the methylotrophic yeast *Hansenula / Ogataea polymorpha* from
homology to template yeasts, then gap-fill, simulate and curate it. There are no
questions — explore how the reconstruction works and, if you like, adapt it to
your own organism.

## Contents

| File | What it is |
|------|------------|
| [`gemExtra.ipynb`](gemExtra.ipynb) | **Python** translation using [`raven-python`](https://github.com/SysBioChalmers/raven-python) (the Python port of the RAVEN toolbox). Open in Colab with the badge in the notebook. |
| `reconstructionProtocol.m` | The original **MATLAB / RAVEN** script, for reference. |

## Background

The MATLAB script accompanies the book chapter *"Reconstruction of a
Genome-Scale Metabolic Model for Hansenula polymorpha Using RAVEN"* (Francisco
Zorrilla & Eduard Kerkhoven, in *Yeast Metabolic Engineering*). The section
numbers (3.1–3.9) in both files match the chapter.

The full MATLAB project — including the input data (template models, genome
FASTA files, biomass and lipid-curation tables) and the organism-specific helper
functions (`addLipidReactions`, `addSLIMEreactions`, `scaleLipids`) — lives in
the **[hanpo-GEM repository](https://github.com/SysBioChalmers/hanpo-GEM)**. The
Python notebook **downloads the data it needs from that repository**, so it is
self-contained (no large files are kept here).

## Notes on the Python version

- `raven-python` ports RAVEN's reconstruction functions (`run_blast`,
  `get_model_from_homology`, `connect_blocked_reactions`) onto cobrapy
  `Model` objects. It is alpha and installed from GitHub.
- The homology step needs **BLAST+** (the notebook installs `ncbi-blast+` and
  caches the hits, so it can be re-run without BLAST).
- The **SLIME lipid curation** (§3.5) is organism-specific; the notebook
  previews the template data and explains the approach — a full port of the
  three helper functions is left as an extension.
- Gap-filling is an MILP; with the default GLPK solver it is slow, so switch to
  `model.solver = "gurobi"` if available.
