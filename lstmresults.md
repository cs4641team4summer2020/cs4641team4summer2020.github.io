Using LSTMS, we attempted to create a model that could learn from early capacity measurements (first 60% of cycles) of a single battery (0005) and effectively predict its remaining useful life. We developed two LSTM different models to this purpose. 

# Prediction of Capacity Reduction over Time with LSTM

In this model, we used the past capacity curve to predict the future capacity curve (over time). This was based on current literature in the field [Y Zhang et al]. We trained on 60% of the capacity data and used the resulting model to predict the curve for the remaining 40% of cycles. In order to train the LSTM, we modified the shape of the input data for supervised learning using time steps of 3 cycles. The i'th data item in our training data would have 3 features: capacity at cycle i, i-1, and i-2, and a truth value: capacity at cycle i+1. The model is used to predict a capacity for the next cycle, and this predicted value is iteratively fed back into the model to predict even further into the future. The point where the predicted curve hits 1.4 Ah is the predicted Remaining Useful Life. Our model contained an LSTM layer of 50 nodes followed by a dense layer of one node. We trained it for 600 epochs using the RMSProp optimizer. We found that adding anymore layers/nodes/epochs made the model susceptible to overtraining. Below, we show our result. Our model predicts an RUL of 106 while the real RUL is 124. RSME = 0.04 Ah

![LSTM Capacity 1](/images/LSTM1Capacity.jpg)

While this technique was very intuitive and supported by literature, it does not directly predict our desired goal: the Remaining Useful Life; instead, it predicts capacity. Therefore, we aren't optimizing for the right RMSE (e.g. we optimize past models that predict RUL very well but have poor capacity RMSE). To fix this, we tried an alternative approach given below.

# Direct Prediction of RUL over Time with LSTM

For this LSTM model, we used past capacity data to directly predict the RUL. We used a technique known as walk-forward validation. We train on all the capacity data up till the "current" cycle (in time steps of 3 cycles), and predict the RUL from the current cycle. Every time we "recieve" new data (move to the next cycle), we retrain on the new data and make a better prediction. Below is a graph of our predicted RUL at each cycle compared to the real value (starting from training on 60% of the cycles). For this model, we found that we benefitted from stacking an extra LSTM layer of 50 nodes and adding dropout layers. Dropout layers randomly remove a fraction (0.2) of the network nodes to prevent overfitting. We trained for 1000 epochs using the RMSProp optimizer and ran it till the failure point (RUL=0). RMSE = 11 cycles

![LSTM RUL](/images/LSTM2RUL.jpg)

It is interesting to note that the model consistently errs at about +10 above the real RUL. This is probably because the model is overfitting to the small spikes in the capacity vs time curve and overestimating future capacity. Also, as expected, RUL prediction seems to get better as we get closer to the point of failure. Below we show our RMSE loss over training time.

![LSTM Training](/images/LSTMTraining.JPG)

We also created a visualization where we used our predicted RUL at each cycle to calculate a predicted capacity for the consequent cycle as $$C_{i+1} = C_i - (C_i - 1.4) / RUL$$. We ran this till the failure point.

![LSTM Capacity 2](/images/LSTM2Capacity.JPG)

Its important to note that this visualization seems to do much better than the first model only because it is predicting only one cycle into the future.
