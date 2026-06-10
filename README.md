# Time Series Analysis & Financial Econometrics

**Comprehensive econometric analysis of financial volatility, multivariate dynamics, and macroeconomic shock propagation**

**Author:** Tommaso Zeri  
**Institution:** Alma Mater Studiorum, Università di Bologna  
**Department:** Statistical Sciences, Second Cycle Degree in Quantitative Finance  
**Supervisor:** Emanuele Bacchiocchi  
**Date:** 2025

---

## 📋 Project Overview

This project provides a **complete econometric framework** for analyzing financial time series using advanced volatility models and macroeconomic systems. It covers three interconnected research themes:

1. **Univariate Volatility Modelling** — Time-varying conditional variance (GARCH family)
2. **Multivariate Volatility Modelling** — Dynamic correlations among financial assets (DCC-GARCH)
3. **Macroeconomic Shock Analysis** — Structural identification and impulse responses (VAR/SVAR)

**Distinctive Features:**
- Excel-integrated notebooks for practitioner accessibility
- Publication-ready academic report (27 pages, 29 figures)
- Reproducible Python implementation (arch, statsmodels)
- Complete diagnostic testing and forecasting
- Structural identification via Cholesky decomposition

---

## 📊 Three Research Components

### 🔹 Component 1: Univariate Volatility Modelling

**Dataset:** Single financial asset, 6,580 daily observations (2000–2024)

**Key Findings:**
- **Best Model:** GARCH(1,1) with Student-t distribution (BIC: 58,829.9)
- **Drift:** μ = 0.5372% daily (significant, p = 0.025)
- **Reactivity:** α = 0.1119 (moderate shock sensitivity)
- **Persistence:** β = 0.7883 (high memory)
- **Stationarity:** α + β = 0.9002 < 1 (mean-reverting volatility)
- **Tail Risk:** ν = 7.11 (fat-tailed Student-t vs Gaussian)

**Key Results:**
- Volatility clustering clearly visible in returns
- Absence of leverage effect (GJR-GARCH γ = -0.0199, p = 0.259)
- Symmetric shock response (positive/negative news equally impact volatility)
- News Impact Curve: Parabolic shape centered on zero

**Forecasting:**
- 1-step ahead (t+1): σ²ₜ₊₁ = 19.74%
- 5-day horizon shows mean reversion toward long-run variance (~20.7%)
- Volatility term structure: Positive slope → current volatility below long-run average

**Grid Search Results:**
- 72 specifications tested (GARCH, EGARCH, GJR, APARCH × Normal, Student-t, Skew-t)
- Top 5 models ranked by BIC with comparative diagnostics
- All show persistence (α + β) ≈ 0.90, confirming volatility memory

---

### 🔹 Component 2: Multivariate Volatility Modelling

**Dataset:** 3 financial assets (Price1, Price2, Price3), 2,264 observations

**Specification:**
- **Mean Equation:** ARMA(1,1) per asset (removes serial correlation)
- **Variance Component:** GJR-GARCH(1,1,1) with heterogeneity:
  - Price1: α = 0.0412, β = 0.7211 (balanced)
  - Price2: α = 0.2580, β = 0.0857 (high sensitivity, low persistence)
  - Price3: α = 0.0443, β = 0.8379 (stable, persistent)
- **Correlation Dynamics:** DCC(1,1) with a = 0.00718, b = 0.98700

**Dynamic Correlations:**
| Pair | Min | Max | Mean | Std. Dev |
|------|-----|-----|------|----------|
| Price1–Price2 | 0.1451 | 0.5673 | 0.3102 | 0.0803 |
| Price1–Price3 | 0.2257 | 0.5554 | 0.3626 | 0.0642 |
| Price2–Price3 | 0.1244 | 0.4600 | 0.2586 | 0.0634 |

**Key Insights:**
- **High Persistence:** a + b = 0.99418 → Correlation shocks are very long-lasting
- **Strongest Link:** Price1–Price3 (ρ̄ = 0.3626) → Systemic co-movement
- **Slow Dynamics:** DCC follows smooth trend (not noisy) due to high β
- **Validity of DCC:** CCC (constant correlation) would underestimate systemic risk during stress periods

