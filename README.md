#Trading Problem Overview
This problem requires a mix of statistics and data analysis skills to create a predictive model using financial data. We will provide you with a toolbox and historical data to develop and test your strategy. Get the toolbox and data here [].

##Problem 2.a
You are given a list of securities to trade.  Create a prediction model that uses the daily price data provided and returns a daily signal for every security: +1 for buy, -1 for sell or 0 for no opinion.
Use file problem2.py provided with the toolbox for this problem and complete the function trading_strategy().

###Input: 
You are given 5 years of daily price and volume data of 50 stocks.To help you get started with the challenge, we will also provide you with a set of features. (The list of features only serve as an introduction and are not exhaustive, you can create new features from the given price data). Read more about the data format here [link to data ].

###Output format:
Your code’s final output should be pandas dataframe with security symbol as rows and *SIGNAL* as columns:

|Security |SIGNAL|
|AAPL|1|
|GOOG|-1|

###Scoring:
The objective of this problem is to maximize prediction efficiency.  A prediction is correct if you label a security as a buyon day t and open price on day t+1 is greater than previous day’s open price. Similarly, if you label a security as sell, it is correct if open price on day t+1 is lower than previous day’s open price
Your score on any day will be

Score = Sum(Signal (t) (Open Price (t+1) - Open Price(t))


To discourage overfitting, you are provided a subset of data for backtesting.  Your final score will be calculated over complete data set ( performance over backtested data and test data which is not available for download).

Final points for this problem are relative. i.e. the contestant with highest score gets 40 points. 

40 points

##Problem 2.b
You are now given $1,000,000 as starting funds and the same list of securities to trade. Use the prediction model from problem 2a  to develop a trading strategy which decides how to allocate funds to each security. For every security, you have to return a trading signal and it’s weight in the portfolio.
Use file problem3.py provided with the toolbox for this problem and complete the function trading_strategy().

###Input:
You are given the same input as problem 2a,  5 years of daily price and volume data of 50 stocks and a set of features. (The list of features only serve as an introduction and are not exhaustive, you can create new features from the given price data). Read more about the data format here [link to data ].

Order executions happen at open price every day. The toolbox applies a commision of 10 cents per trade. Read more about execution and backtesting here [ ].

###Output format:
Your code’s final output should be pandas dataframe with security symbol as rows and *SIGNAL* and *WEIGHTS* as columns. The weights, which are a number between 0 to 1, define allocation of funds to that security:

|Security |SIGNAL|WEIGHTS|
|AAPL|1|55%|
|GOOG|-1|45%|

Here, if we have $1,000,000 in funds, we buy $550,000 worth of AAPl shares and sell $450,000 worth of GOOG shares.

The weights don’t have to sum to 1. If they sum to < 1, the remaining funds are available as cash. If they sum to > 1, the weights are renormalized so that they sum to 1.

At any given time t, your model should only use data from time T=0 to T=t. Our toolbox is designed to prevent you from accessing data beyond the timestamp for which you are predicting.

If  you run out of available funds, you will stop trading (i.e. total losses > initial budget).

###Scoring:
The objective of this problem is to maximize risk adjusted Profits. For this problem, we will score you on the Sharpe Ratio of your strategy i.e.:

Score: Sharpe Ratio = Annualized Return/ Annualized Volatility

To discourage overfitting, you are provided a subset of data for backtesting.  Your final score will be calculated over complete data set ( performance over backtested data and test data which is not available for download).

If two strategies tie, we will use the profit factor as a tie breaker, which is
Profit Factor: Sum of Profits/Sum of Losses

Final points for this problem are relative. I.e. the contestant with highest score gets 45 points. 

45 points

#Other information:

##Data Format:
Date| Bid Volume|Bid Price|Ask Price|Ask Volume|F1|F2|F3|F4...

1) TimeStamp: Date and time at which the information is given
2)Bid Volume: Total size that the market is willing to buy, i.e., total size you can sell to the market
3)Bid Price: The highest price that the market is willing to buy, i.e., The best price that you can sell the security at.
4)Ask Volume: Total size that the market is willing to sell, i.e. total size you can buy from the market
5)Ask Price: The lowest price that the market is willing to sell,i.e. The best price that you can buy the security at.
6)Feature 1: VWAP: Volume Weighted Average Price: Bid Volume*BidPrice+AskVolume*AskPrice/BidVolume+AskVolume
7)Feature 2: 30d Moving Average
8) Feature 3: 90d Moving Average
9) Feature 4: 90d Standard Deviation
10) Feature 5: Momentum
11) Feature 6: RSI indicator
12) Feature 7: Average True Range 

 
##Execution:
An execution is the completion of a buy or sell order for a security. Once you provide your list of trades, our code will trade them for you. You can treat the execution code as a black box.
Trade executions happen at next day’s open price. It will calculate the quantity of each security you will trade based on portfolio weights  and available funds net of trading costs. For every trade, there will be a commission charge of 10 cents. 

Example, you want to buy a security and keep portfolio weight of 50%. The open price is 99. If your portfolio value before this trade is 1000$, you buy 5 lots at 99$ and are charged 5*0.1 = 0.50$ for it. Your new portfolio value is 999.50 and available funds will be 1000-5*99 - 5*0.1 = 504.5$.

The toolbox will automatically rebalance the portfolio for you daily. In the above example, if the value of security rises to $139 and you make a profit so that your portfolio value is $1199.50 the system sells one share at 139 to maintain 50% portfolio weight.  Your new portfolio value is $1199.40 and your available funds are 504.5+139-0.1 = 643.40

##Backtesting and Debugging:
Backtesting is the process of evaluating a trading strategy’s performance on historical data. You can use the backtesting tool provided by us to evaluate the performance of your code on historical data and examine how your model’s performance changes as you make changes to any model parameters. 
The toolbox charts your performance and provides detailed logs in a csv file. You can visualize the performance of your strategy over different time periods, compare it with the performance of a benchmark index, inspect your daily trades and positions in each security and analyze how your model performs on some key metrics. Our backtesting tool will display the following metrics.

Total Return: The Profit (or Loss) from the model in the given time period, specified as a percentage of the initial available funds.

Annualized  Return: The average % Profit(or Loss) from the model in a year.

Annualized Volatility: The dispersion of daily returns of the model in a year. Volatility is used as a measure of risk, therefore higher vol implies riskier model.

Sharpe Ratio: Risk adjusted Returns, calculated as Annualized Return/Annualized Standard Deviation

Sortino Ratio: Returns adjusted for downside risk, calculated as Annualized Return/Annualized Standard Deviation of Negative Returns

Max Drawdown: Maximum negative difference in Total Portfolio Value. It is calculated as the maximum high to subsequent low difference before a new high is reached 

Profit Factor: Sum of Profits from trades that results in profits/Sum of losses from trades that results in losses

% Profitability = Sum of Profits from trades that results in profits/Total PnL