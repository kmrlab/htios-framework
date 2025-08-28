# HTIOS Framework: Systematic Approach to Algorithmic Trading Strategy Development 📊

## Introduction

HTIOS (Hypothesis-Testing-Implementation-Optimization-Scaling) is a comprehensive framework for sequential implementation of algorithmic trading strategies in institutional funds. Developed with best practices in quantitative analysis and risk management, the framework provides a structured approach from idea to strategy scaling.

## Framework Architecture

The framework consists of five sequential stages:

```
HTIOS
├── 1. Hypothesis 💡
├── 2. Testing 🔬  
├── 3. Implementation ⚙️
├── 4. Optimization 🔧
└── 5. Scaling 📈
```

---

## Stage 1: Hypothesis 💡

### 1.1 Objective

**Defining strategic fund objectives:**

1. **Alpha generation** — creating additional returns above market benchmark
2. **Increasing scalability** — expanding assets under management without losing efficiency
3. **Capital diversification** — reducing correlation with main portfolio
4. **Creating uncorrelatedness** — developing strategies with low correlation to other positions
5. **Improving Sharpe ratio** — optimizing risk-adjusted returns
6. **Business unit expansion** — implementing market-making, high-frequency trading

### 1.2 Research

**Fundamental analysis of market inefficiency:**
- Studying academic research
- Market microstructure analysis
- Identifying arbitrage opportunities
- Researching behavioral distortions of market participants
- etc.

### 1.3 Hypothesis Formation (Model)

**Mathematical strategy justification:**

For each strategy, a hypothesis is formulated as a mathematical model. For example, for a Mean-Reversion strategy:

```
H₀: Prices temporarily deviate from fair value 
H₁: Short-term supply/demand imbalances create arbitrage opportunities
```

**Economic justification:**
- Identifying the source of inefficiency
- Analyzing factors affecting price behavior
- Determining time frames for anomaly existence

### 1.4 Manual Model Testing

**Preliminary concept validation:**

**Evaluation criteria:**
- Basic/inefficient way of extracting profit
- Testing workability on historical data
- Obtaining statistically significant results (30-100 trades)

**Metrics for initial assessment:**
- Win Rate > 55%
- Profit Factor > 1.5
- Maximum Drawdown < 15%

---

## Stage 2: Testing 🔬

### 2.1 Infrastructure

**Creating basic infrastructure for testing:**
- Database (alternative source for data) for Bybit data
- Data parsing and normalization
- Quantitative testing and research
- MVS (Minimum Viable Strategy)
- Strategy robustness testing
- Stress testing
- Overfitting
- Tester

### 2.2 Quantitative Research

**Comprehensive statistical analysis using Mean-Reversion as example:**

#### Time series stationarity tests:

**ADF test (Augmented Dickey-Fuller):**
```python
# Null hypothesis: series is non-stationary
# H₀: ρ = 1 (unit root is present)
# H₁: ρ < 1 (series is stationary)

Statistic: -27.681067426486
p-value: 0.0
Critical values: {'1%': -3.430448, '5%': -2.8615837, '10%': -2.5667932}
Conclusion: Series is stationary (reject null hypothesis)
```

**KPSS test:**
```python
# Null hypothesis: series is stationary
KPSS Statistic: 0.029786021736160143
p-value: 0.1
Critical values: {'10%': 0.347, '5%': 0.463, '2.5%': 0.574, '1%': 0.739}
Conclusion: Series is stationary (cannot reject null hypothesis)
```

#### Analysis through Half-life (Ornstein-Uhlenbeck model):

**Mathematical model:**
```
dX(t) = θ(μ - X(t))dt + σdW(t)

where:
- θ - speed of mean reversion
- μ - long-term mean value  
- σ - process volatility
- W(t) - Wiener process
```

**Half-life calculation:**
```
Half-life = ln(2) / θ ≈ 0.693 / θ
```

#### Detailed stationarity tests (one of the simplest examples, on SMA):

1. **Absolute price deviation from SMA**
2. **Percentage price deviation from SMA** 
3. **Z-score of percentage price deviation from SMA**
4. **Distribution of percentage price deviation** (maximums in %)
5. **Min-max price deviation from SMA**
6. **QQ-plot of percentage price deviation**
7. **Analysis of frequency and amplitude of price deviations from SMA(30)**
8. **Research of critical levels of price return to SMA**
9. **Correlation between percentage deviation from SMA and return probability**
10. **Statistical analysis of duration of periods exceeding critical levels**
11. **Analysis of moments preceding large deviations**
12. **Determination of critical levels of deviation from SMA**
13. **Determination of overbought/oversold areas through RSI**

