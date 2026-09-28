# pe-atlas: Attribution-based cross-cohort mining of preeclampsia single-cell programs

Code and per-figure source data for:

> **Attribution scores from single-cell foundation models reveal reproducible, subtype-specific preeclampsia programs across cohorts**

## Repository layout

```
├── code/            47 numbered scripts (Python 3.12 + Bash), in pipeline order:
│   ├── 00–09    environment, verified downloads (ranged/SHA256), GSE173193 preprocessing
│   ├── 10–14    cohort harvest (NCBI eutils), batch download/QC, format repairs
│   ├── 15–18    attribution sanity checks, whole-cell consensus mining (v1)
│   ├── 20–25    annotation, type-stratified mining (v2/v2.1), head-to-head benchmark
│   ├── 26–38    all figures, Enrichr annotation, subtype analyses, scFoundation
│   │            sensitivity, bulk validation, supplementary-table export
│   └── 39–42    publication-grade figure iterations (QC-driven rebuilds)
├── source_data/     per-figure CSVs backing every main figure (fig1–fig6)
└── LICENSE          MIT
```

## Key results (see paper)

1. **Replication benchmark (Fig 2):** attribution signatures replicate across cohorts 3–6× better than expression signatures in all four lineages (24/24 pairs, Wilcoxon p = 1.2×10⁻⁷; leave-one-cohort-out stable).
2. **Consensus programs (Figs 3–4):** myeloid complement–innate immunity module (KEGG complement cascade adj.p = 9.8×10⁻⁸), FLT1-containing trophoblast module, endothelial complement infiltration, stromal ECM remodeling.
3. **Subtype structure (Fig 5):** complement module two-face dichotomy (EOPE/severe +0.21..+0.25 vs LOPE/mixed −0.31..−0.32), directionally corroborated in bulk cohorts.
4. **TF rescue (Fig 6):** low-expression regulators (GATA2, NR2F2) systematically rescued in all cohorts (2.3–8.8×).

## Reproduction

```bash
pip install scanpy sc-signature torch captum requests openpyxl

# 1) cohorts: harvest + download + QC (needs internet; ~4 GB)
bash code/11_batch_download.sh
python code/12_preprocess_batch.py

# 2) attribution (single GPU, ~90 s per cohort on one RTX 4090; ~35 min total)
python code/13_phase1_attribution.py

# 3) annotation, stratified mining, head-to-head
python code/20_annotate_all.py
python code/22_phase3_v2.py
python code/25_headtohead.py

# 4) figures, enrichment, subtype, bulk validation, SI tables
python code/26_fig2_headtohead.py    # … through 38; final figure QC iterations 39–42
```

All downstream analyses are CPU-only. Data transfer helpers (`autodl_*.py`, `local_fetch_chunks.sh`, `16/21/23/33_*.py`) are optional infrastructure used for verified downloads from a rented GPU relay; server addresses are placeholders.

## Data

All input datasets are public (NCBI GEO): GSE173193, GSE290578, GSE298602, GSE298119, GSE282038, GSE329173, GSE240922 (single-cell); GSE25906, GSE306864 (bulk). Attribution helper files: Zenodo 17903196. scFoundation weights: HuggingFace `genbio-ai/scFoundation`.

## Citation

[Preprint/manuscript reference upon posting]
