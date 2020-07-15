In the LSTM part of this project, we attempted to create a model that could learn from early capacity measurements (first 60% of cycles) from a single battery (0005) and effectively predict its remaining useful life. We developed two LSTM different models to this purpose. 

# Prediction of Capacity Reduction over Time with LSTM

In this model, we used the past capacity curve to predict the future capacity curve (over time). This was based on current literature in the field [1]. We trained on 60% of the capacity vs time data and used the resulting model to predict the curve for the remaining 40% of cycles. In order to train the LSTM, we modified the shape of the input data to represent supervised learning by creating time steps of 3 cycles. Each data item, say for the i'th cycle, contained the capacity at cycle i, i-1, and i-2 as features, and its truth value was the capacity at cycle i+1. The latest predicted value is iteratively fed back into the model to predict even further into the future. The point where the predicted curve hits 1.4 Ah is the predicted Remaining Useful Life. The LSTM contained an LSTM layer of 50 nodes followed by a dense layer condensing it to one node and we trained it for 600 epochs using the RMSProp optimizer. We found that adding anymore layers/nodes/epochs made the model susceptible to overtraining. Below, we show our result. Our model predicts an RUL of 106 while the real RUL is 124. RSME = 0.04 Ah

![LSTM Capacity 1](/images/LSTM1Capacity.jpg)

While this technique was very intuitive and supported by literature, it does not directly predict our desired goal: the Remaining Useful Life; instead, it predicts capacity. Therefore, we aren't optimizing for the right RMSE (e.g. we optimize past models that predict RUL very well but have poor capacity RMSE). To fix this, we tried an alternative approach given below.

# Direct Prediction of RUL over Time with LSTM

For this LSTM model, we used past data to directly predict the RUL. We used a technique known as walk-forward validation. We train on all the capacity data up till the "current" cycle (in time steps of 3 cycles), and predict the RUL from the current cycle. Every time we "recieve" new data (move to the next cycle), we retrain on the new data and make a better prediction. Below is a graph of our predicted RUL at each cycle compared to the real value (starting from training on 60% of the cycles). For this model, we found that we benefitted from stacking an extra LSTM layer of 50 nodes and adding dropout layers. Dropout layers randomly remove a fraction of the network nodes to prevent overfitting. We trained for 1000 epochs using the RMSProp optimizer. RMSE = 11 cycles

![LSTM RUL](/images/LSTM2RUL.jpg)

It is interesting to note that the model consistently errs at about +10 above the real RUL. This is probably because the model is overfitting to the minute spikes in the capacity curve and overestimating future capacity. Also, as expected, RUL prediction seems to get better as we get closer to the RUL. Below is our RMSE loss over training time.

![LSTM Training](/images/LSTMTraining.JPG)

We also created a visualization where we used our predicted RUL at each cycle to calculate a predicted capacity for the consequent cycle as C<sub>i+1</sub> = C<sub>i</sub> - (C<sub>i</sub> - 1.4) / RUL.

![LSTM Capacity 2](/images/LSTM2Capacity.JPG)

Its important to note that this visualization seems to do much better than the first model because it is only predicting one cycle into the future
