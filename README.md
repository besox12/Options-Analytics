# Options & Derivatives Analytics

**A standalone, zero-dependency derivative analytics workbench built entirely in vanilla HTML/JS/CSS.**

Live demo → [**lucabesomi.github.io/options-analytics**](https://lucabesomi.github.io/options-analytics)

![Dark Theme](https://img.shields.io/badge/theme-dark%20%2F%20light-1a1a2e?style=flat-square)
![Zero Dependencies](https://img.shields.io/badge/dependencies-zero-22d68a?style=flat-square)
![Strategies](https://img.shields.io/badge/strategies-66-5eead4?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-818cf8?style=flat-square)

---

## Overview

A comprehensive options pricing, Greeks analysis, and strategy visualization tool designed for quantitative finance education, energy trading preparation, and derivatives research. Everything runs client-side in the browser — no server, no API keys, no installation required.

### Key Features

**Pricing & Greeks**
- Black-Scholes-Merton analytical pricing with 5 Greeks (Δ, Γ, ν, Θ, ρ)
- Greeks evolution over time — visualize gamma spikes and vega decay toward expiry
- 2D Greeks surface heatmap (spot × time)
- P&L Taylor decomposition (Δ + ½Γ + Θ + ν attribution)

**Strategies**
- 66 pre-built strategies across 13 categories (Income, Spreads, Condors, Butterflies, Ladders, Ratios, Synthetics, and more)
- Dynamic multi-strategy comparison with up to 10 overlaid payoff curves
- Strategy descriptions on hover
- One-click strategy loading with live metrics

**Volatility**
- Newton-Raphson IV solver with convergence diagnostics
- Multi-strike IV extraction with empirical smile overlay
- Parametric vol smile: σ(K) = σ_ATM + skew·(K−S)/S + convexity·((K−S)/S)²
- Auto-fit smile to empirical IV points (least-squares regression)
- Full smile propagation to all BS-dependent calculations

**Monte Carlo**
- GBM path simulation with Box-Muller normal variates (100–5,000 paths)
- Probability cone fan chart (P5/P25/P50/P75/P95 percentile bands)
- 14 risk metrics including VaR, CVaR, Sharpe, Sortino, Calmar, Jarque-Bera normality test

**Exotics Lab**
- Path-dependent pricing via Monte Carlo (Asian, Lookback, Barrier options)
- Greeks via bump-and-reprice (finite differences on MC price)
- Payoff distribution histograms and sample path visualization
- Comparison to vanilla BS closed-form prices

**Risk & Hedging**
- Scenario & breakeven analysis with spot × vol shock matrix
- Capital efficiency metrics (margin estimate, return on capital)
- Hedge advisor: Δ, Γ, ν neutralization via linear system solver
- Vol sensitivity overlay (±5% σ curves on payoff)

**Market Data**
- 15 commodity/asset reference profiles (Power, Gas, Carbon, Oil, Metals, FX, Equity)
- Auto-scales all instruments to market-realistic levels
- PDF report export with SVG payoff diagram, metrics, and Greeks

---

## Quick Start

**Option 1 — Open directly**
Download `index.html` and double-click to open in any modern browser.

**Option 2 — GitHub Pages**
Fork this repo → Settings → Pages → Deploy from main branch.

**Option 3 — Local server**
```bash
# Python
python3 -m http.server 8000

# Node
npx serve .
```

---

## Architecture

```
index.html          ← Single-file application (~2,400 lines, ~190KB)
├── CSS             ← Custom properties, dark/light theme, responsive layout
├── HTML            ← 16 tab panels, instrument panel, strategy dropdown
└── JavaScript
    ├── Black-Scholes    ← bsPrice(), bsGreeks(), normCDF(), normPDF()
    ├── Payoff Engine     ← calcPayoff() for 11 instrument types
    ├── Portfolio         ← portfolioValueAtT(), getPortfolioGreeks()
    ├── Monte Carlo       ← GBM simulation, Box-Muller, percentile fan
    ├── Vol Surface       ← getSmileVol(), getEffectiveVol(), IV solver
    ├── Hedge Advisor     ← 2×2 linear system for Γ+ν, Δ via futures
    ├── Strategies        ← 66 pre-built, categorized, with descriptions
    ├── Canvas Rendering  ← devicePixelRatio, pan/zoom, hover tooltips
    └── Export            ← PNG screenshot, PDF report, JSON save/load
```

**Design choices:**
- Zero dependencies — works offline, no CDN, no build step
- Canvas-based rendering with retina support
- Pure analytical Greeks (no finite differences for vanilla options)
- Vol smile propagation via `getEffectiveVol()` interceptor pattern

---

## Screenshots

| Payoff + Greeks | Scenario Matrix | Monte Carlo Fan |
|---|---|---|
| Collar with live Δ/Γ/ν/Θ annotation | Spot × Vol shock heatmap | P5–P95 percentile bands |

| Strategy Compare | Exotics Lab | Vol Smile + IV |
|---|---|---|
| Multi-strategy overlay (up to 10) | MC-priced barriers with bump Greeks | Empirical dots + parametric fit |

---

## Mathematical Foundation

**Black-Scholes-Merton**

$$C = S_0 N(d_1) - K e^{-rT} N(d_2)$$

$$d_1 = \frac{\ln(S/K) + (r + \sigma^2/2)T}{\sigma\sqrt{T}}, \quad d_2 = d_1 - \sigma\sqrt{T}$$

**Monte Carlo (GBM)**

$$S(t + \Delta t) = S(t) \cdot \exp\left[\left(r - \frac{\sigma^2}{2}\right)\Delta t + \sigma\sqrt{\Delta t} \cdot Z\right], \quad Z \sim \mathcal{N}(0,1)$$

**Hedge Advisor (Γ+ν neutralization)**

$$\begin{bmatrix} \Gamma_1 & \Gamma_2 \\ \nu_1 & \nu_2 \end{bmatrix} \begin{bmatrix} n_1 \\ n_2 \end{bmatrix} = \begin{bmatrix} -\Gamma_{\text{port}} \\ -\nu_{\text{port}} \end{bmatrix}, \quad n_{\text{fut}} = -(\Delta_{\text{port}} + n_1\Delta_1 + n_2\Delta_2)$$

See the **About** tab in the application for complete methodology documentation.

---

## Author

**Luca Besomi**
MSc Banking & Finance — University of Zurich
Minor in Quantitative Finance (ETH Zürich coursework)

---

## License

MIT License — free to use, modify, and distribute.
