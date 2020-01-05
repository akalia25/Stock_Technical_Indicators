# Technical Analysis on stock's historical data

Google Doc: \https://docs.google.com/document/d/1T6dNP2ZhvYJJRiygnX3O2w3YTFCHT3SoZN62algWVwk/edit?usp=sharing

## Introduction
In this assignment our group was given the DJI-30 Minutes and NASDAQ 30 Minutes data in addition to AAPL daily (January 1st 2017-March 15th 2019) data. We created technical indicators to track various SMAs and EMAs, as well as RSI and Bollinger Bands for each stock. We decided to use Price Channels as our fifth technical indicator because they can be used to identify trend reversals that denote pullbacks within a bigger trend. The following sections describe the steps taken to achieve these results.

## Relative Strength Index (RSI)
The relative strength index is a momentum oscillator that measures the speed and change of price movements. The calculation takes into account the previous 14 periods of data, and compares how many of those days the price went up or down. The formula for RSI is as follows:
RSI=100-1001-Avg GainAvg Loss
The RSI is bound between 0 and 100.

### Analysis:
In technical analysis using RSI there are 2 basic method that indicate a buy or sell signal. For a buy signal, the following condition must be met; the RSI crosses above 30, and was below 30 within 5 recent periods. This indicates that the stock has been “oversold” and that the stock is undervalued. Similarly, a sell signal is defined by when the RSI crosses below 70, and was above 70 within 5 recent periods. This indicates that the stock has been overbought, and the price is likely to go down. 

Since the AAPL data was formatted differently than the other two stocks, two separate functions were required to create the RSI sell/buy signals. With AAPL, the data was in chronological order (oldest first), while DJI and NASDAQ were in reverse chronological order (newest first). This difference affects how you calculate the ROI and rolling averages in Python. To overcome this issue, the DJI and NASDAQ must be treated in the reverse order when calculating the average number of up and down periods (to ensure we are moving forward in time, not backward). After this step is complete, all three stocks calculate RSI in the same manner. The Relative Strength is calculated by dividing the average of up periods by the average of down periods. Then the RSI can be calculated using the formula above for each period of data. Finally, we must create 2 new columns for buy and sell signals, and then populate those based on the rules described above. 

### Exit Signal:
The exit strategy for RSI is that if the stock goes above 80 or below 20, we reverse our positions. For example, if the RSI goes above 70 and you short sell, and then some time later it goes above 80 you would buy (exit the short), because the trend is likely to have reversed and the price dropped as others have sold. Similarly, if the RSI drops below 30 and you buy shares because it is oversold, you would then sell those shares if the price goes below 20. An RSI of less than 20 is an indication that the stock is too oversold, and the price is going to drop.

### Results:
The following figure is the plot created for AAPLs stock history that shows the Normalized Price and RSI over time.

As you can see, the RSI frequently oscillated between values less than 20 and greater than 80. Moreover, the RSI indicator would be a decent representation of how the stock may perform. On the left side of the chart, the price usually went up after the RSI hit a local minimum, and went down shortly after it hit its local peak. This falls apart midway through 2018, when the AAPL stock takes a dive. By the time RSI is at 30, the value of the stock has already plummeted. 

## Bollinger Bands
### Analysis:
Developed by John Bollinger, Bollinger Bands® are volatility bands placed above and below a moving average. Volatility is based on the standard deviation, which changes as volatility increases and decreases. The bands automatically widen when volatility increases and contract when volatility decreases. 

With a setting of Bollinger Bands of length 20 and standard deviation of 2, Bollinger Bands have the following calculation:

Middle Band = 20-day simple moving average (SMA) of close price
Upper Band = 20-day SMA + (20-day standard deviation of price x 2) 
Lower Band = 20-day SMA - (20-day standard deviation of price x 2)

If the closing price crosses below upper band and was above upper band within 5 recent periods, it would be a sell signal. On the other hand, if closing price crosses above lowerband and was below lowerband within 5 recent periods it is a buy signal. 

The graph below illustrates Bollinger Bands, buy and sell signal.


In our python code, we have three functions regarding Bollinger Band.

def Bollinger(df): This function calculates Upper,Middle and lower band for Bollinger. It also indicates sell/buy signal and ROI (Price[t]/price[t-1]-1)

def BollingerPlot(df,pltname): This function plot Bollinger Band 

def bollingerOutputCSV(df,file_name): This function output Bollinger Band dataframe into .csv file

### Exit signal:
Exit signal for Bollinger Band is that if a stock has more than 20% profit or 10% loss, the exit signal would be generated. There's an old expression, "You don't go broke taking a profit.". This rules make sure trader exit the position when profit reaches 20%. It would avoid traders wait in the market expecting to get more profit while the market drops. It makes sure trader take some profit but not necessary all of it. This rule also guarantee that trader avoid a bigger loss than 10%. It stops trader keep the position and hope the market to go up. There is a high chance this waiting results in a bigger loss. 