**Why DCC over BEKK?**
- **Parsimony:** BEKK suffers from curse of dimensionality (parameters ∝ N²)
- **Interpretability:** DCC separates variances (Dₜ) from correlations (Rₜ)
- **Modularity:** Fit best univariate model per asset, then combine via correlations
- **Computational:** Sequential estimation → better convergence

**Matrix Representation:**
```
Σₜ = DₜRₜDₜ

where:
Dₜ = diag(σ₁,ₜ, σ₂,ₜ, σ₃,ₜ)  [individual volatilities]
Rₜ = dynamic correlation matrix with:
  Qₜ = (1−a−b)R̄ + a(uₜ₋₁u'ₜ₋₁) + bQₜ₋₁
```

**Forecasting (5 days):**
- DCC forecasts show convergence to mean correlations
- Correlations become increasingly stable beyond 10-day horizon
- Portfolio risk (joint volatility) more predictable than individual variances

---

### 🔹 Component 3: VAR & SVAR Models

**Macroeconomic System:**
- **Variables:** Oil prices, Inflation, GDP growth (% changes)
- **Ordering (Cholesky):** Oil → Inf → Growth
- **Rationale:** Small Open Economy (SOE) hypothesis
  - Oil is international → exogenous to domestic economy
  - Oil affects domestic prices → inflation contemporaneously
  - Inflation affects real growth → with lag

**VAR(1) Estimation:**
| Equation | Oil (t-1) | Inf (t-1) | Growth (t-1) | R² |
|----------|-----------|-----------|--------------|-----|
| Oil | 0.524*** | 0.134*** | 0.113** | 0.28 |
| Inflation | 0.295*** | 0.501*** | -0.048 | 0.35 |
| Growth | 0.034 | -0.105** | 0.456*** | 0.25 |

**Diagnostic Results:**
- **Stationarity:** All roots > 1 (eigenvalues inside unit circle) ✓
- **Autocorrelation:** Durbin-Watson ≈ 2.0 for all equations ✓
- **Model Selection:** AIC, BIC, HQIC all minimize at P = 1

**Structural Findings:**

1. **Energy Pass-Through (Direct):**
   - Oil → Inflation coefficient: 0.295 (p < 0.001)
   - 1% oil shock → 0.29% inflation increase immediately
   - Peak response at t=2–3 months, then gradual decay

2. **Recessive Effect (Indirect):**
   - Oil → Growth direct effect: 0.034 (not significant)
   - Oil → Inflation → Growth indirect path (significant)
   - Oil shock depresses growth via inflation erosion

3. **Feedback Loops (Economic Cycle Synchronization):**
   - Inflation → Oil: 0.134 (p < 0.001)
   - Growth → Oil: 0.113 (p < 0.05)
   - Interpretation: Global demand shocks correlate with both oil and domestic cycle

**Impulse Response Functions (12-month horizon):**
- Oil shock impact: Peak immediately, decays to near-zero within 6 months
- Inflation response: Hump-shaped, peaks at 2–3 months
- Growth response: Initially negative, slow recovery toward zero

**Forecast Error Variance Decomposition:**

| Horizon | Oil self | Oil→Inf | Oil→Growth |
|---------|----------|---------|-----------|
| t=1 | 100% | 0% | 0% |
| t=5 | 95.3% | 2.5% | 2.2% |
| t=10 | 95.2% | 2.6% | 2.2% |

**Interpretation:**
- Oil exogeneity confirmed (95.2% self-variance in long run)
- Energy pass-through strong (37% of inflation variance from oil)
- Growth independence from oil (98% idiosyncratic variance)

**Granger Causality (p < 0.05):**
| Direction | P-value | Strength |
|-----------|---------|----------|
| Oil → Inflation | 0.0000 | *** (Strong) |
| Inflation → Oil | 0.0047 | *** (Strong) |
| Inflation → Growth | 0.0287 | ** (Medium) |
| Growth → Oil | 0.0098 | *** (Strong) |

---

## 🛠️ Technical Implementation

### Notebooks Included

#### 1. **GARCHModelsEXCEL.ipynb**
- Comparative fitting of GARCH, EGARCH, GJR-GARCH, APARCH
- Student-t vs Normal vs Skew-t distributions
- BIC/AIC ranking and selection
- Complete diagnostic plots

#### 2. **GARCHGridSearchEXCEL.ipynb**
- Systematic grid search over (p,q,o) specifications
- 72 model configurations tested
- Information criteria comparison
- Convergence diagnostics