### 2.3 Creating MVS

**Minimum Viable Strategy (MVS)** — ready strategy for testing, containing minimum necessary functionality to validate hypothesis.

### 2.4 Robustness Testing

**Process of evaluating strategy's ability to maintain efficiency in various market conditions and parameter changes:**

**Testing approaches:**
1. **Walk-forward testing** — sequential optimization on data with validation on next period
2. **Bootstrap resampling** — creating multiple synthetic samples through random trade selection
3. **Out-of-sample testing** — splitting data into training and testing samples
4. **K-fold cross-validation** — dividing data into K parts for validation

#### Walk-Forward Analysis:

```
Training period: 70% data → Testing period: 30% data
```

**Evaluation metrics:**
- **Efficiency Ratio** = Out-of-sample Performance / In-sample Performance
- **Result stability** — coefficient of variation of returns across periods
- **Performance degradation** — comparison of results between periods

#### K-fold Cross-Validation approaches:

1. **K-fold** — standard k-fold cross-validation
2. **Combinatorial Purged Cross-Validation (CPCV)** — accounts for temporal dependencies
3. **Purged k-fold** — excludes information leakage between folds
4. **TimeSeriesSplit** — specifically for time series
5. **Leave-One-Out (LOO)** — extreme case of k-fold
6. **Nested k-fold** — two-level validation for hyperparameter selection

**Statistical tests:**
1. **Reality Check / Hansen's SPA Test** — checking if strategy superiority is statistically significant
2. **Bootstrap resampling** — creating multiple synthetic samples
3. **Variance Ratio Test** — testing return variance stability
4. **Information Ratio (IR)** = (Rₚ - Rᵦ) / TE, where TE — tracking error

**Significance assessment and Sharpe coefficient:**
```python
# Deflated Sharpe Ratio — adjusted Sharpe ratio
DSR = SR × √(1 - γ₁²/T), where:
- SR — observed Sharpe ratio  
- γ₁ — return skewness coefficient
- T — number of observations

# Probabilistic Sharpe Ratio (PSR)
PSR = Φ[(SR - SR*) / √((1 + Yₓ*(1 + SR²)) / (T - 1))]
```

### 2.5 Overfitting Testing

**Overfitting** — situation where strategy is too precisely tuned to historical data and shows excellent backtest results but performs poorly in real market.

**Overfitting causes:**
- Using too short period for testing
- Excessive parameter optimization on all available history
- Multiple testing without multiplicity adjustment
- Data snooping — repeated testing of different ideas on same data
- Insufficient number of trades in backtest

**Detection methods:**

#### Deflated Sharpe Ratio — adjusted Sharpe ratio:
```python
DSR = SR × √(1 - skewness²/(4×T))
where:
- SR — observed Sharpe ratio
- skewness — return asymmetry  
- T — number of periods
```

#### Probabilistic Sharpe Ratio (PSR):
```python
PSR = Φ[(SR - benchmark_SR) / σ_SR]
where Φ — normal distribution function
```

#### Information Ratio decay:
- Information ratio deterioration when transitioning from in-sample to out-of-sample

#### Random permutation test (Permutation Test):
- Comparing strategy performance with results on shuffled data

#### Profit factor degradation:
- Comparing profit factor between training and testing samples

**Prevention methods:**
- **Win rate consistency** — stability of winning trade percentage
- **White's Reality Check** — Bailey-López de Prado formula for hft
- **Random permutation test** 
- **Maximum drawdown analysis** — comparing drawdowns between samples

### 2.6 Stress Testing ⚠️

**Three main approaches:**

1. **Historical crisis testing** — analyzing strategy behavior during historical crashes and high volatility periods
2. **Monte-carlo simulations** — random permutation of time series or trades to create alternative market development scenarios  
3. **Bootstrap** — removing random trades to test result stability

### 2.7 Creating Tester

**Ready backtester with configured parameters for entering trading.**

**Key evaluation metrics:**

#### Basic return indicators:
- **Total Return** = (Final capital - Initial capital) / Initial capital × 100%
- **Annualized Return** = (1 + Total Return)^(252/trading_days) - 1  
- **Compound Annual Growth Rate (CAGR)** = (Ending_Value/Beginning_Value)^(1/years) - 1

