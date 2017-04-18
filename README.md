# IntegratedGradients
Python implementation of integrated gradients (https://arxiv.org/abs/1703.01365). The algorithm "explains" a prediction of a Keras-based deep learning model by approximating Shapley values and assigning them to the input sample features. 

# Usage

Here is a minimal working example on UCI Iris data

```
from IntegratedGradients import *
from keras.layers import Dense
from keras.layers.core import Activation

X = np.array([[float(j) for j in i.rstrip().split(",")[:-1]] for i in open("iris.data").readlines()][:-1])
Y = np.array([0 for i in range(100)] + [1 for i in range(50)])

model = Sequential([
    Dense(1, input_dim=4),
    Activation('sigmoid'),
])
model.compile(optimizer='sgd', loss='binary_crossentropy')
model.fit(X, Y, epochs=300, batch_size=10, validation_split=0.2, verbose=0)

ig = integrated_gradients(model)
ig.explain(X[0])
==> array([-0.25757075, -0.24014562,  0.12732635,  0.00960122])
```

The above example is on a Sequential model but this module works on Keras generic models as well.

# How does it work?

"The Shapley value is a solution concept in cooperative game theory. It was named in honour of Lloyd Shapley, who introduced it in 1953. To each cooperative game it assigns a unique distribution (among the players) of a total surplus generated by the coalition of all players. (Wikipedia)". In the framework of deep-learning, integrated gradients attempt the difference between the predictions with the sample of interest and reference sample (defaulted to all zeros) to input features.

Email me at hiranumn at cs dot washington dot edu for questions.
