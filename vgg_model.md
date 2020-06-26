# VGG Model
VGG model is a convolutional neural network created by researchers at Oxford's Visual Geometry Group. The model won the ImageNet Challenge (ILSVR) in 2014
with error rate of 7.3%. VGG is composed of 13 convolutional layers and 3 dense layers, making it total of 16 layers (with weights). In addition, there are an input layer,
an output layer and pooling layers. VGG architecture is illustrated below. Note that this is VGG16 and there is also VGG19 which has 19 layers.

<p align="center">
<img src="https://github.com/TravisH0301/learning/blob/master/images/vgg0.png" width="400">
<img src="https://github.com/TravisH0301/learning/blob/master/images/vgg1.png" width="400">
</p>

The convolutional layers have 3x3 filters with a stride 1 and same padding, while maxpooling layers have 2x2 filters with a stride 2. And the dense layers have 4096 neurons.
These stacked layers result in a very large network with 138 million (approx) parameters. For stacked convolutional layers, two convolutional layers with 3x3 filters
have the effective receptive field as a convolutional layer with 5x5 filters. And three convolutional layers with 3x3 filters have the effective receptive
field as a convolutional layer with 7x7 filters. And more layers result in more abstract information being learnt through more ReLu activation functions. 
Hence, it is more effective than a single convolutional layer with larger filter size. Additionally, the deeper the network goes, the spatial size of the feature maps 
reduces due to pooling, yet, the depth of feature maps increases due to having higher number of filters in deeper layers. 

## Learning abstract information & features
The model learns abstract information and features of input images through its deep structure. In the shallow level of the model, it learns simple edges and lines about
the image and in the deeper level of the model, it learns about more specific concepts such as person's eye or nose. And the feature maps become sparser in deeper levels
due to uniqueness of the specific features in the images. 

## Limitation
VGG model harvests more abstract information with stacked convolutional layers with 3x3 filters. However, when the input image is large, this induces more memory 
consumption and computation time. To resolve this issue, recent architecures such as ResNets or GoogleNet first reduce the spatial resolution drastically 
before applying any convolutional layers.

## Implementation
A pre-trained VGG model can be implemented where it is trained to classify 1000 objects such as a keyboard, a dog and a cat. The model can be implemented using Keras and
it is a heavy model of 528MB. The following Keras documentation can be referred to for implementing VGG16 and VGG19 on Python.

https://keras.io/api/applications/vgg/

In the following notebook, VGG16 model is implemented to classify COVID-19 based on lung CT scans. 

[notebook link]


