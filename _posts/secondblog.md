---
date: '2021-11-11T11:50:54.000Z'
title: SIMPLE STOCK PRICE INDICATOR WITH KERAS AND YAHOO FINANCE API
tagline: November 11, 2021
preview: >-
  I believe fundamentals are the most important, but there is much more to it than that.
image: >-
  https://github.com/philip-L/react-blog/blob/main/public/static/img/model_chart.png?raw=true
---

## Intro

I believe fundamentals are most important, however there is more to it than that. I want to have a safe, conservative strategy to be able to set and forget, hold and go long. Contrarily, I believe trends or swings in stock price might be a better way to make profits in the short-term. Additionally, Initial Public Offering's (IPO's) can have wild swings, but it seems only insiders are able to profit since company data is not yet public.

Traditional methods like certain technical indicators and polynomial regression are becoming outdated, and machine learning (ML) models can be used to replace these. In this report, I discuss a ML method for a stock price indicator using the past 60 days to predict the following day. This project uses the python yfinance package to collect historical price for a list of stock symbols. Code is contained in a jupyter notebook found on my github [here]("https://github.com/philip-L/AlgoTrading/"). I follow the same approach found in the blog by Jason Brownlee at his website [here]("https://machinelearningmastery.com/time-series-prediction-lstm-recurrent-neural-networks-python-keras/") and watching youtube videos to help.
                    

## Problem Statement

The subject of this post is to predict the closing price of a stock symbol n days into the future based on of m days of past data. In the case of IPO's, data is limited and simply the previous day's closing price can be used (the first day)IPO price   In this project I wanted to explore the Long Short-Term Memory (LSTM) recurrent artificial neural network, which is powerful in time-series prediction problems. I discovered this method in the following tutorial on youtube: [here]("https://www.youtube.com/watch?v=QIUxPv5PJOY&ab_channel=ComputerScience"). Based on watching the video I expect the predictions of the model to be close, but not totally accurate, and should not be taken as real investing advice by itself.

## Metric

The metric used to measure the model's performance here is the root-mean-squared error (RMSE). The RMSE is a popular metric used to measure the difference between actual and predicted values. It is a good metric to use for a regression problem like mean-squared error (MSE) or mean-absolute error (MAE).
                    

## Get Data

The input data is the open/low/high/close (OLHC) and volume for a company's public share price since the company went public. Only the closing price is used in the model. Below is a statistical data visualization of the closing price for ticker symbol "ANY" using pandas DataFrame describe() method:

![ANY close price](https://github.com/philip-L/react-blog/blob/main/public/static/img/any_closeprice.png?raw=true)

## Methodology

The data is scaled to values between 0 and 1 using sklearn's MinMaxScaler and is split into training and testing sets (80% and 20% respectively). In this problem the data needs to be split further into x and y sets, where x is an array of arrays for the closing price for m days in the past (60 data points is chosen) and y is an array of the closing price one day later. With this prepared data, I use built in keras (with tensorflow) methods to build, compile, and fit a Sequential model with LSTM and Dense layers to train on the x and y datasets. With this trained model, I predict using the testing data and calculate RMSE as a metric of difference between predicted and actual values. For refinement, I wanted to try to lower RMSE. The model originally had four layers, two LSTM layers and two Dense layers. The RMSE was ~16, and removing one of the Dense layers in the model improved RMSE to ~11.          

## Keras Model

The Sequential() Keras model is used with the following layers:

- LSTM(50, return_sequences = True, input_shape = (x_train.shape[1], 1)): output dimensionality of 50 units and returns the full sequence in the output sequence, also specify input shape
- LSTM(50, return_sequences = False): output dimensionality of 50 units, return the last output in the output sequence only
- Dense(1): regular densely-connected neural-network (NN) layer with output being the scaled predited value

## Results

The following plot shows the historical closing price of ticker symbol "ANY" with training, testing and predicted values labeled in the legend. The model predicts values close but slightly lower than actual values. RMSE is used to validate the model’s solution as described above and overall it is a nice result, therefore no other model, parameters, or techniques are shown or compared.

![model chart](https://github.com/philip-L/react-blog/blob/main/public/static/img/model_chart.png?raw=true)                    

The model (LSTM) is a type of recurrent neural network designed to handle sequence dependencies and is ideal for time-series prediction problems. It is trained using Backpropagation Through Time and overcomes the [vanishing gradient problem]("https://en.wikipedia.org/wiki/Vanishing_gradient_problem"). It is a good choice for any linear time-series regression problems.
                    
## Conclusion

In conclusion, the LSTM is just what I needed for this problem, however it could further be explored more in depth to understand the optimizer, loss function and the layers of the model. I could add the open/high/low and volume data to the model and try predicting into the future where there is no valid data to test against. It was interesting how just three layers in the model could produce effective results. Some other possible areas I want to explore: use finviz news and insider buying/selling as an indicator. Also I want to calculate percent gain for every previous year (X) and predict current year gain (y).
                    