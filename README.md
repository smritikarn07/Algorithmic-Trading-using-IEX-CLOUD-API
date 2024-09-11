# Algorithmic-Trading-using-IEX-Cloud-API

- **User Input** : Portfolio Size of the investor, number of stocks to invest in (default = 50)
- **Data Used** : Data Extracted from IEX Cloud API and data that contains a list of 500 S&P 500 listed companies
- **Output** : Excel sheet that consists of detailed information about each company and the number of stocks investor should invest in.

## Abstract:
The project aims at extracting raw and live data from the IEX Cloud API, development of two strategies named **_Quantitative Momentum Investing Strategy_** and **_Quantitative Value Investing Strategy_** and generating the number of stocks to be bought by the investor according to their portfolio size.

## Definition
Algorithmic trading is a process for executing orders utilizing automated and pre-programmed trading instructions to account for variables such as price, timing and volume. An algorithm is a set of directions for solving a problem. Computer algorithms send small portions of the full order to the market over time.
the algorithmic trading basically means using computers to make trading decisions. While there are many types of algorithmic trading, their main difference is their speed of execution.

Algorithmic trading is also called Black Box Trading since it uses mathematical models to code programs for automatic trading. These programs have direct access to market and can order trades from market when certain conditions are met. Thus algorithmic trading is a combination of finance, trading, mathematics and computer science. 

Algorithmic trading makes use of complex formulas, combined with mathematical models and human oversight, to make decisions to buy or sell financial securities on an exchange. Algorithmic trading is the use of process- and rules-based algorithms to employ strategies for executing trades.

## Motivation
Reasons why Algorithmic Trading is much more profitable than Manual trading:

* Ability to backtest. Here, the algorithms can be backtested based on past data to see if it would have worked in the past. 
* Increased Speed and Accuracy: Automation of algorithmns not only increase speed but also decreases chances to avoid the pitfalls of accidentally putting in the wrong trade associated with human trades.
* Reduced Transaction Costs
* Trading can not be emotionally affected

## Methodology:
* Data Extraction
* Hypothesis for Strategy
* Back Testing
* Implementing the Strategy

### Data Extraction
Usage of raw and live data for algorithmic trading is one of the most important tasks in the project. 
Thus, I've used an _**Application programming interface (API)**_ to automate and extract raw data from the web. The API used in the project is IEX Cloud API.

***IEX Cloud API*** IEX Cloud is a platform that makes financial data and services accessible to everyone. And is based on REST, has resource-oriented URLs, returns JSON-encoded responses, and returns standard HTTP response codes. The base url for the API is: https://cloud.iexapis.com/ We support JSONP for all endpoints.

***IEX Cloud API Sandbox*** Token is used to request testing data from the stock market using _requests_ library.

### Quantitative Momentum Strategy:
A Quantitative Momentum strategy is a strategy implemented to choose stocks that have increased in price the most. 
Momentum is the rate at which a stock's or an index's returns vary. A trading method known as momentum investing involves buying stocks when they are rising and selling them when they appear to have peaked. The goal of the strategy is to choose equities with the strongest momentum, or those that are reliable and consistently perform well. Because low-quality momentum can frequently be triggered by recent news that is  most likely to repeat itself in the future, thus, high-quality momentum stocks are recommended.

"Momentum investing" refers to investing in the stocks that have increased in price the most.
For this project, I've build an investing strategy that selects the n-stocks with the highest price momentum. Further calculating the recommended trades for an equal-weight portfolio of these n-stocks.

Momentum investing is a trading strategy in which investors buy securities that are rising and sell them when they look to have peaked. The strategy is to filter out highest momentum stocks i.e., stocks which are stable and perform well in long periods of time.

* **High-quality momentum stocks** perform slow and steady over long periods of time and are significantly less volatile.

* **Low-quality momentum stocks** are ones that does not show any momentum for a long time.

High-quality momentum stocks are  preferred because low-quality momentum can often be caused by short-term news that is likely to be repeated in the future.

* To identify HQM, the strategy selects stocks from the highest percentiles of:
  * 1-month price returns
  * 3-month price returns
  * 6-month price returns
  * 1-year price returns
  * 5-year price returns

1. **Calculating momentum percentile** scores for every stock, percentile scores for the following metrics for every stock is calculated:
  * Five-Year Price Return
  * One-Year Price Return
  * Six-Month Price Return
  * Three-Month Price Return
  * One-Month Price Return

