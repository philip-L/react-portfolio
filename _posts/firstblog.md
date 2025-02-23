---
date: '2021-10-14T11:50:54.000Z'
title: Finviz and Yahoo Finance API's for Retail Investors
tagline: October 14, 2021
preview: >-
  Since the March 2020 covid flash-crash, the stock market has been very bullish, with quantitative easing adding so much new money into the market, gains seem to be popping up everywhere. It may be a good time to enter the market with a bullish strategy or perhaps there are still oportunities for bearish strategies, and to prepare for market consolidation.
image: >-
  https://www.morningstar.com/features/what-prior-market-crashes-can-teach-us-in-2020/chart-growth-of-1-dollar-headline-3x-106e17d32465fecbc901f2eba32e5cf3.png
---

## Intro

If you saw my email' inbox, you'd think I'm a retail trader. Retail investors are powerful in numbers but can be manipulated with news. It's a game large firms can't lose, and the lonely retail investor seems to have no chance. Since the March 2020 covid flash-crash, the stock market has been very bullish, with quantitative easing adding so much new money into the market, gains seem to be popping up everywhere. It may be a good time to enter the market with a bullish strategy or maybe this is the time to prepare for when the market will eventually reconcile?

## Automation

Nowadays, without the help of automation and algorithmic trading, retail investors face a lot of manual data analysis in choosing what company is the best bet, deciding a target, stop loss, and tracking their portfolio. Thankfully there are websites like Finviz and Yahoo Finance (if you have access to a computer and internet), and if you know how to code there are their Application Programming Interface (API's). Thank you to the maintainers of these repos: https://github.com/mariostoev/finviz, https://github.com/ranaroussi/yfinance.

## Stock Analysis

Some of the questions I wanted to address here are the following:

- What companies stock are trending upward?
- What companies have the best balance sheet?
- What does the visualization of historic price action show?

I used the Finviz screener API to collect a list of stocks, filtering on average volume (over 10000), share price (under $50), and country (Canada). These companies are also option-able and short-able. I had a list of 150 symbols and I decided to use the Yahoo Finance API to fetch their open/low/high/close historical data. I then found the distribution of percent change for the total, five-year, one-year, three-month, and one-month periods and the heat map of correlation.
                        
![Gain distribution](https://github.com/philip-L/react-blog/blob/main/public/static/img/gaindist.png?raw=true)

![Gain heatmap](https://github.com/philip-L/react-blog/blob/main/public/static/img/gainheatmap.png?raw=trueg)

 The histograms show that the market has positive bias. Most of these companies have positive gain over time. From the heat map, the most recent periods are more related (trends are local), and longer periods have small sometimes negative correlation which could hint at mean reversion. These visualization confirm basic, fundamental knowledge that all investors should know: market positive bias and mean reversion.

To create a short list of potential buys, I chose stocks with positive gain greater than 342% (the 75th percentile) overall, and gain greater than 1% in the last month. This gives me a list of 18 companies.                        

## Intrinsic Value, F and Z-score

Next, some value investing, because it is the most fundamental (pun intended). Here are some definitions:

- Intrinsic value: a measure of the true worth of a company's share
- Altman Z-Score = 1.2A + 1.4B + 3.3C + 0.6D + 1.0E. A score below 1.8 means it's likely the company is headed for bankruptcy. Scores above 3 are not likely to go bankrupt. Where:

  - A = working capital / total assets
  - B = retained earnings / total assets
  - C = earnings before interest and tax / total assets
  - D = market value of equity / total liabilities
  - E = sales / total assets

- Piotroski F-score: health of a company's balance sheet. Best value = 9

Profitability criteria:

- Positive net income (1 point)
- Positive return on assets in the current year (1 point)
- Positive operating cash flow in the current year (1 point)
- Cash flow from operations being greater than net Income (quality of earnings) (1 point)

Leverage, liquidity, and source of funds criteria:

- Lower ratio of long term debt in the current period, compared to the previous year (decreased leverage) (1 point)
- Higher current ratio this year compared to the previous year (more liquidity) (1 point)
- No new shares were issued in the last year (lack of dilution) (1 point)

Operating efficiency criteria:

- A higher gross margin compared to the previous year (1 point)
- A higher asset turnover ratio compared to the previous year (1 point)

First, I find companies that have twice as many current assets compared to current liabilities. (current ratio = 2), This is the result:

![Current ratio](https://github.com/philip-L/react-blog/blob/main/public/static/img/currentratio.png?raw=true)

Next, I want to use Z and F-Score (see descriptions above from Investopedia.com), but the data I have is missing some of the necessary values, thus I simply calculate intrinsic value with two methods: using the price/earnings ratio and with Benjamin Graham's method. The goal here is to find the best valued company, therefore I filter with intrinsic value to find the companies which are undervalued. The result is that Kirkland Lake Gold Ltd. is the only one left.

## Charting and Technical Analysis

The most basic form is the use of moving-averages (MA's). When the price is above a MA it could be considered bullish, and below it is bearish. Additionally, I use simple Bollinger Bands which are an indication of a possible break out of a channel. I do a final check to see how the graph looks. From top to bottom, the chart display three-month, one-year and total history.

![Chart](https://github.com/philip-L/react-blog/blob/main/public/static/img/chart.png?raw=true)

## Conclusion

Since graduating from University, I wanted to develop a trading strategy for myself to help remove emotions and improve my investment decision making. I believe everyone should have access to tools to automate their investing strategy and I found there are a lot of resources to explore on Github. My github repo for this project can be found [here](https://github.com/philip-L/yahoo-trading/). Other data that could be useful to investigate in the future: options trading, news analysis, and insider buying/selling. If you made it this far, congrats and thanks for reading.