#### 3. **NICEXCEL.ipynb**
- News Impact Curve calculation for all 5 models
- Visualization of shock-to-volatility transmission
- Asymmetry assessment (leverage effect)
- Publication-quality plots

#### 4. **DCC_UGARCH_MGARCH_EXCEL.ipynb**
- Univariate GJR-GARCH(1,1,1) for each asset
- DCC(1,1) dynamic correlation estimation
- Correlation time series with confidence bands
- Heatmap visualization

#### 5. **DCCForecastEXCEL.ipynb**
- Multi-step ahead volatility forecasting
- Dynamic correlation predictions (5–30 days)
- Comparison to historical rolling correlations
- Forecast error analysis

#### 6. **SVAREXCEL.ipynb**
- VAR(1) estimation for 3-variable macroeconomic system
- Cholesky identification (recursive ordering)
- IRF calculation with 95% confidence bands
- FEVD decomposition
- Granger causality tests

### Libraries & Dependencies

```
Python Core:
  pandas >= 1.5.0          # Data manipulation
  numpy >= 1.24.0          # Numerical computing
  matplotlib >= 3.7.0      # Visualization
  scipy >= 1.10.0          # Statistical functions

Econometrics:
  arch >= 6.0              # GARCH/EGARCH/GJR-GARCH
  statsmodels >= 0.14.0    # VAR/VECM, time series analysis
  
Excel Integration:
  openpyxl >= 3.10.0       # Read/write Excel files
  xlwings >= 0.30.0        # Excel automation (optional)
```

### Installation

```bash
# Basic installation
pip install pandas numpy scipy matplotlib arch statsmodels openpyxl

# With Excel automation (Windows/Mac only)
pip install pandas numpy scipy matplotlib arch statsmodels openpyxl xlwings

# From requirements file
pip install -r requirements.txt
```

---

## 📁 Project Structure

```
Time-Series-Analysis-Financial-Econometrics/
├── README.md                                      # This file
├── Time_Series_analysis.pdf                       # Full 27-page academic report
├── Zeri_Tommaso.xlsx                              # Data file (Sheet: Ex.1, Ex.2, Ex.3)
│
├── Python_Notebooks/
│   ├── 1_GARCHModelsEXCEL.ipynb                   # GARCH variants comparison
│   ├── 2_GARCHGridSearchEXCEL.ipynb               # Grid search (p,q,o)
│   ├── 3_NICEXCEL.ipynb                           # News impact curves
│   ├── 4_DCC_UGARCH_MGARCH_EXCEL.ipynb            # DCC-GARCH estimation
│   ├── 5_DCCForecastEXCEL.ipynb                   # Multivariate forecasting
│   └── 6_SVAREXCEL.ipynb                          # Structural VAR
│
├── Data/
│   └── Zeri_Tommaso.xlsx
│       ├── Sheet "Ex.1" — Univariate asset (6,580 obs)
│       ├── Sheet "Ex.2" — Multivariate 3-asset system (2,264 obs)
│       └── Sheet "Ex.3" — Macroeconomic variables (Oil, Inf, Growth)
│
├── Output/
│   ├── Figures/
│   │   ├── fig_garch_grid_search.png
│   │   ├── fig_nic_overlay.png
│   │   ├── fig_dcc_correlations.png
│   │   ├── fig_irf_oil_shock.png
│   │   └── ... (29+ figures)
│   ├── Diagnostics/
│   │   ├── garch_diagnostics.csv
│   │   ├── dcc_statistics.csv
│   │   └── var_residuals.csv
│   └── Forecasts/
│       ├── garch_forecast_5days.csv
│       ├── dcc_forecast_30days.csv
│       └── var_forecast_12months.csv
│
└── requirements.txt
```

---

## 🚀 Quick Start

### Running the Analysis

**Step 1: Load Data**
```python
import pandas as pd
data = pd.read_excel('Zeri_Tommaso.xlsx', sheet_name='Ex.1')
returns = data['LogReturns']
```

**Step 2: Fit GARCH(1,1)**
```python
from arch import arch_model
model = arch_model(returns * 100, vol='Garch', p=1, q=1, 
                   mean='Constant', rescale=False)
result = model.fit(disp='off')
print(result)
```

**Step 3: DCC-GARCH Multivariate**
```python
# See DCCForecastEXCEL.ipynb for complete workflow
# Fits GJR-GARCH to each asset, then DCC correlation structure
```