2. **HQM Score** is calculated to filter stocks in the investing strategy.
The HQM Score is the arithmetic mean of the 5 momentum **percentile** scores that we calculated in the last section.
   Q. Why mean of percentile scores of these metrics and not mean of the metric itself?
   Ans: The mean of percentile scores is preferred over the mean of the raw metrics because it standardizes the metrics, ensuring they are on the same scale and      
        comparable. Each metric has a different scale and significance (e.g., P/E vs. P/B), so averaging raw values would distort the analysis, while percentiles 
        provide a consistent relative ranking across different metrics, offering a balanced comparison.

4. **Accepting the number of shares** to be invested
_Strategy_: Investing in top n-high quality momentum stocks
Accepts the number of stocks the investor is wants to invest in, identifying the 'n' best momentum stocks in our universe by sorting the DataFrame on the HQM Score column and dropping all but the top 'n' entries.
Further, removing the remaining Low-Momentum Stocks

5.  **Determining the Number of Shares** to Buy based on the HQM Score

### Quantitative Value Strategy
**Value investing** refers to investing in the stocks which are cheapest relative to common measures of business value (like earnings or assets).
Value investing is an investment strategy that involves picking stocks that appear to be trading for less than their intrinsic or book value.

Investing strategy is built that selects the 50 stocks with the best value metrics. Further calculating the recommended trades for an equal-weight portfolio of these 50 stocks high value stocks.

A basket of valuation metrics to build a robust quantitive value strategies is considered and stocks are filtered with respect to the lowest percentiles on the following metrics:

* **Price-to-earnings ratio**: It is the ratio for valuing a company that measures its current share price relative to its earnings per share (EPS). It indicates the dollar amount an investor can expect to invest in a company in order to receive $1 of that company's earnings. They determine the relative value of a company's shares in an apples-to-apples comparison and can also be used to compare a company against its own historical record or to compare aggregate markets against one another or over time.
    * Low P/E: Stock is undervalued

* **Price-to-book ratio**: It is a financial ratio used to compare a company’s current market price to its book value. Companies use this ratio to compare a firm's market capitalization to its book value. Book Value per Share is the value of the company’s assets as recorded on the balance sheet, minus liabilities, divided by the total number of shares outstanding.
    * P/B < 1: The stock price is undervalued.

* **Price-to-sales ratio**: It is a valuation ratio that compares a company’s stock price to its revenues. It is an indicator of the value that financial markets or investors have placed on each dollar of a company’s sales or revenues.
    * Low P/S: The stock price is undervalued.

* **Enterprise Value (EV)** is a measure of a company's total value, often viewed as a more comprehensive alternative to market capitalization. 
    * EV = Market Capitalization + Total Debt - Cash Reserves
    * Reflects actual cost of acquiring the entire company

* **EBITDA** : Reported Earnings Before Interest, Tax, Depreciation and Amortization.

* **EV/EBITDA** : Signifies how cheap or expensive the company is relative to its earnings.
    * Low EV/EBITDA : Company is undervalued

 **EV/GP** : Enterprise Value divided by Gross Profit
    * Low EV/EBITDA : Company is undervalued

1. **Computing percentiles** of these metrics after pulling raw data.
  * Price-to-earnings ratio
  * Price-to-book ratio
  * Price-to-sales ratio
  * EV/EBITDA
  * EV/GP

2. **Computing Robust Value** Score of each Share.
_Robust Value Score_ is the Arithmetic mean of the 5 percentile scores used above. The best 50 stocks are calculated by sorting the dataframe with respect to robust value scores of each stock. Portfolio size the investor is willing to invest.

3. **Determining the Number of Shares** to Buy based on the robust value score
_Strategy_: Rank the companies from lowest to highest and focusing on companies with low scores. Further, Invest in the top 50 ranked stocks.


### Comparison Between Momentum Investing Strategy and Value Investing Strategy:
#### Value Investing Strategy:
Advantages
* Less Voltile
* Long Term Potential (Useful for investors interested in long term investing and determining nice stocks)

Disadvantages
* Value Traps: Undervalued stocks may have underlying reasons to be undervalued
* Slow Returns: Might take long to materialize

**Best For**: People looking for long term investing

#### Momentum Investing Strategy:
Advantages
* High Returns
* Leverages market sentiment: Thus hyping up the stock prices more than original

Disadvantages
* Highly Volatile
* Requires Precise timing
  
**Best For**: People looking for short term investing
