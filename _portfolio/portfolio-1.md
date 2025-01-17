---
title: "Hoa Phat Stock Price Prediction (HOSE: HPG)"
excerpt: "**Hoa Phat Stock Price Prediction** (HOSE: HPG) leverages historical data and the ARIMA model to forecast stock prices for Hoa Phat Group's stock, using historical data. It provides insights for investors while visualizing stock performance on an interactive chart.
<br/><img src='/images/HoaPhat.jpg'>"
collection: portfolio
---

## Overview - Business Problem

Investors in the stock market constantly seek methods to predict future stock prices to make informed decisions and maximize returns. Accurate stock price prediction models can provide significant advantages by anticipating market movements and identifying profitable investment opportunities. This project addresses the need for reliable stock price prediction models by focusing on the stock prices of Hoà Phát Group (HPG), one of the leading companies listed on the Ho Chi Minh Stock Exchange (HOSE). Given the inherent volatility and complexity of the stock market, traditional methods of stock analysis may not always yield accurate predictions. Therefore, developing a robust predictive model is crucial.

![](/images/HoaPhat.jpg)

## Project Structure


- **Step 1 (Data Collection):** Utilizes the vnstock library to retrieve historical stock price data for HPG.
- **Step 2 (ARIMA Model Building):** Preprocesses the data and constructs an ARIMA model for stock price prediction.
- **Step 3 (Model Evaluation)**: Employs the Root Mean Squared Error (RMSE) metric to assess the model's performance.
- **Step 4 (Conclusions and Recommendations):** Provides insights into the model's effectiveness and suggests directions for future research.

## System Requirements
- Python 3.6+
- Python Libraries
    - vnstock
    - pandas
    - numpy
    - matplotlib
    - statsmodels

## Using the Notebook

Open and run each cell in the [Stock_Price_Prediction.ipynb](https://github.com/chinneee/Stock-Price-Prediction/blob/main/Stock_Price_Pediction.ipynb) notebook to:
- Collect historical stock price data for HPG.\
- Preprocess the data and build an ARIMA model.\
- Evaluate the predictive model.

## Results

The predictive model was evaluated on the historical HPG stock price dataset with an **RMSE of 1764.22 VND**. The results indicate that the model has the potential to assist investors in making investment decisions.

![](/images/prediction.png)
## Limitations and Recommendations

- The model has some limitations:

    - Historical data does not fully reflect all factors that affect future stock prices.
    - The ARIMA model is not suitable for all cases and needs to be adjusted for each specific case.
- Recommendations for further research:

    - Use advanced machine learning models such as Artificial Neural Networks (ANN) or Deep Learning.
    - Incorporate multiple data sources such as news, market analysis, and economic data.
    - Evaluate the model on different datasets to confirm its stability and effectiveness.

<a href="https://github.com/chinneee/Python--Stock_Price_Prediction" style="display: inline-block; padding: 10px 15px; background-color: #CCCCFF; color: Black; text-decoration: none; border-radius: 5px;">
  <img src="https://pngimg.com/uploads/github/github_PNG40.png" alt="GitHub logo" style="width: 30px; height: 30px; vertical-align: middle; margin-right: 8px;">
  Explore the full project on GitHub
</a>