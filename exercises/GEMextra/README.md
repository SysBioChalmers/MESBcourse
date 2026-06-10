# GEMextra — genome-scale model reconstruction

An **optional, open-ended** exercise: reconstruct a genome-scale metabolic model
(GEM) for the methylotrophic yeast *Hansenula / Ogataea polymorpha* from
homology to template yeasts, then gap-fill, simulate and curate it. There are no
questions — explore how the reconstruction works and, if you like, adapt it to
your own organism.

## Contents

| Path | What it is |
|------|------------|
| [`gemExtra.ipynb`](gemExtra.ipynb) | **Python** translation using [`raven-python`](https://github.com/SysBioChalmers/raven-python). Open in Colab with the badge in the notebook. |
| [`code/reconstructionProtocol.m`](code/reconstructionProtocol.m) | The original **MATLAB / RAVEN** script. Run it from the `code/` folder. |
| `code/lipidMetabolism/`, `code/curation/` | Organism-specific helper functions the script needs (`addLipidReactions`, `addSLIMEreactions`, `scaleLipids`, `cleanupModel`). |
| `code/newCommit.m`, `code/newRelease.m` | RAVEN/GitHub bookkeeping helpers (the "save" steps; need a git repo + `model/` folder). |
| `data/` | Inputs: template models (`templateModels/`), genome proteomes (`genomes/*.faa`), biomass (`biomass/`) and lipid-curation tables (`reconstruction/`). |

## Background

The MATLAB script accompanies the book chapter *"Reconstruction of a
Genome-Scale Metabolic Model for Hansenula polymorpha Using RAVEN"* (Francisco
Zorrilla & Eduard Kerkhoven, in *Yeast Metabolic Engineering*). The section
numbers (3.1–3.9) in both the script and the notebook match the chapter.

The MATLAB version here is self-contained: the helper functions and the input
data it uses are included under `code/` and `data/`. Only the files referenced by
the protocol are kept — the unused `hanpo.fna` / `hanpo.gbff` genome files (~27 MB)
and `BLASTgapfillGenes.txt` were omitted. The complete project (those files, the
finished model, etc.) is the
**[hanpo-GEM repository](https://github.com/SysBioChalmers/hanpo-GEM)**.

> You also need the **RAVEN Toolbox** (with BLAST+, libSBML and a solver such as
> Gurobi) installed and on the MATLAB path — see
> [RAVEN installation](https://github.com/SysBioChalmers/RAVEN/wiki/Installation).
> The script was written for RAVEN 2.6.2; most functions are unchanged in current
> RAVEN, though a few signatures (e.g. `getModelFromHomology`) have gained options.

The **Python notebook downloads the same data from the hanpo-GEM repository** at
run time, so it works standalone on Colab without the `data/` folder here.

## Notes on the Python version

- `raven-python` ports RAVEN's reconstruction functions (`run_blast`,
  `get_model_from_homology`, `connect_blocked_reactions`) onto cobrapy
  `Model` objects. It is alpha and installed from GitHub.
- The homology step needs **BLAST+** (the notebook installs `ncbi-blast+` and
  caches the hits, so it can be re-run without BLAST).
- The **SLIME lipid curation** (§3.5) is organism-specific; the notebook
  previews the template data and explains the approach — a full port of the
  helper functions is left as an extension. (The MATLAB helpers are in `code/`.)
- Gap-filling is an MILP; with the default GLPK solver it is slow, so switch to
  `model.solver = "gurobi"` if available.