#### Risk-adjusted metrics:
- **Sharpe Ratio** = (Rₚ - Rᶠ) / σₚ > 1.5 (where Rᶠ — risk-free rate)
- **Sortino Ratio** = (Rₚ - MAR) / Downside_Deviation > 1  
- **Calmar Ratio** = Annual Return / Maximum Drawdown > 1
- **MAR Ratio** = Compound Annual Return / Maximum Drawdown

#### Risk indicators:
- **Maximum Drawdown** < 15% — maximum portfolio drawdown
- **Value at Risk (VaR 95%)** < 2% — potential losses with 95% probability
- **Conditional VaR (CVaR)** — expected losses beyond VaR
- **Beta** — sensitivity to market movements

#### Stability indicators:
- **Win Rate** = Profitable Trades / Total Trades > 55%
- **Profit Factor** = Gross Profit / Gross Loss > 1.5
- **Recovery Factor** = Net Profit / Maximum Drawdown > 2
- **Payoff Ratio** = Average Win / Average Loss > 1.5

#### Advanced metrics:
- **Information Ratio** = (Portfolio Return - Benchmark Return) / Tracking Error
- **Treynor Ratio** = (Rₚ - Rᶠ) / βₚ  
- **Jensen's Alpha** = Rₚ - [Rᶠ + βₚ(Rₘ - Rᶠ)]
- **Omega Ratio** = Probability Weighted Gains / Probability Weighted Losses

---

## Stage 3: Implementation ⚙️

### 3.1 Infrastructure  

**Creating infrastructure for production robot launch:**
- Dedicated/cloud server
- API connection
- Monitoring and alert system
- Logging and monitoring

### 3.2 MVS

**Minimum Viable Strategy (MVS)** — ready strategy for testing, but in our case for production.

### 3.3 MVS Testing on Demo Account

**Connecting robot to API and demo account on Bybit for strategy testing in real market conditions.**

### 3.4 Trade Analysis

**Monthly detailed trading activity analysis:**

1. **Take existing backtest data as basis for comparison of work correctness by data**
2. **Compare backtest with live**

#### Paire Sharpe degradation model:
```python
# This model predicts how much Sharpe ratio deteriorates 
# when transitioning from backtest to live
SR_live = SR_backtest × √((1 - k) / 1), where:
- SR_live — expected live trading Sharpe  
- SR_backtest — backtest Sharpe
- k — degradation coefficient (usually 0.25-0.5)
```

#### Bayesian approach:
```python
# Uses Bayes' theorem to estimate probability that
# strategy is actually profitable based on our data
P(Strategy Profitable | Observed Data) = 
    P(Observed Data | Strategy Profitable) × P(Strategy Profitable) / 
    P(Observed Data)
```

**Main ideas for strategy updating:**
- SR_live ≈ median Sharpe in real trading
- SR_backtest — backtest Sharpe  
- k — degradation coefficient (usually 0.25-0.5)

**Calculation example:**
- SR_backtest = 2.5
- k = 250 days  
- SR_prior = 0
- t = 100
- SR_actual = (2.5 × 250 + 0 × 100) / (250 + 100) = 1.79

### 3.5 Production Allocation in MVS

**According to allocation distribution in fund, for example, by MVO, after testing correlation between strategies, we add funds to strategy according to distribution.**

### 3.6 Monitoring

**Continuous strategy operation control:**
- Position monitoring
- Monitoring necessary metrics  
- Comparing backtest and live results weekly
- Evaluating strategy efficiency in fund context and correlation

### 3.7 Reporting

**Weekly and monthly reporting on strategy performance results.**

---

## Stage 4: Optimization 🔧

### 4.1 Optimization

**Something not satisfying? Change and run tests again**

#### Optimization directions:

**1. Entry parameter optimization:**
- Indicator threshold values
- Breakout levels for breakout strategies  
- Time windows for signal calculation
- Increasing standard deviation multiplier (e.g., from 2 to 2.5-3) for position entry
- Selecting optimal moving average window size

**2. Position size management:**
- Implementing Kelly Criterion or its modification
- Risk parity approach for strategy portfolio
- Adapting capital size to market volatility

**3. Exit optimization:**
- Dynamic stop-losses (trailing, volatility-based)
- Multi-level take-profits with partial closing
- Time exits (time stops)
- Reverse signal exits: Improving replacement of fixed SL with ATR-based

