# Interest Rate Modeling: Short Rates to Market Models

## Project Overview

This project, conducted under the guidance of Dr. Tao Pang and Mr. Patrick Roberts at NC State University, focuses on interest rate modeling, exploring short-rate models (Vasicek, Cox-Ingersoll-Ross (CIR), Hull-White) and market models (Brace-Gatarek-Musiela (BGM)). The objective is to model interest rate dynamics, calibrate models to historical yield curve data, and evaluate their performance in pricing bonds and forecasting yield curves. The project leverages financial mathematics to price zero-coupon bonds (ZCBs), coupon bonds, and derivatives like swaptions, with applications in risk management and trading strategies.

**Team Members:**
- **Divija Balasankula** (Project Leader): drbalasa@ncsu.edu
- **Jacob Dolan**: jddolan@ncsu.edu
- **Isaac Gohn**: iegohn@ncsu.edu

## Objectives

- Conduct a literature review on interest rate modeling frameworks.
- Collect and analyze historical yield curve data.
- Implement and compare short-rate models (Vasicek, CIR, Hull-White) and market models (BGM).
- Evaluate model performance using metrics like R-squared, RMSE, and AIC.
- Develop trading strategies based on yield curve predictions and bond pricing.
- Forecast future yield curves and assess inflationary trends.

## Methodology

### Data Collection
- Historical yield curve data sourced for calibration, focusing on U.S. Treasury securities (e.g., T-bills, Treasury Bonds, TIPS) and corporate bonds.
- Data analyzed for yield curve shapes, including inverted yield curves observed on February 4, 2025.

### Models Implemented

1. **Short-Rate Models**:
   - **Vasicek Model**:
     - Equation: `dr(t) = κ[θ - r(t)]dt + σdW(t), r(0) = r_0`
     - Features: Mean-reverting, Gaussian process, allows negative interest rates, closed-form solution for bond pricing.
     - Parameters: `κ` (mean-reversion speed), `θ` (long-term mean rate), `σ` (volatility), `dW(t)` (Brownian motion).
   - **Cox-Ingersoll-Ross (CIR) Model**:
     - Equation: `dr(t) = κ[θ - r(t)]dt + σ√r(t)dW(t), r(0) = r_0`
     - Features: Non-negative interest rates, non-central chi-squared distribution, suitable for parameter calibration.
   - **Hull-White Model**:
     - Extension of Vasicek with time-dependent parameters, effective for fitting observed yield curves.
   - **One-Factor (1F) and Two-Factor (2F) Models**:
     - 1F: `dr(t) = [θ(t) - ar(t)]dt + σdW(t)`
     - 2F: Combines two stochastic processes for more realistic yield curve dynamics:
       - `dr₁(t) = [θ₁(t) - a₁r₁(t)]dt + σ₁dW₁(t)`
       - `dr₂(t) = [θ₂(t) - a₂r₂(t)]dt + σ₂dW₂(t)`
       - `r(t) = r₁(t) + r₂(t)`

2. **Market Models**:
   - **Brace-Gatarek-Musiela (BGM) Model**:
     - Equation: `dL(t, Tᵢ, Tᵢ₊₁) = L(t, Tᵢ, Tᵢ₊₁)σᵢ(t)dWᵢᵀⁱ⁺¹(t)`
     - Features: Models forward LIBOR rates directly, captures correlations between rates, ideal for pricing caps, floors, and swaptions.

### Model Performance Metrics
- **R-squared**: Measures variance explained by the model (higher is better).
- **RMSE**: Quantifies average prediction error (lower is better).
- **AIC**: Balances model fit and complexity (lower is better).

**Performance Results** (as of February 4, 2025 yield curve):
| Model              | R-squared | RMSE   | AIC      |
|--------------------|-----------|--------|----------|
| Vasicek 1F         | 0.76      | 15.83% | -161.57  |
| Vasicek 2F         | 0.93      | 8.61%  | -209.56  |
| CIR 1F             | 0.76      | 15.93% | -160.99  |
| CIR 2F             | 0.79      | 15.01% | -158.49  |
| Hull-White 1F      | 0.76      | 15.98% | -162.68  |
| Hull-White 2F      | 0.84      | 12.95% | -172.09  |
| BGM                | 0.76      | 9.44%  | -204.44  |

- **Vasicek 2F** excelled in capturing inverted yield curves (R-squared: 0.93).
- **BGM** showed lower RMSE (9.44%) for yield predictions.
- **Hull-White 2F** effectively mimicked yield trends.
- **CIR 2F** demonstrated moderate fit.

### Bond Pricing
- **Zero-Coupon Bond (ZCB)**: `P = F / (1 + r)ⁿ`
- **Coupon Bond (CB)**: `P = Σ(C / (1 + r)ᵗ) + F / (1 + r)ⁿ`
- Pricing results compared model estimates to market prices for securities like T-bills, Treasury Bonds, TIPS, and corporate bonds (see Page 27 of the presentation).