**Step 4: VAR/SVAR for Macroeconomic Analysis**
```python
from statsmodels.tsa.api import VAR
data = pd.read_excel('Zeri_Tommaso.xlsx', sheet_name='Ex.3')
model = VAR(data)
result = model.fit(1)  # VAR(1)
irf = result.irf(12)   # 12-month impulse responses
irf.plot()
```

---

## 📈 Key Results Summary

### Univariate Volatility
- **Model:** GARCH(1,1)-t
- **Fit:** BIC = 58,829.9, Persistence ≈ 0.90
- **Forecast (5-day):** 19.74% → 20.70% (mean-reverting)
- **Tail Risk:** ν = 7.11 (significant fat tails)

### Multivariate Volatility
- **Price1–Price3:** ρ = 0.3626 (strongest correlation)
- **DCC Persistence:** a + b = 0.9942 (very slow decorrelation)
- **Forecasting Horizon:** 10+ days for stable correlation forecasts

### Macroeconomic Shocks
- **Oil Pass-Through:** 0.295 (immediate inflation impact)
- **Growth Impact:** Indirect via inflation (coefficient −0.105)
- **FEVD:** Oil explains 37% of inflation variance (long-run)
- **Granger Causality:** 4 significant bidirectional/unidirectional links

---

## 📚 Theoretical Framework

### GARCH Family

**Baseline GARCH(p,q):**
```
σ²ₜ = ω + Σαᵢε²ₜ₋ᵢ + Σβⱼσ²ₜ₋ⱼ
```

**GJR-GARCH(p,q,o) — Leverage Effect:**
```
σ²ₜ = ω + Σαᵢε²ₜ₋ᵢ + ΣγᵢIₜ₋₁ε²ₜ₋ᵢ + Σβⱼσ²ₜ₋ⱼ
where Iₜ = 1 if εₜ < 0, else 0
```

**EGARCH(p,q,o) — Exponential:**
```
log σ²ₜ = ω + Σαᵢ|εₜ₋ᵢ|/σₜ₋ᵢ + Σγᵢ(εₜ₋ᵢ/σₜ₋ᵢ) + Σβⱼlog σ²ₜ₋ⱼ
```

**APARCH(p,q,o) — Asymmetric Power:**
```
σᵟₜ = ω + Σαᵢ(|εₜ₋ᵢ| + γᵢεₜ₋ᵢ)ᵟ + Σβⱼσᵟₜ₋ⱼ
```

### DCC-GARCH

**Two-Stage Estimation:**
1. Estimate univariate GARCH for each asset → standardized residuals uₜ
2. Model dynamics of correlation matrix:
   ```
   Qₜ = (1−a−b)R̄ + a(uₜ₋₁u'ₜ₋₁) + bQₜ₋₁
   Rₜ = diag(Qₜ)⁻¹/² Qₜ diag(Qₜ)⁻¹/²
   ```

**Conditional Covariance:**
```
Σₜ = DₜRₜDₜ
```

### Structural VAR (SVAR)

**Reduced Form:**
```
yₜ = c + Φ₁yₜ₋₁ + εₜ,  E[εₜε'ₜ] = Σ
```

**Structural Form (Cholesky Decomposition):**
```
Θyₜ = Φ*₀ + Φ*₁yₜ₋₁ + ηₜ,  E[ηₜη'ₜ] = I
```

where **Θ** is lower triangular from Cholesky of Σ.

---

## 🔬 Diagnostic Testing

### GARCH Diagnostics
- Ljung-Box test on standardized residuals (ACF/PACF)
- ARCH-LM test on squared residuals
- Jarque-Bera normality test
- Q-Q plot vs theoretical quantiles

### DCC Diagnostics
- Time-varying correlations visualization
- Correlation heatmaps by period
- Out-of-sample correlation forecast evaluation
- Stability of DCC parameters across subsamples

### VAR Diagnostics
- Durbin-Watson autocorrelation test
- Stability of companion matrix (eigenvalues)
- Residual normality (Jarque-Bera joint test)
- Lag order selection via AIC/BIC/HQ criteria

---

## 📊 Visualization Standards

All notebooks produce publication-ready figures:
- **Resolution:** 300 DPI PNG export
- **Style:** Minimal decoration, data-focused
- **Colors:** Dark professional palette (navy, burgundy, forest green)
- **Typography:** Sans-serif, readable 10–12pt
- **Legends:** Clear labeling, no redundancy

