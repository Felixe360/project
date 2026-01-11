# 📊 Sentiment Portfolio Optimization

## Overview

This project is inspired by Ben Graham’s famous idea:

> *“In the short run, the stock market is a voting machine. In the long run, it is a weighing machine.”*

Traditional portfolio construction focuses on the *weighing machine*—fundamentals, factors, and long-term risk premia. This project explores the other side of the quote: **can short-term market sentiment, as reflected in news coverage, be systematically incorporated into portfolio optimization?**

**Goal:**  
Build a portfolio that maximizes risk-adjusted returns by explicitly incorporating market sentiment into expected returns.

---

## Data Collection

To avoid selection bias and ensure broad market coverage, the data pipeline was designed with strict rules:

- **Equities:**  
  50 stocks selected randomly to ensure industry and size diversity.
- **Risk Factors:**  
  6 factor ETFs representing common systematic risks (e.g., value, momentum).
- **News Data:**  
  One full year of company-specific news articles pulled from Finnhub’s API for each stock.

---

## Sentiment Analysis

Rather than using simple keyword counts, sentiment was extracted using a **BERT-based language model**.

- Each article was classified as **positive**, **negative**, or **neutral**
- Context-awareness allows the model to distinguish subtle differences in meaning  
  (e.g., “strong earnings” vs. “strong headwinds”)
- Sentiment scores were aggregated over rolling time windows to capture evolving market mood

---

## Expected Return Modeling

### Factor Model Challenges

The initial expected return model was based on factor regressions using the selected ETFs. However, these factors exhibited significant multicollinearity.

### Principal Component Analysis (PCA)

To address this issue, **PCA** was applied to the factor returns:

- Transformed correlated ETFs into orthogonal components
- Reduced noise and instability in the regression
- Produced cleaner and more reliable expected return estimates

---

## The Sentiment Adjustment

The core innovation of the project is integrating sentiment directly into expected returns:

\[
\text{Adjusted Return} = \text{Base Expected Return} + (\alpha \times \text{Sentiment Score})
\]

Where:
- **Sentiment Score** is the average news sentiment over a rolling window
- **α** controls the influence of sentiment on returns

These adjusted returns were then fed into a **Mean–Variance Optimization** framework to determine final portfolio weights.

---

## Results & Insights

### 1. Sentiment is Predictive — with the Right Window

- Best performance occurred with **15–20 day sentiment windows**
- Shorter windows were too noisy
- Longer windows diluted the signal

This window appears to capture the transition from emotional reaction to emerging price trend.

---

### 2. Evidence of Market Overreaction

A notable example occurred with **ROKU (Q3 2024)**:

- Company beat EPS and revenue expectations
- Slightly uncertain guidance triggered a ~13% selloff
- Price decline was not supported by fundamentals
- Stock rebounded shortly afterward

The sentiment-adjusted model captured this rebound, while a standard factor model would have exited the position.

---

### 3. Sentiment Alpha Decays

While the sentiment-enhanced portfolio outperformed initially, excess returns faded over time.

**Interpretation:**  
Sentiment is a **short-term signal**, not a permanent edge. As information is absorbed, prices revert to fundamentals.

---

## Key Takeaways

- Market sentiment meaningfully improves short-term portfolio performance
- The “voting machine” effect is real and measurable
- Static rebalancing schedules (e.g., quarterly) are too slow
- Capturing sentiment-driven alpha requires **frequent re-evaluation**

---

## Future Work

- Identify the optimal rebalancing frequency
- Explore nonlinear sentiment impacts
- Test robustness across market regimes
- Compare news sentiment with social media sentiment

---

## Repository Structure (Suggested)

```text
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
├── src/
├── figures/
├── reports/
└── README.md

