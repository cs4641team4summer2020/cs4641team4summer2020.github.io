The two models applied to the data (LSTM and HMM) delivered differing levels of success modeling the data. The LSTM was accurate, but also generally susceptible to overtraining. It also allowed direct prediction of the RUL up to an accuracy of +/-10 cycles. The HMM, fitted into less than ten compartments, had an unpredictable accuracy as the number of compartments proved to be an unrelated hyperparameter. The predictions of the HMM were low in value as the predictions were discrete. The LSTM, with continuous predictions, was able to make more accurate predictions.

While the LSTM was largely more successful than the HMM, the HMM was significantly more efficient. The first LSTM model was more efficient but less accuraet than the second LSTM model, which utilized walk-forward validation and had to be retrained every transition between cycles.

Though the LSTM model showed the most relevant predictions, these models are not ready for production without more training on larger data sets.
