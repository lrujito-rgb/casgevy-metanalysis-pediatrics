Casgevy (exagamglogene autotemcel) SR-MA — Angle A (Paediatric 5–11 years)
![PROSPERO](https://img.shields.io/badge/PROSPERO-CRD4220261420579-blue)
![License: MIT](https://img.shields.io/badge/Code%20License-MIT-yellow.svg)
![Data License: CC BY 4.0](https://img.shields.io/badge/Data%20License-CC%20BY%204.0-lightgrey.svg)
Reproducible analysis repository for the systematic review and meta-analysis:
> **Efficacy and Safety of Exagamglogene Autotemcel (Casgevy) in Paediatric Patients with Sickle
> Cell Disease and Transfusion-Dependent Beta-Thalassemia: A Systematic Review and Meta-Analysis
> (Angle A: Ages 5–11 Years)**
> Rujito L, Siswandari W, Indriani V. Faculty of Medicine, Universitas Jenderal Soedirman,
> Indonesia.
PROSPERO registration: CRD4220261420579 (Protocol
v1.0, June 2026). This repository reports the pre-specified Angle A (5–11 year) staged
output of a broader registered protocol (population 5–17 years); a corresponding 12–17-year
("Angle B") output is planned as a separate future analysis.
---
⚠️ Protocol-compliance note (read before citing numbers from this repo)
The registered protocol specifies a minimum of k=3 independent studies before formal
random-effects meta-analytic pooling is undertaken (Protocol Field 17). At the time of this
analysis, neither stratum met that threshold:
Stratum	k	What is reported instead
Angle A (5–11y), TDT and SCD	1 each	Exact (Clopper–Pearson) 95% CI for the single study — not a meta-analytic pool
All-age contextual (TDT and SCD)	2 each	DerSimonian–Laird logit-transform point estimate, explicitly labelled exploratory, below the protocol's pooling threshold
This is disclosed prominently — including in the code output itself (see
`scripts/phase5_analysis.py` docstring and console output) — rather than silently treated as a
fully-powered meta-analysis. See manuscript Methods 2.8 and Discussion/Limitations item 1 for
full discussion.
---
Repository structure
```
casgevy-srma-angle-a/
├── data/
│   └── raw/
│       ├── Table_S1_full_text_disposition.csv   # all 66 full-text records: decision + reason
│       └── Table_S2_extracted_data.csv          # event/denominator data used in pooling
├── scripts/
│   ├── phase5_analysis.py        # main analysis: reads Table_S2 -> writes results/Phase5_pooled_results.csv
│   ├── make_figure1_prisma.py    # generates figures/Figure1_PRISMA_flow.png
│   └── make_figure2_forest.py    # generates figures/Figure2_forest_pooled_proportions.png (reads results/)
├── results/
│   └── Phase5_pooled_results.csv # output of phase5_analysis.py
├── figures/
│   ├── Figure1_PRISMA_flow.png
│   └── Figure2_forest_pooled_proportions.png
├── docs/
│   ├── R_reproduction_notes.md   # notes on cross-validating against the pre-specified R pipeline
│   └── PROSPERO_protocol_summary.md
├── requirements.txt
├── LICENSE                       # MIT (code)
├── CITATION.cff
└── README.md                     # this file
```
Reproducing the analysis
```bash
git clone https://github.com/<org-or-user>/casgevy-srma-angle-a.git
cd casgevy-srma-angle-a
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt

# Run the pooled-proportion analysis (reads data/raw/, writes results/)
python scripts/phase5_analysis.py

# Regenerate figures (reads results/, writes figures/)
python scripts/make_figure1_prisma.py
python scripts/make_figure2_forest.py
```
Expected console output for `phase5_analysis.py` (abbreviated):
```
TDT Angle A (TI12): 100.0% (95% CI: 63.1%-100.0%)   [k=1, exact CI]
SCD Angle A (VF12): 100.0% (95% CI: 63.1%-100.0%)   [k=1, exact CI]
TDT all-age (TI12): 92.0% (95% CI: 79.3%-97.1%)     [k=2, exploratory DL logit]
SCD all-age (VF12): 97.6% (95% CI: 84.5%-99.7%)     [k=2, exploratory DL logit]
```
Pre-specified R pipeline vs. this Python implementation
The protocol (Field 17) pre-specifies R (≥4.4.0) with the `meta` (v7.0+) and `metafor` (v4.6+)
packages as the primary analysis environment. `scripts/phase5_analysis.py` is a Python
(numpy/scipy/pandas) reimplementation of the same DerSimonian–Laird, logit-transform,
continuity-corrected methodology, used during manuscript drafting for cross-validation. See
`docs/R_reproduction_notes.md` for the equivalence-checking
status and instructions for reviewers who wish to reproduce results in R directly.
Data dictionary
See the "Data_Dictionary" sheet of `Supplementary_Data_Package.xlsx` (deposited alongside the
manuscript submission) for full column-level definitions. In brief:
Table_S1_full_text_disposition.csv — one row per full-text record screened (n=66);
`final_decision` categorizes each as `INCLUDED-quantitative`, `INCLUDED-narrative`, or one of
several `EXCLUDED-*` reasons (superseded/duplicate, registry entry, ineligible product,
out-of-scope, regulatory correspondence). See manuscript Methods 2.6 for the full
overlap-resolution decision rule applied to reach these determinations.
Table_S2_extracted_data.csv — one row per study/arm contributing to the quantitative
synthesis or its sensitivity analysis; `role_in_synthesis` distinguishes primary
quantitative-synthesis rows from the sensitivity-only (superseded) row.
Overlap-resolution highlight
A central methodological feature of this review — documented in full in the manuscript
(Methods 2.6, Discussion 4.6) — is the explicit, multi-step resolution of duplicate/overlapping
publications, which we found to be pervasive in this rapidly-iterating literature. The paediatric
primary cohort underlying this repository's Angle A estimates was independently reported at
least three times (ASH 2025 oral abstract → Pediatric Blood & Cancer poster → definitive
NEJM publication) before reaching peer-reviewed form; `Table_S1_full_text_disposition.csv`
documents the disposition of all three records and the evidentiary basis (trial-registry
matching, sample-size trajectory, and an identical fatal adverse-event narrative) used to
confirm they describe the same patients.
License
Code (`scripts/*.py`) is released under the MIT License.
Data (`data/`, `results/`) is released under
CC BY 4.0 — it summarizes information
extracted from publicly available published sources and does not contain patient-level or
otherwise identifiable data.
Citation
See `CITATION.cff`. If you reuse this extraction dataset or analysis code,
please cite the accompanying manuscript (citation to be finalized upon publication) and this
repository (DOI to be assigned upon Zenodo/GitHub release).
Contact
Lantip Rujito (Guarantor & Corresponding Author) — Faculty of Medicine, Universitas Jenderal
Soedirman, Indonesia. ORCID: 0000-0001-6595-3265
