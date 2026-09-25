# Crash Metrics and Protective Put Hedge

This project stress-tests a multi-asset portfolio against hypothetical S&P 500 crashes and examines how a protective put changes its losses. 

---

## ⚙️ Working Conditions

- **Initial capital:** $1,000,000.
- **Assets:** Nvidia, Apple, Microsoft, JPMorgan, Bank of America, Exxon Mobil, Shell, gold, oil, and natural gas.
- **Initial allocation** 
- **S&P 500 shocks:** −5%, −10%, −15%, −20%, −30%, and −40%.
- **Protective put:** 1,000 units on the S&P 500, initially six months to expiry, with a strike at 90% of the initial underlying level.
- **Pricing assumptions:** 3% risk-free rate and 40% constant volatility. 

---

## 🎯 Goal

Estimate losses under severe market shocks and compare the portfolio's one-day mark-to-market returns with and without put protection, while keeping the total initial budget fixed at $1,000,000.

---

## 🧠 Methodology

### 1. Data Collection

The notebook retrieves historical market prices with `yfinance`, calculates daily returns, and identifies S&P 500 crash days for the asset-sensitivity analysis.


### 2. Crash Beta

The notebook estimates each asset's crash beta using returns observed on selected negative S&P 500 days. For a hypothetical S&P 500 shock, the stressed return of asset `i` is estimated as:

$$
R_{i,\mathrm{stress}} = \beta_{i,\mathrm{crash}} \times R_{\mathrm{market,shock}}
$$

Here, `R_i,stress` is the estimated asset return, `beta_i,crash` is its crash beta, and `R_market,shock` is the hypothetical S&P 500 return. This is a simplified historical sensitivity, not a prediction.

For example, with a crash beta of 1.5 and an S&P 500 shock of −10%, the asset’s estimated stressed return is 1.5 × (−10%) = −15%.

### 3. Portfolio Stress Test

For each shock, the notebook multiplies every asset's stressed return by its initial position value and sums the resulting dollar P&Ls. Dividing by initial capital gives the unhedged portfolio return.

### 4. Adding 1,000 Puts

A protective put on the S&P 500 proxy is priced using the Black–Scholes model. Its strike is set at 90% of the initial spot price `K = 0.9 × S₀`. After each hypothetical one-day shock, the put is repriced with approximately `0.5 − 1/252` years remaining to expiry. The put position’s P&L equals the number of put units multiplied by the change in price per unit.
The premium is financed by reducing all ten asset positions proportionally. Their relative weights stay at the initial allocation **within the asset sleeve**; their weights in the total portfolio become smaller. The hedged P&L is the resized asset P&L plus the put P&L. 

---

## 📊 Results

The saved notebook run prices the put at approximately **$45.09 per unit**. Buying 1,000 units costs approximately **$45,090**, leaving **$954,910** for the assets. The illustrative results are:

| S&P 500 Shock | Unhedged P&L | Hedged P&L | Unhedged Return | Hedged Return |
|---:|---:|---:|---:|---:|
| −5% | −$54,083 | −$39,755 | −5.41% | −3.98% |
| −10% | −$108,167 | −$76,600 | −10.82% | −7.66% |
| −15% | −$162,250 | −$110,484 | −16.23% | −11.05% |
| −20% | −$216,334 | −$141,189 | −21.63% | −14.12% |
| −30% | −$324,501 | −$192,749 | −32.45% | −19.27% |
| −40% | −$432,667 | −$232,378 | −43.27% | −23.24% |

The comparison reflects **both** gains on the put and the smaller investment in assets used to fund it. Results depend on the retrieved prices and modelling assumptions. The shocks are hypothetical; the model holds volatility constant and omits transaction costs, liquidity effects, and listed-option contract multipliers.

---

## 📈 Visualisations

### Portfolio Stress Curve

The chart below compares the unhedged portfolio P&L with the portfolio P&L after adding 1,000 synthetic protective put units.

<img width="620" height="455" alt="image" src="https://github.com/user-attachments/assets/1aeeddfa-8459-49e0-8802-aeffc96a72cc" />


### Protective Put Benefit

This chart shows the improvement in portfolio return produced by the protective put. The benefit increases as the S&P 500 shock becomes more severe, illustrating the put's nonlinear downside protection.

<img width="777" height="470" alt="image" src="https://github.com/user-attachments/assets/40fea64d-14e9-4622-9aad-c3ea56d6fa32" />

---
## ⚙️ Prerequisites

- Python 3 and Jupyter Notebook.
- Python packages used for the analysis include `numpy`, `pandas`, `yfinance`, `scipy`, and `matplotlib`. Check the notebook for any additional imports.

---

## 📌 Installation

Create and activate a Python virtual environment if desired. If your repository contains a `requirements.txt` matching the notebook's imports, install the dependencies with:

```bash
pip install -r requirements.txt
```

---

## 💻 Usage

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open and run:

```text
Crash_Metrics.ipynb
```
