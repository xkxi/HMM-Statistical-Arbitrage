## **HMM-Enhanced Statistical Arbitrage Strategy**



#### **Summary**

* This project implements a market-neutral Statistical Arbitrage (Pairs Trading) strategy on S\&P 500 equities. Unlike traditional mean-reversion models, which often fail during market crashes, this algorithm integrates a Hidden Markov Model (Gaussian HMM) to detect latent market regimes.



* By dynamically filtering out "High Volatility" states, the strategy prevents trading during structural breaks, significantly improving risk-adjusted returns.





#### **Key Metrics (Out-of-Sample Backtest)**

* Sharpe Ratio: 1.32
* Max Drawdown: <10%
* Portfolio: 8 Uncorrelated Pairs
* Period: 2020-01-01 – 2025-12-20





#### **Strategy Logic**



1\. Selection: Cointegration (Engle-Granger)

* The model scans S\&P 500 tickers to identify pairs with a stationary spread.
* A stationary spread implies that the price difference between two assets fluctuates around a constant mean, ensuring mean reversion.
* Validation: Uses the Augmented Dickey-Fuller (ADF) test on the spread residuals.



2\. Risk Management: Regime Detection (HMM)

* A Gaussian Hidden Markov Model is trained on the spread's variance to classify the market into two hidden states:
  * State 0 (Low Variance): The mean-reversion relationship holds. Signal: ON.
  * State 1 (High Variance): The relationship is broken or unpredictable. Signal: OFF.



3\. Execution: Z-Score Thresholds

* Trading signals are generated only when the HMM detects a "Safe" state:
  * Entry: Z-Score < -0.9 (Long the spread).
  * Exit: Z-Score > -0.5 (Mean reversion achieved).

* Position Sizing: Equal-weighted allocation across qualifying pairs.



#### **Project Structure**

* auto_coint_tester: Append all S&P500 ticker price data into one csv
* auto_coint_runner: Make unique pairs between tickers and run cointegration test to determine pairs with P-value < 0.05
* hmm_train: Run initial Hidden Markov Model on top pair sorted by p_value to determine imaginary profit, Sharpe & Calmar Ratio, and max drawdown. Ran through various entry and exit pairs to determine best performning one
* model_validation: Run same model on another ticker pair to validate initial model and prevent overfitting
* find_best_pairs: Loop through top 20 pairs and pick based on set conditions. Run all 8 pairs over span of 5 years using workflow



* SP500_clean: Contains csv of valid companies on SP500 with price data ranging from 2020-01-01 to 2025-12-20
* cointegration_results: csv of tested ticker pairs with p-value < 0.05
* final_price_clean: csv of all valid companies and stock price data ranging from 2020-01-01 to 2025-12-20


* requirements.txt: Python dependencies.



#### **Limitations \& Future Improvements**

* Look-Ahead Bias Mitigation: The current iteration uses a static Z-score calculation based on the full sample mean. A production version would implement a rolling Z-score (e.g., 30-day window).

* Execution Costs: Returns are calculated on a gross basis. Future work will incorporate a slippage model (e.g., 5-10 bps per trade) and transaction fees to validate net alpha.

* Microstructure: The model relies on daily closing prices. Migrating to tick-level data and implementing VWAP execution logic would be necessary for live deployment.


#### Author



### William Gong Xiao



https://www.linkedin.com/in/william-xiao-04b870299/

Johns Hopkins University - Applied Mathematics \& Statistics