### Trading Strategies
Based on the Medium article ["Cracking the Yield Curve Code"](https://medium.com/@divijarao.b/cracking-the-yield-curve-code-d68ce7d3f02f) written by Divija Balasankula, the following trading strategies were developed:
1. **Yield Curve Arbitrage**:
   - Exploit discrepancies between model-predicted yields (e.g., Vasicek 2F, BGM) and observed market yields.
   - Strategy: Buy bonds undervalued by the model and sell overvalued bonds, focusing on mispriced T-bills and Treasury Bonds.
2. **Curve Steepening/Flattening Trades**:
   - Use Vasicek 2F forecasts indicating inflationary trends (e.g., April 4, 2025 forecast) to position for yield curve steepening.
   - Strategy: Long short-maturity bonds (e.g., 2-year Treasury) and short long-maturity bonds (e.g., 10-year Treasury) when a steepening curve is predicted.
3. **Interest Rate Derivatives Trading**:
   - Leverage BGM model for pricing swaptions and caps/floors.
   - Strategy: Enter swaption positions when model correlations suggest undervaluation of volatility in multi-period derivatives.
4. **Hedging with TIPS**:
   - Use TIPS pricing discrepancies (e.g., model price: 92.82 vs. market price: 104.84 for 2-year TIPS) to hedge inflation risk.
   - Strategy: Buy TIPS when model prices are significantly lower than market prices, anticipating inflation adjustments.

## Repository Structure
```
interest-rate-modeling/
├── data/
│   └── yield_curve_data.csv     # Historical yield curve data
├── src/
│   ├── vasicek_model.py         # Vasicek 1F and 2F model implementation
│   ├── cir_model.py             # CIR 1F and 2F model implementation
│   ├── hull_white_model.py      # Hull-White 1F and 2F model implementation
│   ├── bgm_model.py             # BGM model implementation
│   ├── bond_pricing.py          # Bond pricing functions (ZCB, CB)
│   └── analysis.py              # Data analysis and performance metrics
├── notebooks/
│   └── yield_curve_analysis.ipynb # Jupyter notebook for yield curve visualization
├── results/
│   └── model_performance.csv    # Model performance metrics
└── README.md                    # Project documentation
```

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/interest-rate-modeling.git
   cd interest-rate-modeling
   ```

2. **Set Up Python Environment**:
   - Requires Python 3.8+.
   - Install dependencies:
     ```bash
     pip install -r requirements.txt
     ```
   - Example `requirements.txt`:
     ```
     numpy
     pandas
     scipy
     matplotlib
     ```

3. **Data Preparation**:
   - Place historical yield curve data in `data/yield_curve_data.csv`.
   - Ensure data includes columns for dates, maturities, and yields.

4. **Run Analysis**:
   - Execute model scripts:
     ```bash
     python src/vasicek_model.py
     ```
   - Visualize results using the Jupyter notebook:
     ```bash
     jupyter notebook notebooks/yield_curve_analysis.ipynb
     ```

## Usage

1. **Model Calibration**:
   - Use `src/analysis.py` to calibrate model parameters (`κ`, `θ`, `σ`) using historical data.
   - Example: Calibrate Vasicek 2F model to February 4, 2025 yield curve.

2. **Bond Pricing**:
   - Run `src/bond_pricing.py` to compute ZCB and CB prices.
   - Compare model prices to market prices (e.g., T-bill, Treasury Bonds).

3. **Yield Curve Forecasting**:
   - Use `src/vasicek_model.py` for yield curve forecasts (e.g., April 4, 2025).
   - Visualize forecasts in `notebooks/yield_curve_analysis.ipynb`.

4. **Trading Strategy Implementation**:
   - Implement arbitrage and steepening/flattening strategies using model outputs.
   - Example: Use BGM model outputs to identify mispriced swaptions.

## Future Work
- Enhance model calibration with machine learning techniques.
- Incorporate stochastic volatility models for improved derivative pricing.
- Expand trading strategies to include dynamic hedging and portfolio optimization.
- Validate models with additional datasets (e.g., Eurozone yields).

## Contact
For questions or collaboration, reach out to:
- Divija Balasankula: drbalasa@ncsu.edu
- Jacob Dolan: jddolan@ncsu.edu
- Isaac Gohn: iegohn@ncsu.edu

## Acknowledgments
- Academic Advisors: Dr. Tao Pang, Mr. Patrick Roberts
- Institution: NC State University, Financial Mathematics Department

---

**Note**: This project is for educational purposes. Trading strategies carry financial risk and should be tested thoroughly before deployment.
