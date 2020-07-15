Seven HMM models were trained, with numbers of initial components ranging from 3 to 10. 10 was selected as the maximum number of components as the average number of cycles until failure was around 100, and too many components would lead to overfitting of the data. Thus the square root of the number of cycles until failure was used as a maximum number of components. Data was split 75-25 between training and testing sets. Each model was trained four times in order to ensure convergence to the best local minima, and the model with the smallest root mean square error (RMSE) when tested on the testing data set was selected as the best performing model for that number of compartments. 

![RMSE vs. number of components](/images/RMSEvsncomponents.png)

In this figure, the RMSE of the best performing model versus the number of initial components can be seen. The data generally shows an upward trend in the error as the number of components increases, likely due to the increase in number of learnable parameters as the number of components increases. Interestingly, the RMSE dropped dramatically to its lowest value with 9 components. 

Here, we show the results of the prediction on the test data from the best overall model, with 9 initial components.

![Best performing model prediction](/images/m9results.png)

Here, remaining cycles until failure is plotted versus cycle number. The actual data is in blue and the predicted data is in red. It is apparent that this model tends to underpredict the number of cycles until failure, giving a low estimate of remaining useful life of the battery. Also of note is the number of steps in the predicted data. As the model was initialized with nine compartments, it is expected that the prediction data would have nine steps. However, upon visual inspection, only six discrete steps are present. The rest of the plots can be seen here:

![other models](/images/models.png)

As seen in these figures, this behavior is consistent across all models. Each model has converged to a number of compartments smaller than the initial number. The most common number of compartments after convergence is six. This is consistent with the observation that the best performing model converged to six compartments. 