We assume that we cannot long or short position in two consecutive periods. For example, on both June 18th and June 19th, Bollinger band gave us buy signals. If we are not holding any position before June 17th, we can long the position on June 18th. However, on June 19th we can not long the position. We will exit on June 18th and wait for next buy or sell signal.  

### Result:
For all three stocks (DJI-30 Minutes, NASDAQ 30 Minutes and AAPL), we output Bollinger Band result with following format:
Close price
ROI (Price[t]/price[t-1]-1)
BB_lower
BB_upper
BB_middle
Sellsignal : Closing price crosses below upperband and was above upperband within 5 recent periods (0 or 1). 1 is taking action on the sell
Buysignal: Closing price crosses above lowerband and was below lowerband within 5 recent periods (0 or 1). 1 is taking action on the buy
Exit signal: if there is 20% profit or 10% loss (0 or 1). 1 is taking action to exit.
A graph for each stock is also output as below:
The band is highlighted in orange. Bold blue line is normalized historical close price.

AAPL:

DJI:

Nasdaq:

## Price Channel
### Analysis:
Price Channels are lines set above and below the price of a security. The upper channel is set at the x-period high and the lower channel is set at the x-period low. For a 20-day Price Channel, the upper channel would equal the 20-day high and the lower channel would equal the 20-day low. Price Channels can be used to identify upward thrusts that signal the start of an uptrend or downward plunges that signal the start of a downtrend. Price Channels can also be used to identify overbought or oversold levels within a bigger downtrend or uptrend.

With a price channel of 20 days, price channel has the following calculation:

Upper Channel Line: 20-day high
Lower Channel Line: 20-day low
Centerline: (20-day high + 20-day low)/2

The Price Channel formula does not include the most recent period. Price Channels are based on prices prior to the current period. A 20-day Price Channel for October 21 would be based on the 20-day high and 20-day low ending the day before, October 20. A channel break would not be possible if the most recent period was used.

If closing price breaks out upper Band, it is a buy signal. If closing price breaks out lower band, it is a sell signal. Following chart represents potential buy/ sell signal and price channel:

In our python code, we have three functions regarding Price Channel.

def PriceChannel(df): This function calculates Upper,Middle and lower band for Price channel. It calculates sell/buy signal of price channel. 

def BollingerPlot(df,pltname): Price channel has the same data frame format as Bollinger Band. Thus, these two indicators share the same function to plot the graph.

def PriceChannelOutputCSV(df,file_name): This function output Price Channel dataframe into .csv file

### Exit signal:
Exit signal for price channel follows the same logic as Bollinger band. If a stock has more than 20% profit or 10% loss, the exit signal would be generated. 

We also assume that we cannot long or short position in two consecutive periods. For example, on both June 18th and June 19th, Price Channel gave us buy signals. If we are not holding any position before June 17th, we can long the position on June 18th. However, on June 19th we can not long the position. We will exit on June 18th and wait for next buy or sell signal.  

### Result:
For all three stocks (DJI-30 Minutes, NASDAQ 30 Minutes and AAPL), we output Price Channel result with following format:
Close price
Lower Bound
Upper Bound
Middle Bound
Sellsignal :  Closing price crosses above lowerband and was below lowerband (0 or 1). 1 is taking action on the sell
Buysignal: Closing price crosses below upperband and was above upperband(0 or 1). 1 is taking action on the buy


A graph for each stock is also output as below:
The band is highlighted in orange. Bold blue line is normalized historical close price.
AAPL:

DJI:

NASDAQ:

## Simple Moving Average (SMA)
### Analysis:
The simple moving technique is very commonly used technique in the stock market, due it being easy to implement and providing great insights. The simple moving average only takes two inputs, the time series data (closing price, date) as well as the moving average window. For this assignment moving average windows of 5,10,15 were selected. The moving average window specifies how many data points minus 1, to take into consideration when predicting. A moving average window of 5 would take 4 data points to predict the 5, 10 would take 9 and 15 would take 14. To calculate the MA for “n” data points you take the demand at each value of “n” and take the sum of it. Once you have the sum of the demand, you divide it by “n” to get the the predicted moving average for the “n” th point. To calculate the moving average, the function “data.rolling(window = i).mean()” , where i is the number of steps for the moving average (5,10,15). The reference “data” object is a time series object with stock closing price, and date. The formula shows as following:
MAn=i=1nDin
  
Where, 
N = number of periods in the moving average
Di =demand in period i

### Exit Signal:
For the simple moving average a commodity channel index indicator is being applied to determine whether the user should exit the stock. The commodity channel index works by determining if the stock is being overbought or oversold. It uses the parameters, such as stock high, low, and close prices. The signal works by taking the average of the high, low, and close price of the stock.

Typical Price=(High + Low + Close) / 3

Once the average of the high, low, and close price is calculated, it is subtracted by a rolling moving average of a specified time period. In this case a time period of 20 days is used to capture both short term and long term changes to the stock. This is then divided by the standard deviation of the typical price, and a factor of 0.015 is used to provide a more reliable commodity channel index. 

