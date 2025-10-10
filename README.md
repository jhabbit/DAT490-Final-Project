# DAT 490 — Crypto & Equities

This repo contains the notebooks and code used to analyze time‑varying relationships between cryptocurrencies (BTC, ETH) and U.S. equities (S&P 500, Dow), with optional gold. It matches the structure and filenames referenced in the final LaTeX manuscript.

---

## Repo layout

```
your-repo/
  notebooks/
    00_data_cleaning_and_connectedness.ipynb
    01_data_eda.ipynb
    02_modeling.ipynb
  data/
    merged_returns.csv
  figures/
  tables/
  README.md
  requirements.txt
  .gitignore
```

---

## Reproduce figures & tables

**Google Colab**
1. Open `notebooks/01_data_eda.ipynb` in Colab.
2. Upload `data/merged_returns.csv` to `/content/data/` (or mount Drive so the path is `/content/drive/.../data/merged_returns.csv`).
3. Run all cells. Exports will be written to:
   - `figures/figure1_volatility.png` (Figure 1 in the paper)
   - `figures/rolling_corr_btc_spx.png`
   - `tables/summary_returns.csv`

**Local (Jupyter)**
```bash
pip install -r requirements.txt
jupyter lab  # or jupyter notebook
# Open notebooks/01_data_eda.ipynb and Run All
```

> If your notebook uses Stooq CSVs instead of `merged_returns.csv`, place them in `data/` and update the loader cell accordingly (`btc_usd_d.csv`, `eth_usd_d.csv`, `^spx_d.csv`, `^dji_d.csv`).

---

## Modeling notebook

Open `notebooks/02_modeling.ipynb` to fit:
- **GARCH(1,1)** per asset (BTC, ETH, S&P, Dow) → saves `figures/*_garch_sigma.png`, `tables/garch_params.csv`
- **VAR(p)** (AIC, Granger, IRFs) → saves `figures/var_impulse_responses.png`, `tables/var_summary.txt`
- **Wavelets (DWT, db4)** → saves `figures/wavelet_energy.png`, `tables/wavelet_energy.csv`

---

## Data

You can work with **either**:
- A single merged returns file: `data/merged_returns.csv` (must include `Date`, and columns for BTC & S&P returns such as `BTC_Close_Return`, `SPX_Close_Return`; if BTC returns are missing but you have `BTC_Close`, the EDA cell computes them).
- **Stooq** daily close CSVs: `btc_usd_d.csv`, `eth_usd_d.csv`, `^spx_d.csv`, `^dji_d.csv` (then compute log‑returns in the notebook).

If you cannot share data publicly, include a short `data/README.md` explaining how to obtain it.

---

## Environment

Install once:
```bash
pip install -r requirements.txt
```

Libraries: `pandas`, `numpy`, `matplotlib`, `statsmodels`, `arch`, `PyWavelets`, `seaborn`.

---

## Troubleshooting

- **File not found**: Ensure `data/merged_returns.csv` (or Stooq CSVs) exist at the expected path. In Colab, create `/content/data/` and upload there.
- **Different column names**: In the EDA export cell, set `BTC_RET_COL` and `SPX_RET_COL` to match your column names.
- **Figures not appearing**: Make sure the notebook created `figures/` and `tables/` (the setup cell does this), and re‑run the final export cell.