**Example Figure Types:**
- Time series with rolling volatility overlaid
- Grid search heatmaps (model rankings by criteria)
- News Impact Curves (5-model comparison)
- Dynamic correlations (3-pair DCC evolution)
- IRF plots with confidence bands (12-month horizon)
- FEVD stacked bar charts

---

## ⚠️ Notes & Limitations

### Data Assumptions
- **Stationarity:** All models assume I(0) or I(1) variables
- **Weak Form EMH:** Constant mean model justified by no significant AR/MA
- **Missing Values:** Handled by arithmetic mean imputation (potential bias)
- **Structural Breaks:** Not modeled; analysis assumes parameter stability

### Model Limitations
- **Estimation Error:** Covariance matrix inversion sensitive in small samples
- **Correlation Bounds:** DCC constraint (R must be positive definite) sometimes binding
- **SVAR Identification:** Cholesky decomposition is just-identified; other orderings possible
- **Forecasting:** GARCH mean-reverts quickly; long-horizon forecasts converge to unconditional moments

### Interpretation Caveats
- **Granger Causality ≠ Structural Causality:** Predictive relation, not causal mechanism
- **Small Open Economy Assumption:** May not hold during global crises
- **Historical Data:** Past volatility/correlations may not reflect future regimes
- **Transaction Costs:** Ignored in all analyses

---

## 📖 Academic References

**GARCH & Volatility:**
- Bollerslev, T. (1986). Generalized autoregressive conditional heteroskedasticity. *J. Econometrics*, 31(3), 307–327.
- Glosten, L. R., Jagannathan, R., & Runkle, D. E. (1993). On the relation between expected value and volatility. *J. Finance*, 48(5), 1779–1801.
- Nelson, D. B. (1991). Conditional heteroskedasticity in asset returns. *J. Econometrics*, 45(1), 227–251.

**DCC-GARCH:**
- Engle, R. F. (2002). Dynamic conditional correlation. *J. Business & Economic Statistics*, 20(3), 339–350.
- Sheppard, K. (2021). Financial Econometrics Notes. University of Oxford.

**VAR & SVAR:**
- Hamilton, J. D. (1994). *Time Series Analysis*. Princeton University Press.
- Sims, C. A. (1980). Macroeconomics and reality. *Econometrica*, 48(1), 1–48.
- Lütkepohl, H. (2005). *New Introduction to Multiple Time Series Analysis*. Springer.

**News Impact Curves:**
- Pagan, A., & Schwert, G. W. (1990). Alternative models for conditional stock volatility. *J. Econometrics*, 45(1), 267–290.

---

## 📧 Citation

If you use this project in research or education:

```bibtex
@thesis{zeri2025timeseries,
  author = {Zeri, Tommaso},
  year = {2025},
  title = {Time Series Analysis and Financial Econometrics: 
           Volatility Modelling and Macroeconomic Shock Propagation},
  school = {Alma Mater Studiorum, Università di Bologna},
  advisor = {Bacchiocchi, Emanuele}
}
```

Or in text:
> Zeri, T. (2025). Time Series Analysis and Financial Econometrics. Master's thesis, 
> Alma Mater Studiorum, Università di Bologna.

---

## 🤝 Repository & Support

**GitHub:** [tommasozeri/Econometrics-Of-Financial-Markets](https://github.com/tommasozeri/Econometrics-Of-Financial-Markets)

**Related Projects:**
- [Financial Sentiment Analysis NLP](https://github.com/tommasozeri/NLP_Financial_Sentiment)
- [HRP Global Portfolio](https://github.com/tommasozeri/HRP-Global-Portfolio)
- [Financial Econometrics Models](https://github.com/tommasozeri/Financial-Econometrics-Models)

---

## 📋 Checklist for Reproducibility

- ✅ Data file (`Zeri_Tommaso.xlsx`) included
- ✅ All notebooks self-contained and runnable
- ✅ Python requirements specified (`requirements.txt`)
- ✅ Output folder structure defined
- ✅ Diagnostic plots generated automatically
- ✅ Results tables exported to CSV
- ✅ Full academic report (PDF) with methodology

---

**Status:** Production-Ready | **Last Updated:** June 2025 | **Python 3.9+**
