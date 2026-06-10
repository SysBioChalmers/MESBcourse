# Changes to the GEM2 GECKO MATLAB exercise

This documents the edits applied to the original GEM2 GECKO 3 ecModel exercise
during review.

The exercise was checked against **GECKO 3.2.5** (released 2026-03-26). Every
GECKO/RAVEN function used across the five stages was verified to still exist
with a compatible signature — **no API breakages**.

> The review was done at source level. The exercise was **not executed**: it
> requires MATLAB + RAVEN + GECKO + Gurobi, plus the online services
> UniProt / BRENDA / PubChem and Docker (for DLKcat).

## Functional fixes

- **`ecRhto/ecRhtoAdapter.m` — portable project path.** `obj.params.path` was
  hardcoded to a personal machine path
  (`C:\Users\eduard\OneDrive - Chalmers\…\Day2\ecRhto`). Replaced with a
  self-locating default so the project works wherever the repository is cloned:

  ```matlab
  obj.params.path = fileparts(mfilename('fullpath'));
  ```

- **Stage 4 — wrong condition described.** A comment said *"the first condition
  is exponential growth on glucose"* while the code uses `condition = 5`. The
  proteomics data (`abs_proteomics.txt`) is ordered Xexp (1), Xnlim (2),
  Aexp (3), Anlim (4), **Gexp (5)**, Gnlim (6) — so condition 5 *is*
  glucose-exponential, and condition 1 is xylose. The comment was corrected to
  describe the fifth condition.

- **Stage 4 — `saveEcModel` filename.** `saveEcModel(ecModelProt,'ecRhto_stage4')`
  was given an explicit `.yml` extension (`'ecRhto_stage4.yml'`), matching the
  other stages. Functionally identical (RAVEN's writer appends `.yml`), but now
  consistent.

## Typo fixes

| Stage | Before | After |
|------:|--------|-------|
| 1 | generaed | generated |
| 1 | your advised | you're advised |
| 1 | in constrast to | in contrast to |
| 1 | two different lenghts | two different lengths |
| 1 | for for the purpose | for the purpose |
| 2 | the whold documentation | the whole documentation |
| 2 | these contraints | these constraints |
| 3 | effect on growht | effect on growth |
| 3 | newly fitter sigma-factor | newly fitted sigma-factor |
| 3 | avaiable on … BRENDA | available on … BRENDA |
| 4 | want to explorer | want to explore |
| 4 | changes have occured | changes have occurred |
| 4 | concentration contraints | concentration constraints |
| 4 | calcualted by | calculated by |
| 4 | reaching the inteneded growth rate | reaching the intended growth rate |
| 4 | when runnign constrainEnzConcs | when running constrainEnzConcs |

## File renaming

- `Day2_stage1.mlx` … `Day2_stage5.mlx` → `gem2_stage1.mlx` … `gem2_stage5.mlx`
- `Day2Answers.pdf` → `gem2Answers.pdf`
- Folder references inside the stages were updated from `Day2` to the actual
  folder name `GEM2_gecko`, and the stage **titles** were changed from
  "Day 2: ecModels in GECKO …" to "GEM2: ecModels in GECKO …".

## Repository size trim (77 MB → 46 MB)

`PreparedFiles/` held copies byte-identical to `ecRhto/` (the stage models the
code loads from `ecRhto/models/`, and the data files in `ecRhto/data/`) plus
regenerable outputs. Those were removed. `PreparedFiles/` keeps only the files
the exercise tells students to copy from it: `rhto.xml`, `abs_proteomics.txt`,
`fluxData.tsv`, `paxDB.tsv`, `DLKcat.tsv`, `minFlux_maxFlux.mat`. `ecRhto/` is
unchanged and fully functional.