**4. Trade filtering:**
- Adding market regimes (trend/sideways)
- Volatility filters  
- Time filters (weekdays)
- Applying trend strength indicators (ADX, MACD) for signal filtering during strong trend periods

**5. Execution optimization:**
- Large order splitting algorithms (TWAP, VWAP)
- Optimal order type selection
- Pyramiding — gradual position increase when price moves favorably

**6. Parameter adaptiveness:**
- Machine learning for predicting optimal parameters
- Switching regimes depending on volatility

**7. Risk management:**
- Implementing portfolio stop-losses
- Maximum exposure limits  
- Correlation limits between positions

**8. Improving backtest and live metrics:**
- All backtest metrics should improve

---

## Stage 5: Scaling 📈

### Vertical scaling:
- **Gradual position size increase**
- **Market impact analysis at different volumes**  
- **Determining strategy capacity limit**
- **Using iceberg orders for large trades**: Until significant slippage appears (~2-3% of profit)

### Multi-market expansion:
- **Other exchanges**
- **Working with CEX**

### Instrument diversification:
- **Expanding tradable asset universe**: from BTC moving to ETH etc.

### Temporal scaling:
- **Application on different timeframes simultaneously**
- **Moving toward pure trades**

---

## Mathematical Models and Formulas

### Basic risk metrics:

**Sharpe Ratio:**
```
SR = (Rₚ - Rᶠ) / σₚ

where:
- Rₚ — portfolio return
- Rᶠ — risk-free rate  
- σₚ — return standard deviation
```

**Sortino Ratio:**
```
Sortino = (Rₚ - MAR) / DD

where:
- MAR — minimum acceptable return
- DD — downside deviation (negative return volatility)
```

**Kelly Criterion for position size determination:**
```
f* = (bp - q) / b

where:
- f* — optimal capital fraction
- b — win to loss ratio  
- p — win probability
- q — loss probability (1-p)
```

**Maximum Drawdown:**
```
MDD = max[1 - (Value_t / Peak_t)]

where Peak_t — maximum portfolio value up to time t
```

---

## Practical Example: Mean-Reversion Strategy

### Stage 1: Hypothesis
**Concept:** High-cap cryptocurrencies demonstrate mean-reversion on short time intervals due to short-term supply/demand imbalances.

### Stage 2: Testing
**Entry and exit signals:**
```python
# Position entry
entry_signal = (price < SMA_20 * (1 - threshold)) and (RSI < 30)

# Position exit  
exit_signal = (price > SMA_20) or (RSI > 70) or (holding_period > max_hold)
```

**Strategy parameters:**
- Asset: BTC/USDT
- Timeframe: 5-minute candles
- SMA period: 20
- Z-score threshold: ±2.0
- Maximum holding period: 4 hours
- Stop-loss: -3%
- Take-profit: +2%

### Stage 3: Backtest Results
- **Period:** 2024-01-01 to 2024-12-31
- **Total trades:** 1,247
- **Win Rate:** 67.3%
- **Profit Factor:** 1.89
- **Sharpe Ratio:** 2.14
- **Maximum Drawdown:** 8.7%
- **Annualized Return:** 43.2%

---

## Risk Management ⚠️

### Maximum risk per trade:
- **Maximum 30% of strategy capital**

### Maximum portfolio risk:
- **VaR 95% < 2% daily**

### Managing drawdowns:
- **Reduce position sizes by 50% when drawdown > 10%**

### Transaction costs:
- **Real execution costs:** 0.002% per trade
- **Expected slippage:** Half of bid-ask spread (usually 0.02-0.05%)
- **Minimizing market impact:** 0.1% for orders < $100K

### Market regimes:
- **Market conditions where strategy works better:** If market filter exists, then ATR < value, low market trendiness
- **When strategy should be disabled:** Strong trends
- **How to determine regime change:** ADX < 25 (no strong trend)
- **Is parameter adaptation needed:** Increase Z-score threshold to 2.5 during high volatility

---

## Conclusion

The HTIOS framework provides a systematic approach to developing, testing, and implementing algorithmic trading strategies. Each stage has clear success criteria and evaluation methodology, minimizing risks and maximizing probability of successful strategy implementation in institutional funds.

**Key principles:**
- **Strict adherence to validation procedures**
- **Quantitative justification of each decision**  
- **Continuous monitoring and optimization**
- **Risk management at each stage**

The framework adapts to different strategy types: arbitrage, market-making, momentum, mean-reversion, and can scale from single strategies to complex algorithm portfolios.