CCI=(Typical Price - MA of Typical Price) / 0.015 * STD (Typical Price)
The channel commodity index results in wide range of values, when the CCI results in values that are below -100, this implies that the stock is being oversold. In contrast if the CCI value is greater than 100 then the stock is overbought. We can use these indicators to exit the stock at the correct time, when a stock is being overbought or oversold, the stock generally corrects itself to get out of being oversold or overbought. Therefore, by knowing this, if we have a position of a long in a stock, and it is found to be overbought, then we can conclude the stock will try to balance itself and the price will drop, hence indicating a good time to exit. The same strategy applies when the user is in a short position. If the user is in a short position and the stock is found to be oversold, then we can conclude that the stock will try to balance itself and raise its price, indicating a good time to exit. 

### Result:
From the graphs it can be concluded that a higher moving average window is able to capture consistent growth better, if a stock is stable for a long period of time it is better to use a longer period for moving average. However if the stock is really volatile (i.e AAPLdata) it is better to use a smaller moving average to capture any sudden changes in the stock price, and have a prediction that is more heavily based on recent data, rather than a longer period. 

## Exponential Moving Average (EMA)
### Analysis:
Exponential moving average works by applying a weight factor to the data points. The weight factors work by assigning more weight (value) to the closer/most recent data points, while decreasing the weights of the further data points exponentially. To implement exponential moving average in the code for (5,10,15) periods, the pandas function “data.ewm(span = i , adjust = False).mean()" where i is equal to the different specified periods (5,10,15), in addition, “adjust = False” is set to ensure a recursive calculation. 

The calculation of EMA shows as following:
EMA(t)=(1-)EMA(t-1)+p(t)
EMA(t0)=P(t0)

Where p(t) is the price at time t and ɑ is called the decay parameter for the EMA. ɑ is related to the lag as
=1L+1

And the length of the window(span) M as
=2M+1

### Exit Signal:
For the exponential moving average, the commodity channel index is also applied. This exit signal strategy is applied for simple moving average as well. The reason this strategy is used for both exponential and simple moving average is because of its ability to find cyclic trends in stock prices. Furthermore, since both moving average and exponential average are heavily dependent on the stocks previous data, the commodity channel index works well by identifying when to exit. 

### Result:
The key differences between SMA and EMA, is that by assigning a weight higher to closer data points, EMA is able to effectively respond to changing prices faster than SMA. The higher the EMA lag value (i), the better it is for considering a trend that has a consistent trend (either upward, downward, horizontal) . The lower the lag value (i) the more focus on the most recent data points, this is good for a volatile stock thats current price is heavily based on its last closing price. 

## Machine Learning Based Strategy
### Analysis:
For the machine learning based strategy, the two technical indicators combined are “Simple Moving Average” and “RSI”. For each period of data, a column was added to the dataframe that contained a binary value representing if the ROI from the last period was positive or not. Since this data was already classified, a supervised machine learning algorithm was a suitable choice. A classification based learner was implemented to combine both the technical indicators, implementing a k-nearest neighbors (KNN) algorithm.

The KNN algorithm looks at the classified historical data to predict which class a new datapoint should have. The algorithm takes a parameter, k, as the number of neighbors to use when classifying the new data point. Given a new data point, KNN looks at the k nearest neighbors and takes a vote to classify this data point. The vote is won by the majority (see figure below). For this strategy we are training the KNN algorithm with 80% of our data and testing the strategy on 20%. 

The figure above shows how the KNN algorithm works with an illustration. It looks at the new data point and decides to classify it as Class A or Class B. From the 20% being tested on, we are able to compare if the KNN algorithm is able to correctly classify the data point or not, and from this we are able to compute accuracy levels for the different combinations of the technical strategies. 

Accuracy=(TP +TN) / (TP+TN+FP+FN)
Where TP = True Positive, TN = True Negative , FP = False Positive, FN = False Negative

The accuracy score is calculated with the following formula above. A true positive and true negative are correct classifications that the KNN algorithm makes which is then divided by all the predictions made by the KNN algorithm, resulting in the accuracy score of the KNN algorithm. 

Results    
Indicator
Accuracy Score
RSI & SMA5
0.610039
RSI & SMA10
0.624196
RSI & SMA15                
0.604891
RSI & SMA10/SMA5           
0.611326
RSI & SMA15/SMA10          
0.603604
RSI & SMA5/Close(t)        
0.619048
RSI & SMA10/Close(t)       
0.621622
RSI & SMA15/Close(t)       
0.612613
RSI/Close & SMA5           
0.640927
RSI/Close & SMA10          
0.629344

### Result:
From the results of the KNN algorithm the best combination of technical indicators is using RSI/Close & SMA 5. Essentially you are taking the RSI value and dividing it with the close price of the stock for that day and combining that with the simple moving average of 5 terms. This strategy yields a 0.64 accuracy score showcasing how more than half of the times it is able to correctly identify which classification the new data point should belong in. 



