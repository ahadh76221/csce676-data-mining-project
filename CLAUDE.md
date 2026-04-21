# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a data mining semester project for CSCE 676 at Texas A&M University (Spring 2026). The final deliverable notebook is titled **"Shopper Archetypes: Testing the Universal-Pattern Assumption in Grocery Recommendation"** and analyzes the **Instacart Online Grocery Shopping Dataset 2017** (~3.4M orders, ~32M order-product lines, ~50K products) to:

1. Discover frequent itemsets and association rules with FP-Growth (`mlxtend` + `pyfim`)
2. Study how temporal segments (weekend/weekday, time-of-day) reshape discovered rules
3. Mine sequential purchase patterns across user order histories with PrefixSpan (`prefixspan`)
4. Cluster users with K-Means / GMM / NMF on user×aisle + behavioral features and mine archetype-specific rules

The final notebook runs all four RQs end-to-end, then closes with a Cross-RQ Synthesis, Conclusions, and a Unit Tests section.

## Repository Structure

All work lives in `notebooks/`. There is no `src/` or test suite yet.

- `notebooks/project_checkpoint_1.ipynb` — Dataset selection and EDA (Instacart chosen over alternatives)
- `notebooks/project_checkpoint_2.ipynb` — Research questions, FP-Growth pilot (mlxtend), PrefixSpan pilot, unit tests
- `notebooks/project_final.ipynb` — Main deliverable: all four RQs end-to-end (FP-Growth via `mlxtend` + `pyfim`, PrefixSpan, K-Means / GMM / NMF clustering)
- `video/` — 2-minute pitch deliverable: `Grocery Recommendations Are Not Universal.mp4`, `shopper_archetypes_pitch.pptx`, `video_script.md`
- `data/instacart/` — Parquet versions of the Instacart CSVs (committed, ~118 MB total). Raw CSVs are gitignored; the parquet files are produced with downcast dtypes and zstd-19 compression so the full dataset fits under GitHub's 100 MB per-file limit.

## Running the Notebooks

```bash
# Launch Jupyter
jupyter notebook notebooks/

# Or run a notebook non-interactively
jupyter nbconvert --to notebook --execute notebooks/project_checkpoint_2.ipynb
```

## Key Dependencies

- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `scikit-learn` (`KMeans`, `GaussianMixture`, and `NMF` are all load-bearing for RQ4's algorithm sweep)
- `pyarrow` — required to read the committed parquet files
- `mlxtend` — `fpgrowth` + `association_rules` for rule generation, small/segmented runs (RQ2 temporal slices, RQ4 per-archetype rules), and the FP-Growth pilot in `project_checkpoint_2.ipynb`
- `pyfim` — C-backed FP-Growth (~100× faster than mlxtend) used for the full 3.2M-basket department sweeps in RQ1/RQ3; auto-installed by the final notebook if missing
- `prefixspan` — Sequential pattern mining for RQ3; auto-installed by the final notebook if missing
- `tqdm`, `joblib` — progress bars and parallelization in the final notebook

Manual install (optional — `pyfim` and `prefixspan` bootstrap themselves at runtime via `subprocess.check_call`):
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn pyarrow mlxtend pyfim prefixspan tqdm joblib
```

## Research Questions

- **RQ1**: How does varying minimum support threshold affect quality, diversity, and interpretability of association rules?
- **RQ2**: How do temporal segments (weekend vs. weekday, morning vs. evening) affect discovered rules?
- **RQ3**: Do sequential purchase patterns reveal temporal structure missed by unordered frequent itemsets?
- **RQ4**: If we cluster users by what they buy, do different shopper types surface cluster-exclusive rules?

## Data Loading Convention

Notebooks load parquet from `../data/instacart/` relative to `notebooks/`. Main files: `orders.parquet`, `order_products__prior.parquet`, `order_products__train.parquet`, `products.parquet`, `aisles.parquet`, `departments.parquet`. Use `pd.read_parquet(...)` — requires `pyarrow`.

## Milestones

- **Checkpoint 4 — Project Showcase**: Apr 21, 2026 — slides (`video/shopper_archetypes_pitch.pptx`), 2-minute pitch video (`video/Grocery Recommendations Are Not Universal.mp4`), and the final notebook are all committed
- **Checkpoint 5 — Final Deliverable**: Apr 27, 2026 — remaining
