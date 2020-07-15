We trained 2 LSTM models for different purposes relating to Lithium Ion Life Forecasting. 
1. Predicting Capacity Reduction over Time
For this model, we predicted future capacity directly. This was based on literature that oultined a technique. We trained on 60% of the data and used the resulting model to predict the curve of capacity over time for the remaining 40% of cycles. The point where the predicted curve hit 1.4 Ah would be the predicted Remaining Useful Life. Below, we show our result,
![RMSE vs. number of components](/images/LSTM1Capacity.jpg)
