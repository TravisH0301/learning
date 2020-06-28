# Neural Network Optimisation

## Optimisation for high variance
#### High bias
High bias is indicated by high training error and this is due to model not learning the features of the data well. This can be improved by adding up more layers 
of the network and training longer. Other NN architecture may be sought as well.
#### High variance
High variance is indicated by higher testing/validation error than training error. This indicates that the model is overfitting. This can be resolved by increasing the
size of the dataset, reducing network size or applying regularisation. Again, other NN architecture may be sought too. 

### L1 L2 Regularisation
Also known as weight decay, the regularisation can be applied on gradient descent process where a regularisation term is added on the error function. Hence, adding a 
weight decay will result in further reduction in network weights. 

L1 (Lasso) Regularisation implementation on gradient descent<br>
W' = W - (learning rate)[dE/dW + ((lambda)/n)*||W||]

L2 (Ridge) Regularisation implementation on gradient descent<br> 
W' = W - (learning rate)[dE/dW + ((lambda)/n)*||W||^2]
<br> where, n=number of training datapoints
<br>||W||^2 is also referred as Frobenius norm

### Dropout regularisation
Dropout regularisation involves dropping out random activation units in training process. It will have a probability of keeping (p) where it determines 
how many units are shut down. In this way, the model doesn't rely on specific weights and instead it relies on more weights. And this spreads out weights 
and limit overfitting. 

#### Invert dropout
Note that dropout only occurs in training process and due to dropout regularisation, the model becomes used to dropped out activation units. And this causes
the activation units too large during the training process when dropout is not applied. Hence, to prevent this, the during invert dropout process, the activation 
units that are not shut down will be multiplied by 1/p to ensure expected activation is the same for the training and testing processes. In traditional dropout process, 
the probability of keeping are multiplied to each layer during training process which consumes more computational power.

### Data augmentation
Data augmentation is a technique to increase dataset size by increasing diversity of the available data. This includes roation, distortion, cropping, zooming and padding 
of images. 

### Early stopping
Early stopping of training can limit the increase of weight size (either -ve or +ve, since it's heading to reach local minima of error function). Early stopping can be 
triggered at different events. For example when generalised error (bias^2 + variance) increases, it can be triggered or when validation accuracy no longer increases, 
it can be triggered as well. On the otherside, early stopping can prevent the model to reach the minimised error function due to stopping early. Hence, a use of 
regularisation is more recommended sometimes. 
 
## Optimisation for input features
### Input normalisation
Normalisation on input features allow the error function to have more round and uniform shape. Below image illustrates contours of the error function when it's 
un-normalised (LEFT) and normalised (RIGHT). When features are normalised, gradient descent process becomes more efficient. 
Input features can be in different units, yet, they have to be within similar ranges. ex) -1 ~ 1, 0 ~ 1, 1 ~ 2<br>
Note that both training and testing datasets must have the same normalisation process.

<p align="center">
<img src="https://github.com/TravisH0301/learning/blob/master/images/nn_opt1.png" width="400">
</p>

## Vanishing/Exploding gradients
Vanishing and exploding gradients occurs at shallow level of the network layers. The partial derivative of error with respect to the weight at shallow level layer 
is associated with all the weights that are in deeper layers. Hence, when the gradients at deeper layers are small, the gradients at shallow layers will be vanishing.
And large gradients at deeper layers will result in exploding gradients at shallow layers. 

This is related to the depth of the network. Deeper the network is, more prone it is to vanishing/exploding gradients. Also, initialisation of weight is related to this.
Since weights or activations are transferred to the next layers, initial weights of the network is important. If initial weights are less than 1, then weights or activations
at deeper layers will be vanishing. And when backpropagation is performed on such small weights, gradients at shallow layers will be vanishing. Likewise initial weights 
larger than 1 will cause exploding weights/activations at deeper layers and exploding gradients gradients at shallow layers. 

_This link includes example of backpropagation with real numbers and shows how gradients at one layer is associated with deeper layers._
<br>https://mattmazur.com/2015/03/17/a-step-by-step-backpropagation-example/

### Weight initialisation
As initial weights are related to vanishing/exploding gradients, weight initialisation can be adjusted to partially resolve the issue. 
 
