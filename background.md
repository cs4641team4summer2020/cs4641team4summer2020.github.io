The rapidly increasing use of fossil fuels and its associated
environmental costs has led to a growing need for efficient methods of
energy storage. One widespread and readily available energy storage
solution is the lithium-ion rechargeable battery, which is universally used as it
has high energy storage capacity, high energy density, long
lifetime, and relatively low environmental impact. They are commonly
used in small electronics, including laptop computers, mobile phones,
and power tools. More recently, as battery technologies have improved,
they have been employed in electric and hybrid vehicles as well.

One problem inherent in these batteries is the mechanical strain which
occurs as lithium ions travel from the cathode to the anode as part of
normal battery operation. This can result in a buildup of cracks and
charge heterogeneity inside the battery, which eventually contributes
to a decrease of the battery's energy capacity over many charge and
discharge cycles. These effects, among others, contribute to a loss of
a battery's efficiency over time.

For practical purposes, battery failure is considered to occur when
its capacity (i.e., the amount of charge it can hold) drops below a
certain threshold value. When this happens, its power output drops and
becomes unpredictable; this makes the battery much less efficient and
potentially unsafe to use. There are several methods in practice which
predict when a battery will fail. The prediction metric we
will use is remaining useful life (RUL), which is defined at any
time as the estimated number of cycles until the battery's capacity
drops below the threshold value.

There are several existing RUL prediction methods in use, falling into
two categories: model-based and data-based models. The former relies
heavily on a mathematical model specific to a battery, but is often
prone to overfitting and is generally not able to take into account
long-term performance and behavior of the battery. The latter class of
RUL prediction methods uses machine learning techniques in order to
model battery degredation without the use of complex mathematical
models. In the past, methods including SVM and LSTM have been used
with varying degrees of success. Zhang et al. (2017) employed an LSTM
model in a similar fashion as the current study and showed that
recurrent neural network (RNN) methods such as LSTM tend to show
greater levels of accuracy with battery degredation, which is modeled
naturally as sets of time-series data. We will focus on data-based models.

In this study, we will compare the performance of long short-term
memory (LSTM) models and hidden Markov models (HMM) as RUL
predictors. Both models have been applied successfully to
time-dependent data in the past. Zhang et al. describe how LSTM is
applied successfully to battery degredation data as time series of the
battery capacity over cycles.

LSTM is a type of recurrent neural network which relies on specialized
LSTM neurons.

{% include image.html
url="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/The_LSTM_cell.png/800px-The_LSTM_cell.png"
description="Source: Wikipedia, Long short-term memory" %}

The LSTM cell comprises an input gate, an output gate, and a forget
gate, which control the flow of information through the cell. The
input gate controls how much new information enters the cell, the
forget gate controls how long information remains in the cell, and the
output gate controls how much information leaves the cell via an
activation function. Using an algorithm such as gradient descent and
backpropagation, the parameters of these gates can be calculated and
each cell can be tuned to use varying amounts of past data.

HMM is a discrete-time
statistical model which assigns states to each point in time and
attempts to calculate the conditional probability of transitioning
into the next state at each time interval. If $$X$$ is a process
with unobservable states and $$Y$$ is an observable process whose
observed states we wish to use to make inferences about $$X$$, then
the pair $$(X, Y)$$ is a hidden Markov model if for all $$n$$ in our
time domain we have that

$$P(Y_n \mid X_1 = x_1, \dots, X_n = x_n) = P(Y_n \mid X_n = x_n).$$

In other words, we assume that the process $$X$$ depends only on the
most recent state and not on any prior states. Typical HMM learning
methods include various expectation-maximization algorithms employing
maximum likelihood estimation.

In this case of battery failure, we assume the final state is absorbing - that is, once a battery is observed in this final state, it cannot enter a different state. This means that a battery that has failed cannot "un-fail." This assumption allows the calculation of expected number of charge/discharge cycles until failure. In a Markov model with an absorbing state, the expected number of steps until absorption can be calculated as 

$$t = (\mathbf{Q}-I)\mathbf{c}$$

Here, $$\mathbf{Q}$$ is the square submatrix of the transition matrix of the Markov model that does not include the row or column of the absorbing state, and $$\mathbf{c}$$ is a column vector of all ones. This calculation is used to predict the remaining useful life of a battery. As the model categorizes batteries into the different states in the model, these categorizations can be used to determine the expected number of cycles until absorption, which signifies failure of the battery.

The critical difference
between LSTM models and HMMs is the Markov assumption which is made in
HMMs - that is, when working with a HMM, we assume that the underlying
hidden process depends only on its current state and not any prior
states. LSTM models, in contrast, are able to take into account past
data and use this to make a future prediction. Thus, comparison of
these two methods will reveal some information about the behavior of
the processes underlying degredation of lithium-ion batteries.

For this study we use a dataset whose testbed comprises several 18650
sized rechargeable lithium-ion batteries who were run through charge,
discharge, and electrochemical impedance spectroscopy cycles until
failure, which is defined as a 30% reduction in the batteries' nominal
capacity of 2.0 amp-hours (Ah). In other words, we consider the batteries to have
failed once their capacity reaches 1.4 Ah.
We focus only on discharge cycles and consider
the battery capacity vs. cycle as the time-series data with which we
will train a LSTM model and a HMM.
