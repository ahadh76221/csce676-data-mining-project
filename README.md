# Shopper Archetypes: Testing the Universal-Pattern Assumption in Grocery Recommendation

**CSCE 676 - Data Mining and Analysis**  
**Texas A&M University, Spring 2026**

## Project Overview

This project analyzes the Instacart Online Grocery Shopping Dataset to ask whether a single set of association rules actually generalizes across shoppers, or whether context (time, order history, shopper identity) changes which rules hold. The final notebook (`notebooks/project_final.ipynb`) answers four research questions end-to-end:

1. Discover frequent itemsets and association rules with FP-Growth (course technique) — calibrate the minimum-support sweet spot
2. Study how temporal segments (weekend/weekday, time-of-day) shape the discovered rules
3. Mine sequential purchase patterns across user order histories with PrefixSpan (beyond-course technique), with a shuffle baseline to test whether any temporal structure is actually recoverable
4. Cluster users by purchase behavior (K-Means / GMM / NMF sweep on user × aisle + behavioral features) and mine archetype-specific rules; compare insights from unordered vs. sequential vs. archetype-segmented mining

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
| 4 | Project Showcase (slides + 2-min pitch video + final notebook) | Apr 21 | ✅ Complete |
| 5 | Final Deliverable | Apr 27 | 🔲 Pending |

## Repository Structure

```
├── README.md
├── CLAUDE.md
├── notebooks/
│   ├── project_checkpoint_1.ipynb                       # Dataset selection and EDA
│   ├── project_checkpoint_2.ipynb                       # Research question formation + FP-Growth/PrefixSpan pilots
│   └── project_final.ipynb                              # Main deliverable: all four RQs end-to-end
├── video/
│   ├── Grocery Recommendations Are Not Universal.mp4    # 2-minute pitch
│   ├── shopper_archetypes_pitch.pptx                    # Showcase slide deck
│   └── video_script.md                                  # Narration script
├── data/
│   └── instacart/                                       # Parquet versions of the Instacart CSVs (committed, ~118 MB; raw CSVs gitignored)
└── .gitignore
```

## Methods

- **FP-Growth** — `mlxtend.fpgrowth` + `association_rules` for rule generation and small/segmented runs (RQ2 temporal slices, RQ4 per-archetype rules), and a `pyfim` C-backed wrapper (`fpgrowth_fast`) for the full 3.2M-basket department sweeps in RQ1/RQ3 where mlxtend's pure-Python implementation is too slow
- **PrefixSpan** (`prefixspan`) for sequential pattern mining across user order histories, paired with a shuffle-baseline test for temporal significance (RQ3)
- **K-Means / GMM / NMF** (`scikit-learn`) for user clustering in RQ4: baseline K-Means on a user × department matrix, then an algorithm sweep on an upgraded user × aisle + behavioral feature matrix; **NMF** is selected for the final soft archetypes, followed by archetype-specific FP-Growth

## Headline Findings

- **Support sweet spot ≈ 0.01** — balances rule count against interpretability across department- and product-level mining
- **Temporal shifts are modest** — roughly 4–15% rule divergence across time-of-day slices and ~44% across weekend/weekday, concentrated in discretionary categories
- **Sequential mining reveals re-purchase flows** that unordered itemsets miss, but at department granularity those patterns fail a strict shuffle test — the sequence signal is weaker than it first appears
- **Shopper archetypes dominate** — NMF surfaces ~3 soft archetypes (fresh-produce, light-pantry, staples/household) with ~90% rule divergence across them, making *who shops* the strongest context lens over *when* or *in what order*

## License

Code: MIT License  
Data: See Instacart/Kaggle terms of use (do not redistribute)
