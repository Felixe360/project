# Sentiment Portfolio Optimization

## Problem

One of Ben Graham's notable quotes is:

> “In the short run, the stock market is a voting machine. But in the long run, it is a weighing machine.”

This quote, cited by many investors in recent years, highlights how fear and greed drive short-term price movements. These movements often reflect market sentiment more than intrinsic value — influenced by news, rumors, and emotions.

This research project incorporates this "voting machine" concept by factoring **sentiment** into the portfolio construction process.

---

## Objective

Construct a portfolio that:

- Maximizes return  
- Minimizes risk  
- Takes sentiment into account

---

## Data Processing

- **Stock selection**: 50 stocks were randomly selected using an online tool to minimize bias. Random selection increases diversity across industries, company sizes, and performance histories.

- **ETF selection**: 6 factor ETFs were selected to represent systemic factors (e.g., value, momentum). These are used in expected return modeling.

- **News data**: Finnhub’s API was used to collect a year’s worth of news articles for the selected stocks. These articles were then used for sentiment analysis.

---

## Sentiment Analysis

Sentiment analysis determines the emotional tone of text data. In this project:

- A **BERT-based model** was used, capable of capturing financial context in text.
- Articles were classified as **positive**, **negative**, or **neutral**.

---

## Expected Return Calculation

Expected return helps identify optimal portfolio weights.

- A **factor model** approach was used initially.
- Due to high correlation between factor ETFs, **Principal Component Analysis (PCA)** was applied to remove multicollinearity.
- PCA transforms correlated variables into independent components (PCs), reducing dimensionality.

**Regression equation:**

E(ri) = β₀ + β₁·PC1 + β₂·PC2 + ... + βn·PCn + ε


Where:
- **E(ri)**: Daily expected return of stock *i*  
- **βn**: Coefficients showing the effect of each principal component  
- **ε**: Error term

---

## Optimal Weights for Sentiment Portfolio

Sentiment was incorporated by **adjusting expected returns**:

E(r_sentiment) = E(ri) + α · SentimentScore



Where:
- **E(ri)**: Original expected return of asset *i*  
- **α (alpha)**: A scaling parameter (0–1) controlling sentiment influence  
- **SentimentScore**: Average score from news over the past *X* days (*X* is a tunable parameter)

These adjusted returns were used in **mean-variance optimization** to calculate final weights — balancing short-term sentiment with long-term diversification.

---


## Findings: ##



<img width="1180" alt="Screenshot 2025-05-08 at 10 20 14 AM" src="https://github.com/user-attachments/assets/09f42a4b-b014-47c3-8760-4312e829e42a" />

Adjusting weights by sentiment does result in better performance across all averages but the most optimal is between 15-20 days. This waiting period allows initial reactions to stabilize and begin to trend in a meaningful direction. Markets are highly efficient in pricing news instantly, but sometimes reactions can be exaggerated, leading to short term mispricing. For example, during ROKU’s earnings calls in 2024 Q3, the company beat expectations in revenue and EPS but resulted in a 13% price decline due to uncertain guidance for the following year. This price drop 
doesn’t justify a significant change in the valuation of the company and days after the announcement, the stock was trading above the price before the earnings call.  At times the stock may rebound days after the announcement which the 15-20 day sentiment window tries to take into account by providing a more balanced view of the sentiment. 

Over the testing period, the sentiment portfolio does outperform but eventually the excess returns evaporates and is even with the regular portfolio. This pattern raises an important question of how frequent the portfolio should be re-evaluated given that the focus is on short term sentiment. While quarterly re-adjustment could be an ideal starting point, it may not be a timeframe that works very well. A deeper analysis is needed to identify the optimal re-evaluation window or a metric that detects sentiment reversal.


![Portfolio_weights (1)](https://github.com/user-attachments/assets/12d6d681-5c88-4c20-ab10-ed32ff36e2e5)




