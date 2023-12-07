# Machine Learning Basics
- [Algorithm Notation](#algorithm-notation)
- [Supervised Learning](#supervised-learning)
    - [Regression](#regression)
        - [Optimisation](#optimisation)
            - [Cost Function](#cost-function--loss-function)
            - [Gradient Descent](#gradient-descent)
        - [Vectorisation](#vectorisation)
- [Unsupervised Learning](#unsupervised-learning)


## Algorithm Notation
$f_{w,b}(x) = wx + b = \hat{y}$

$f$ = function, hypothesis <br>
$w$ = parameter - weight <br>
$b$ = parameter - coefficient <br>
$x$ = input, feature <br>
$y$ = actual output <br>
$\hat{y}$ = prediction / expected output <br>

## Supervised Learning
Supervised learning model takes an input x and returns output y by training the model with the labelled input and output sets.

There are largely 2 types of supervised learning algorithms:
- Regression: returns a numeric output
- Classification: returns a categorical output

### Regression

#### Optimisation
The model algorithm can be optimised by minising the error of the expected output.

##### Cost Function / Loss Function
Cost function, also known as loss function is a mathematical function that quantifies the error or difference between the predicted values and the actual values. The goal in training a machine learning model is to minimise this function to reduce error and improve accuracy.

- In Regression, common cost functions are Mean Squared Error (MSE) or Mean Absolute Error (MAE) <br><br>
E.g., $MSE = J(w,b) = \frac{1}{m} \sum_{i=1}^m (\hat{y}^i - y^i)^2 = \frac{1}{m} \sum_{i=1}^m (f_{w,b}(x^i) - y^i)^2$ <br><br>
$J$ = cost function <br>
$i$ = iteration <br>
$m$ = number of training examples

- In Classification, common cost function is Cross-Entropy Loss (=Log Loss) and 

##### Gradient Descent
The process of minising cost funciton is known as optimisation and gradient descent is often used to find the model parameters that result in the minimum possible cost.

Gradient descent algorithm simulataneously finds new model parameters until it converges at the minimum cost.

$w = w - \alpha \frac{d}{dw} J(w,b)$ <br>
$b = b - \alpha \frac{d}{db} J(w,b)$ <br><br>
$\alpha$ = learning rate

Once all new parameters are found, they are updated together. This process is repeated until reaching convergence. And the learning rate determines the rate of change in the parameters.

- Choosing too small learning rate would take long time to reach convergence or train a model
- Choosing too large learning rate would overshoot and may not be able to reach convergence

Depending on the type of cost functions, there may be multiple local minimas instead of a global minimum. Hence, the starting parameter values can also affect the model training time.

In updating the parameters, the algorithm can have 3 different approaches:
- Batch gradient descent: Parameters are updated based on the cumulative error across all data points. This leads to stable and reliable convergence, but slow and expansive.
- Stochastic gradient descent: Parameters are updated after computing the gradient of one data point. This can compute faster and handle large dataset, but it can be noisy, leading to significant variance in training path, and longer time to converge.
- Mini-batch gradient descent: The dataset is divided into small batches, and parameters are updated for each batch. This is the most common approach, striking balance between reliability and speed. However, the batch size is a hyperparameter that need tuning.

#### Vectorisation
For a multivariate model, features and parameters are vectorised for the ease of computation.

Without vectorisation, multiple linear regression model function looks like: <br>
$ f_{\vec{w},b}(\vec{x}) = \sum_{j=1}^n w_j x_j + b $ <br><br>
$\vec{w}$ = vector w = $[\begin{matrix} w_1 & w_2 & w_3 \end{matrix}]$ <br>
$n$ = total number of features

With vecorisation, <br>
$ f_{\vec{w},b} = \vec{w} \cdot \vec{x} + b $

## Unsupervised Learning
Unsupervised learning model takes unlabelled datasets with no labelled output and identifies a pattern/structure of the data.

There are largely 
- Clustering: Group similar data points
- Anomaly detection: Find outlying data points
- Dimensionality reduction: Compression of data from high-dimension to low-dimension while retaining meaningful properties of original data


