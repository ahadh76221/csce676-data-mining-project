# Shopper Archetypes: Testing the Universal-Pattern Assumption in Grocery Recommendation

**CSCE 676 — Data Mining and Analysis · Texas A&M University · Spring 2026**
**Author:** Ahad Hussain

> 👉 **Start here:** [`notebooks/main_notebook.ipynb`](notebooks/main_notebook.ipynb) — the curated end-to-end deliverable with all four research questions, results, and conclusions.

> 🎥 **2-minute pitch video:** https://www.youtube.com/watch?v=tsGcIjvFSsA

## 📖 Overview

Every major online grocery platform — Instacart, DoorDash, Walmart — answers the same question every time a customer adds something to their cart: *what are you going to add next?* The industry answers this by mining millions of baskets for "what people buy together" patterns and serving those patterns to everyone.

Baked into that approach is an assumption: *that the patterns are universal* — that the rules mined from all shoppers together are the rules that should drive everyone's experience. This project tests that assumption from four angles using the **Instacart Online Grocery Shopping Dataset 2017** (~3.4M orders, ~32M order-product lines, ~50K products), and finds that *who shops* is a far stronger context lens than *when* or *in what order* — undermining the case for one universal recommender.

## 🔍 Research Questions

- **RQ1 — Calibration.** How does varying the minimum support threshold affect the quality, diversity, and interpretability of association rules?
- **RQ2 — Temporal context.** How do temporal segments (weekend vs. weekday, morning vs. evening) reshape the discovered rules?
- **RQ3 — Sequential structure.** Do sequential purchase patterns reveal temporal structure that unordered frequent itemsets miss — or does the signal disappear under a shuffle baseline?
- **RQ4 — Shopper archetypes.** If we cluster users by what they buy, do different shopper types surface cluster-exclusive rules that universal mining washes out?

## 🎯 Headline findings

- **Support sweet spot ≈ 0.01** — balances rule count against interpretability across both department- and product-level mining.
- **Temporal shifts are modest** — roughly 4–15% rule divergence across time-of-day slices and ~44% across weekend/weekday, concentrated in discretionary categories.
- **Sequential mining surfaces re-purchase flows** that unordered itemsets miss, but at department granularity those patterns fail a strict shuffle test — the sequence signal is weaker than it first looks.
- **Shopper archetypes dominate.** NMF surfaces ~3 soft archetypes (fresh-produce, light-pantry, staples/household) with ~90% rule divergence across them. The universal-pattern assumption breaks here, not at the temporal layer.

Full analysis, figures, and unit tests are in [`notebooks/main_notebook.ipynb`](notebooks/main_notebook.ipynb).

## 📊 Data

**Instacart Online Grocery Shopping Dataset 2017**

- **Source:** [Kaggle — Instacart Market Basket Analysis](https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis)
- **Scale:** ~3.4M orders · ~32M order-product lines · ~50K products · 21 departments · 134 aisles
- **Unit of analysis:** orders (baskets) and order-product pairs
- **What's committed:** parquet versions of the six Instacart CSVs in [`data/instacart/`](data/instacart/) (~118 MB total). The parquet files are produced with downcast dtypes and **zstd-19** compression so the full dataset fits under GitHub's 100 MB per-file limit. Raw CSVs are gitignored.
- **Loading:** `pd.read_parquet("data/instacart/orders.parquet")` (etc.) — requires `pyarrow`.

## 🔁 How to reproduce

This project was developed and last run end-to-end in **Google Colab** on **Python 3.12.13**. To reproduce:

1. **Clone the repo**
   ```bash
   git clone https://github.com/ahadh76221/csce676-data-mining-project.git
   cd csce676-data-mining-project
   ```
2. **Install dependencies** from the pinned `requirements.txt`:
   ```bash
   pip install -r requirements.txt
   ```
   (Or upload the notebook to Colab — most dependencies are pre-installed there. The notebook itself bootstraps `pyfim` and `prefixspan` at runtime if they are missing.)
3. **Open the main notebook:** [`notebooks/main_notebook.ipynb`](notebooks/main_notebook.ipynb)
4. **Run all cells top-to-bottom.** End-to-end runtime is roughly **30–45 minutes** on a Colab CPU runtime. The committed parquet under `data/instacart/` is loaded automatically; no Kaggle download is required.

The two checkpoint notebooks under [`checkpoints/`](checkpoints/) are committed as historical artifacts of the project's progression — not the primary grading target.

## 📦 Key dependencies

| Package | Version | Used for |
|---|---|---|
| python | 3.12.13 | Colab runtime at last execution |
| pandas | 2.2.2 | Tabular data, group-by/pivot for basket matrices |
| numpy | 2.0.2 | Numeric arrays, basket math |
| scikit-learn | 1.6.1 | `KMeans`, `GaussianMixture`, `NMF` for RQ4 clustering |
| mlxtend | 0.23.4 | `fpgrowth` + `association_rules` for segmented runs (RQ2, RQ4) |
| pyfim | 6.28 | C-backed FP-Growth (~100× faster than mlxtend) for full 3.2M-basket sweeps (RQ1, RQ3) |
| prefixspan | 0.5.2 | Sequential pattern mining (RQ3) |
| pyarrow | 18.1.0 | Reads the committed parquet |
| matplotlib | 3.10.0 | Figures |
| seaborn | 0.13.2 | Heatmaps + categorical plots |
| joblib | 1.5.3 | Parallelization in the segmented sweeps |
| tqdm | 4.67.3 | Progress bars |

The full pinned dependency list (670 packages from the Colab session) lives in [`requirements.txt`](requirements.txt).

## 🗂 Repository structure

```
.
├── notebooks/
│   └── main_notebook.ipynb              # 👈 Start here — the curated final deliverable
├── checkpoints/
│   ├── checkpoint_1.ipynb               # Dataset comparison + EDA (Instacart chosen)
│   └── checkpoint_2.ipynb               # RQ formation + FP-Growth/PrefixSpan pilots
├── data/
│   └── instacart/                       # Parquet versions of Instacart CSVs (committed, ~118 MB)
├── video/
│   ├── Grocery Recommendations Are Not Universal.mp4   # 2-minute pitch
│   ├── shopper_archetypes_pitch.pptx    # Showcase slide deck
│   └── video_script.md                  # Pitch narration script
├── requirements.txt                     # Pinned versions from the Colab run
├── README.md
├── CLAUDE.md                            # Project notes for AI-pair-programming sessions
└── .gitignore
```

## 📜 License

Code: MIT License. Data: see [Instacart / Kaggle terms of use](https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis) — please do not redistribute the original CSVs.
