# Automated Equity Valuation Model
### Discounted Cash Flow, Sensitivity Analysis, and Investment Memo

A single Google Colab notebook that takes any public-company **ticker** and produces a full, defensible equity valuation: a five-year unlevered DCF, a CAPM/WACC discount rate, dual terminal-value methods, scenario and sensitivity analysis, a reverse DCF, presentation-quality charts, and a written investment memo with a final rating.

Built in Python with `pandas`, `numpy`, `matplotlib`, `seaborn`, and `yfinance`. Example company is **Apple (AAPL)**, but the ticker is a one-line change.

---

## Why this exists

A DCF is not a magic number generator. It is a structured way to make assumptions explicit and stress-test them, which is exactly how equity research, investment banking, private equity, and buy-side analysts use it. This project is built to be **honest** rather than tuned to a flattering answer: for mega-cap compounders like Apple, a plain perpetuity DCF often reads "overvalued," and the notebook explains why instead of hiding it.

## Features

- **Live data ingestion** from Yahoo Finance with a fuzzy row-lookup helper that survives `yfinance` label drift, plus retries and a bundled offline fallback so the notebook always runs.
- **Unlevered DCF engine** — `UFCF = EBIT*(1-tax) + D&A - CapEx - ΔNWC`, discounted at a CAPM-based WACC.
- **Dual terminal value** — Gordon Growth perpetuity *and* an EV/EBITDA exit multiple, triangulated the way a real desk does (roughly 70% of value sits in the terminal assumption).
- **Bull / base / bear scenarios** and **two-variable sensitivity tables** (WACC × terminal growth, revenue growth × EBIT margin, WACC × exit multiple).
- **Reverse DCF** — a bisection solver that backs out the constant revenue growth the market is currently pricing in.
- **Visualizations** — a six-panel dashboard and an investment-banking-style valuation football field.
- **Investment memo** — auto-generated, with data-driven risks and a final UNDERVALUED / FAIRLY VALUED / OVERVALUED rating.

## How to run

1. Open [Google Colab](https://colab.research.google.com/).
2. `File > Upload notebook` and select `Automated_Equity_Valuation_Model.ipynb`.
3. In **Cell 3 (Control Panel)**, set `TICKER` to whatever you want (default `"AAPL"`).
4. `Runtime > Run all`.

Everything else is automatic. To run fully offline on the bundled snapshot, set `FORCE_FALLBACK = True` in Cell 3.

> **Note on data:** Yahoo Finance is free but rate-limited and its statement labels change between versions. If a live pull fails, the notebook prints a clear banner and continues on bundled fallback data so it never breaks during a demo. Re-run after a minute for live figures.

## Control panel (what you can change)

All in Cell 3: ticker, forecast horizon, and an `USER_OVERRIDES` dictionary for revenue growth, EBIT margin, tax rate, D&A %, CapEx %, NWC %, risk-free rate, equity risk premium, cost of debt, terminal growth, and exit multiple. Leave overrides empty to let the model derive sensible defaults from the company's own history. Scenario deltas (bull/base/bear) are also editable there.

## Modeling philosophy

- **History informs the forward view, it does not dictate it.** Trailing CAGR is computed and shown, but the base-case growth is an explicit, floored analyst choice.
- **No fake precision.** Proxies (e.g., book value of debt for market value) are labeled as proxies.
- **Guardrails.** Terminal value is protected against `WACC ≤ terminal growth`; CapEx sign is normalized; missing fields degrade gracefully.
- **Triangulation over a single point.** The football field shows the full range across methods and scenarios; the memo discusses the range, not one decimal.

## Roadmap / possible upgrades

SEC EDGAR integration for audit-grade inputs, comparable-company and precedent-transaction modules, Monte Carlo simulation, two/three-stage growth, an automated PDF memo, a Streamlit dashboard, a multi-company screener, and an LLM-drafted narrative memo.

## Tech

Python · pandas · numpy · matplotlib · seaborn · yfinance · Google Colab

## Disclaimer

For educational and portfolio purposes only. This is **not investment advice**, and every output depends entirely on user-supplied assumptions.
