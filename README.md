# Market Basket Analysis: Frequent Itemsets and Sequential Patterns

**CSCE 676 - Data Mining and Analysis**  
**Texas A&M University, Spring 2026**

## Project Overview

This project analyzes the Instacart Online Grocery Shopping Dataset to:
1. Discover frequent itemsets and association rules with FP-Growth (course technique)
2. Study how temporal segments (weekend/weekday, morning/evening) shape discovered rules
3. Mine sequential purchase patterns across user order histories with PrefixSpan (beyond-course technique)
4. Cluster users by purchase behavior with K-Means and mine cluster-specific rules
5. Compare insights from unordered vs. sequential pattern mining

## Dataset

**Instacart Online Grocery Shopping Dataset 2017**

- Source: [Kaggle Instacart Market Basket Analysis](https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis)
- Unit of analysis: orders (baskets) and order lines (order-product pairs)
- Scale: ~3.4M orders, ~32M order-product lines, ~50K products

## Checkpoints

| Checkpoint | Description | Due Date | Status |
|------------|-------------|----------|--------|
| 1 | Dataset Comparison, Selection, and EDA | Feb 12 | ✅ Complete |
| 2 | Research Question Formation | Mar 17 | ✅ Complete |
| 3 | ~~Study of Research Questions (Deep Dive)~~ | ~~Apr 2~~ | ❌ Canceled |
| 4 | Project Showcase | Apr 21 | 🔲 Pending |
| 5 | Final Deliverable | Apr 27 | 🔲 Pending |

## Repository Structure

```
├── README.md
├── CLAUDE.md
├── notebooks/
│   ├── project_checkpoint_1.ipynb   # Dataset selection and EDA
│   ├── project_checkpoint_2.ipynb   # Research question formation + FP-Growth/PrefixSpan pilots
│   └── project_final.ipynb          # Main deliverable: all four RQs end-to-end
├── data/
│   └── instacart/                   # Parquet versions of the Instacart CSVs (committed, ~118 MB)
└── .gitignore
```

## Methods

- **FP-Growth** (`pyfim` in the final notebook, `mlxtend` in the checkpoint pilot) for frequent itemsets and association rules
- **PrefixSpan** (`prefixspan`) for sequential pattern mining across user order histories
- **K-Means** (`scikit-learn`) for user clustering in RQ4, followed by cluster-specific FP-Growth

## License

Code: MIT License  
Data: See Instacart/Kaggle terms of use (do not redistribute)
