# MSCS 634 — Lab 6: Association Rule Mining with Apriori and FP-Growth

Course: **MSCS 634 — Advanced Big Data and Data Mining**  
Assignment: **Lab 6: Association Rule Mining with Apriori and FP-Growth**  
Student: **[Enter your name]**

## Purpose

Compare Apriori and FP-Growth on invoice-level product transactions, mine frequent product combinations, generate support/confidence/lift association rules, and visualize patterns and runtimes using Seaborn.

## Dataset

- **Source:** UCI Machine Learning Repository, [Online Retail](https://archive.ics.uci.edu/dataset/352/online+retail) (ID 352).
- **Direct download:** https://archive.ics.uci.edu/static/public/352/online+retail.zip
- **Citation:** Chen, D. (2015). *Online Retail* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5BW33
- The notebook downloads the official compressed archive automatically on first run, caches `data/Online Retail.xlsx`, and reads it with pandas. No manual CSV preparation is required.

## Setup and execution

1. Install **Python 3.10+**.
2. Open a terminal in the project root, create/activate a virtual environment if desired, then run:

   ```bash
   python -m pip install -r requirements.txt
   jupyter lab
   ```

3. Open `Lab6_Association_Rule_Mining.ipynb` and click **Run All** from top to bottom.
4. Ensure you have internet access for the **first** dataset download (approximately 23 MB compressed; parsing the workbook can take some time).
5. The notebook automatically writes charts into `screenshots/` and analysis tables into `outputs/`.

**Offline fallback:** Manually download the UCI archive, extract `Online Retail.xlsx`, and save it as `data/Online Retail.xlsx` before running the notebook.

## Methodology

1. **Cleaning:** remove duplicates, null identifiers/description, canceled invoices, non-positive sales, and selected non-product fees; retain UK invoices.
2. **Basket construction:** randomly select up to 6,000 invoices (`RANDOM_STATE=42`) and 65 most common product descriptions in that sample; create a Boolean invoice-by-product matrix. Invoices not containing any retained product are dropped from the basket. **Support is calculated using these retained invoices as the denominator.**
3. **Apriori:** mine frequent itemsets at `min_support=0.015`, maximum 3 products per itemset.
4. **FP-Growth:** use the *identical matrix and parameters*, and compare the sets/supports of mined itemsets.
5. **Association rules:** filter to `confidence >= 0.35`, inspect rules with `lift >= 1.20`, and plot confidence against lift with support shown by color and size.
6. **Timing:** benchmark each algorithm 3 times and compare median runtime. Runtime conclusions are environment-specific.

All thresholds and sample-size settings are declared near the top of the notebook and can be adjusted for experimentation. An overly restrictive threshold may produce no multi-item sets, in which case lower it and rerun the notebook.

## Project structure

```text
MSCS_634_Lab_6/
├── Lab6_Association_Rule_Mining.ipynb
├── README.md
├── requirements.txt
├── .gitignore
├── screenshots/                       # generated plots after running notebook
│   ├── 01_top_items.png
│   ├── 02_item_cooccurrence_heatmap.png
│   ├── 03_apriori_itemsets.png
│   ├── 04_fpgrowth_itemsets.png
│   ├── 05_association_rules_scatter.png  # only if rules are generated
│   └── 06_runtime_comparison.png
├── outputs/                           # generated tables and narrative
│   ├── apriori_frequent_itemsets.csv
│   ├── fpgrowth_frequent_itemsets.csv
│   ├── apriori_association_rules.csv    # only if rules are generated
│   ├── fpgrowth_association_rules.csv   # only if rules are generated
│   ├── algorithm_comparison.csv
│   └── analysis_summary.txt
└── data/                              # auto downloaded; ignored by Git
    └── Online Retail.xlsx
```

## Suggested GitHub submission

After running the notebook, reviewing the computed results, and editing your name:

```bash
git init
git add Lab6_Association_Rule_Mining.ipynb README.md requirements.txt .gitignore screenshots outputs
git commit -m "Complete Lab 6 association rule mining"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/MSCS_634_Lab_6.git
git push -u origin main
```

Create your empty public GitHub repository before running `git push`, and replace `YOUR_USERNAME` with your own handle. **Submit the repository URL to the course LMS.**

## Rubric coverage

| Criterion | Weight | Evidence |
|---|---:|---|
| Data Preparation | 20% | Data download, quality checks, cleaning, encoding, top-product plot, co-occurrence heatmap |
| Apriori Algorithm | 20% | Apriori itemsets, support scores, plot |
| FP-Growth Algorithm | 20% | FP-Growth itemsets, support scores, plot |
| Association Rule Analysis | 20% | Apriori + FP-Growth rules, support/confidence/lift, interpretation, scatterplot |
| Comparative Analysis | 10% | Itemset equality/support check, timing and chart, challenges/limitations |
| Code Implementation | 10% | Functions, reproducibility, descriptive code, exported artifacts |

## Interpretation and limitations

Rules describe **co-occurrence**, not causality. Sampling to a UK market, a capped invoice subset, a restricted product vocabulary, and a maximum itemset size makes the lab manageable but limits generalizability. Do not claim FP-Growth or Apriori is universally faster; consult the timed comparison cell and report observed results. The repository intentionally does **not** include the UCI Excel dataset.

## Sources

- UCI Online Retail: https://archive.ics.uci.edu/dataset/352/online+retail
- mlxtend Apriori: https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/apriori/
- mlxtend FP-Growth: https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/fpgrowth/
- mlxtend Association Rules: https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/association_rules/
