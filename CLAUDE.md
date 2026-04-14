# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a data mining semester project for CSCE 676 at Texas A&M University (Spring 2026). The project analyzes the **Instacart Online Grocery Shopping Dataset 2017** (~3.4M orders, ~32M order-product lines, ~50K products) to:

1. Discover frequent itemsets and association rules using FP-Growth (`mlxtend`)
2. Mine sequential purchase patterns across user order histories using PrefixSpan (`prefixspan`)
3. Compare insights from unordered vs. sequential pattern mining

## Repository Structure

All work lives in `notebooks/`. There is no `src/` or test suite yet.

- `notebooks/project_checkpoint_1.ipynb` — Dataset selection and EDA (Instacart chosen over alternatives)
- `notebooks/project_checkpoint_2.ipynb` — Research questions, FP-Growth pilot, PrefixSpan pilot, unit tests
- `data/instacart/` — Parquet versions of the Instacart CSVs (committed, ~118 MB total). Raw CSVs are gitignored; the parquet files are produced with downcast dtypes and zstd-19 compression so the full dataset fits under GitHub's 100 MB per-file limit.

## Running the Notebooks

```bash
# Launch Jupyter
jupyter notebook notebooks/

# Or run a notebook non-interactively
jupyter nbconvert --to notebook --execute notebooks/project_checkpoint_2.ipynb
```

## Key Dependencies

- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `scikit-learn`
- `pyarrow` — required to read the committed parquet files
- `mlxtend` — FP-Growth and association rule mining
- `prefixspan` — Sequential pattern mining (auto-installed by the notebook if missing)

Install all at once:
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn pyarrow mlxtend prefixspan
```

## Research Questions

- **RQ1**: How does varying minimum support threshold affect quality, diversity, and interpretability of association rules?
- **RQ2**: How do temporal segments (weekend vs. weekday, morning vs. afternoon) affect discovered rules?
- **RQ3**: Do sequential purchase patterns reveal temporal structure missed by unordered frequent itemsets?

## Data Loading Convention

Notebooks load parquet from `../data/instacart/` relative to `notebooks/`. Main files: `orders.parquet`, `order_products__prior.parquet`, `products.parquet`, `aisles.parquet`, `departments.parquet`. Use `pd.read_parquet(...)` — requires `pyarrow`.

## Upcoming Milestones

- **Checkpoint 4 — Project Showcase**: Apr 21, 2026
- **Checkpoint 5 — Final Deliverable**: Apr 27, 2026
