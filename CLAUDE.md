# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a data mining semester project for CSCE 676 at Texas A&M University (Spring 2026). The project analyzes the **Instacart Online Grocery Shopping Dataset 2017** (~3.4M orders, ~32M order-product lines, ~50K products) to:

1. Discover frequent itemsets and association rules using FP-Growth (`pyfim`)
2. Mine sequential purchase patterns across user order histories using PrefixSpan (`prefixspan`)
3. Cluster users by purchase behavior (K-Means) and mine cluster-specific rules
4. Compare insights from unordered vs. sequential pattern mining

## Repository Structure

All work lives in `notebooks/`. There is no `src/` or test suite yet.

- `notebooks/project_checkpoint_1.ipynb` — Dataset selection and EDA (Instacart chosen over alternatives)
- `notebooks/project_checkpoint_2.ipynb` — Research questions, FP-Growth pilot (mlxtend), PrefixSpan pilot, unit tests
- `notebooks/project_final.ipynb` — Main deliverable: all four RQs end-to-end (FP-Growth via PyFIM, PrefixSpan, K-Means clustering)
- `data/instacart/` — Parquet versions of the Instacart CSVs (committed, ~118 MB total). Raw CSVs are gitignored; the parquet files are produced with downcast dtypes and zstd-19 compression so the full dataset fits under GitHub's 100 MB per-file limit.

## Running the Notebooks

```bash
# Launch Jupyter
jupyter notebook notebooks/

# Or run a notebook non-interactively
jupyter nbconvert --to notebook --execute notebooks/project_checkpoint_2.ipynb
```

## Key Dependencies

- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `scikit-learn` (K-Means is load-bearing for RQ4)
- `pyarrow` — required to read the committed parquet files
- `pyfim` — FP-Growth and association rule mining in the final notebook (~100× faster than mlxtend)
- `mlxtend` — still used by `project_checkpoint_2.ipynb` for the FP-Growth pilot
- `prefixspan` — Sequential pattern mining (auto-installed by the notebook if missing)

Install all at once:
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn pyarrow pyfim mlxtend prefixspan
```

## Research Questions

- **RQ1**: How does varying minimum support threshold affect quality, diversity, and interpretability of association rules?
- **RQ2**: How do temporal segments (weekend vs. weekday, morning vs. evening) affect discovered rules?
- **RQ3**: Do sequential purchase patterns reveal temporal structure missed by unordered frequent itemsets?
- **RQ4**: If we cluster users by what they buy, do different shopper types surface cluster-exclusive rules?

## Data Loading Convention

Notebooks load parquet from `../data/instacart/` relative to `notebooks/`. Main files: `orders.parquet`, `order_products__prior.parquet`, `order_products__train.parquet`, `products.parquet`, `aisles.parquet`, `departments.parquet`. Use `pd.read_parquet(...)` — requires `pyarrow`.

## Upcoming Milestones

- **Checkpoint 4 — Project Showcase**: Apr 21, 2026
- **Checkpoint 5 — Final Deliverable**: Apr 27, 2026
