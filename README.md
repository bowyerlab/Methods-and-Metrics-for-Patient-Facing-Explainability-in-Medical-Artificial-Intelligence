# Methods and Metrics for Patient-Facing Explainability in Medical Artificial Intelligence

Figure generation code for the systematic scoping review *Methods and Metrics for
Patient-Facing Explainability in Medical Artificial Intelligence: A Systematic
Scoping Review* (Kumarathas, Surty, Woyton, Lord and Bowyer, 2026).

The review follows PRISMA-ScR and maps studies of patient-facing explainable AI
(XAI) across clinical domain, explanation modality, patient involvement phase and
evaluation metrics. It introduces a **Trust Calibration Matrix**, which classifies
each study by subjective user trust against objectively verified model accuracy
(Concordant, Discordant, Unverified).

Protocol: [OSF SPFJ7](https://doi.org/10.17605/OSF.IO/SPFJ7)

## What is in this repository

| Path | Description |
| --- | --- |
| `generate_figures.ipynb` | One cell per figure, each writing a PDF/PNG pair into `figures/` |
| `data/appendix-d-extraction-matrix.xlsx` | Appendix D data extraction matrix — one row per included study, 24 variables |
| `data/appendix-d-extraction-matrix.csv` | Plain-text export, rewritten by the notebook on every run |
| `tables/` | Table 1, variable frequencies and the trust cross-tab, written as CSV |
| `pyproject.toml`, `uv.lock` | Dependency declaration and exact pinned resolution |

Every count in every figure is derived from the workbook at run time — it is the
single source of truth. Editing a study, or adding one, changes the figures with no
other edit required. The CSV is regenerated on each run, so it cannot drift; edit the
xlsx only. The copy archived on OSF
([EJSGZ](https://doi.org/10.17605/OSF.IO/EJSGZ)) is the version of record.

Categorical variables are recorded at their original granularity and grouped into the
Appendix B taxonomies in the Setup cell. Those mappings are explicit: a value with no
taxonomy entry raises rather than being dropped, so a new study with a new category
fails loudly instead of quietly vanishing from a figure.

The final cell writes the manuscript tables into `tables/` as CSV, from the same
derived frame the figures use. Appendix D is deliberately not among them: at one row per study
across 24 mostly free-text variables it is not a printable table, so the
supplement links it on OSF and the workbook in `data/` serves as the appendix itself.

The only numbers not derived from the workbook are the PRISMA screening tallies, which
describe records the matrix never held. Each exclusion reason is entered once in the
Figure 1 cell; the arm totals and the included count per arm are computed from them,
and the cell stops if they do not add up to the studies in the workbook.

Numbers quoted in the manuscript text should be taken from
`tables/summary-variable-frequencies.csv` and `tables/summary-trust-calibration.csv`
rather than copied into the code.

## Outputs

Cells run in manuscript order and every output is named for its label in the paper,
so no mapping table is needed.

| Output | Manuscript |
| --- | --- |
| `figures/figure-1-prisma-flow` | Figure 1 — PRISMA-ScR flow diagram |
| `figures/figure-2-landscape` | Figure 2 — clinical, methodological and technical landscape (A–E) |
| `figures/figure-3-evaluation-metrics` | Figure 3 — evaluation metrics by measurement domain |
| `figures/figure-4-trust-calibration-matrix` | Figure 4 — Trust Calibration Matrix |
| `figures/table-1-study-characteristics` | Table 1, rendered as an image |
| `figures/figure-s1-timeline-geography` | Figure S1 — publication timeline and geography |
| `figures/figure-s2-explanation-delivery` | Figure S2 — explanation modality, scope, delivery |
| `figures/unused-patient-involvement-phase` | not used; duplicates Figure 2, panel E |
| `tables/table-1-study-characteristics.csv` | Table 1, as text |
| `tables/summary-variable-frequencies.csv` | derived; n and % for every grouped variable |
| `tables/summary-trust-calibration.csv` | derived; Figure 4 as a cross-tab |

Figures are written as a vector PDF (Type 42 embedded fonts, editable text) and a
600 dpi PNG preview. Anything not carrying a manuscript label is prefixed `unused-`
or `summary-`.

BMJ typesets tables from text rather than reproducing images, so
`tables/table-1-study-characteristics.csv` is the version to submit; the rendered
PDF is a visual proof only.

Both directories are committed, so co-authors can see the current figures and table
values on GitHub without running anything. Output is byte-reproducible — PDFs carry
no creation timestamp — so a file changes in git only when the figure or the data
behind it changes.

## Reproducing the figures

Dependencies are managed with [uv](https://docs.astral.sh/uv/); `uv.lock` pins the
exact resolved set.

```sh
uv sync              # resolves deps from pyproject.toml into .venv
uv run jupyter lab   # run Setup, then any figure cell in generate_figures.ipynb
```

To execute headlessly:

```sh
uv run jupyter nbconvert --to notebook --execute --inplace generate_figures.ipynb
```

Without uv, `pip install matplotlib numpy pandas openpyxl` on Python 3.11+ is enough —
those are the only runtime dependencies.

Figures prefer Helvetica or Arial and fall back to DejaVu Sans, so glyph metrics may
differ slightly between machines.

## Citation

See [CITATION.cff](CITATION.cff). Please cite the paper rather than this
repository where a single citation is expected.

## Licence

Code is released under the MIT Licence — see [LICENSE](LICENSE). The manuscript
text and figures are subject to the publisher's terms and are not covered by this
licence.
