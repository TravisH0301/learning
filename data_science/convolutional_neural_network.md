# Convolutional Neural Network (CNN or ConvNet)
Convolutional neural network is a regularised version of multilayer perceptron (MLP a.k.a Feedforward NN). MLP is consisted of layers of perceptrons
where neurons of layers are fully connected to features/neurons in the neighboring layers. This makes the model prone to overfitting. For image processing, 
number of features to be learned by the model increased sharply as each pixel of an image is regarded as a feature. Hence, CNN is commonly used for images. 

## Table of Contents
- [Convolving](#convolving)
- [Stride & Padding](#stride--padding)
- [Architecture](#architecture)
- [Additional Regularisation](#additional-regularisation)

## Convolving
Typically, CNN is consisted of an input and output layers and series of convolutional layers. Convolutional layers have filters/kernals as a 2D matrix that 
performs sum of element-wise multiplication on input features while rolling over (convolving) as the image below. The left is a 2D input features 
and the right is a 2D feature map resulted from convolving. 

<img src="https://github.com/TravisH0301/learning/blob/master/images/convnet1.gif" width="400">

An element of the feature map can be referred as a neuron, however, this term is not generally used to describe convolutional layer. Filters contain weights that represent a particular feature and convolving allows the model to detect the feature on receptive field, which is a portion of the input features that filter performs multiplication. 
And the feature map will contain information about how the receptive field is like in regards to the feature of the filter. Hence, this is how convolutional layer
learns abstract information of the input data. Besides the weights of a filter, they also have a bias. And note that for tensors (3D features), dot product may be used to describe convolving instead of element-wise multiplication as element-wise multiplication of two tensors, X & Y are eqaul to dot product of X & trasposed Y. 

Multiple filters can be applied in the convolutional layer. (Numbers can be power of 2 between 32 to 1024) An extra filter will result in an additional feature map as illustrated 
in the image below. And note that the depth of a filter has to be equal to the depth of an input feature. The final output feature map is made by stacking all feature maps,
making the depth of the final output feature map to equal to the number of filters. An activation function is then applied for each element of the final feature map. 

<img src="https://github.com/TravisH0301/learning/blob/master/images/convnet2.png" width="400">

## Stride & Padding
Stride specifies a size of movement filters make during convolving. Bigger stride will result in smaller feature map. The size of feature map can be determined by 
the following equation:

Feature map size = (Input size - Filter size)/Stride + 1

This equation shows that feature dimension will be reduced even stride is 1. Thus, to prevent reduction in dimensions, padding can be applied. 

<img src="https://github.com/TravisH0301/learning/blob/master/images/convnet3.gif" width="400">

Then the input size will be changed to 'Input size + 2 x Padding size'. Paddings can be filled with 0 or the values of the edge. This helps to preserve the features
during convolving. 

## Architecture
<img src="https://github.com/TravisH0301/learning/blob/master/images/convnet4.png" width="400">

Basic CNN architecture is composed of convolutional layers, maxpooling layers, fully connected (dense) layers between an input and an output layers.
Pooling layer reduces the size of feature maps learned from convolutional layers to reduce number of features parameters. This limits overfitting and enhance 
computation time. This only reduces 2D of the feature map (not depth). Common pooling type is maxpooling where a maximum value of a pooling window is selected. 
Similar to how convolving works, the pooling window rolls over the feature map to select the max. values. Below is the maxpooling with 2x2 pooling window with stride of 2.

<img src="https://github.com/TravisH0301/learning/blob/master/images/convnet5.png" width="400">

After convolving and pooling process, the features are then flatten down to 1D since dense layers only take 1D tensors. 

## Additional Regularisation
Apart from adding a pooling layer, dropout technique can be used. Dropout eliminates neurons in a layer and this prevents the network to be too dependent on small number of neurons
and forces neurons to operate independently. This technique is only used during training process and each training step will drop out different neurons. 






