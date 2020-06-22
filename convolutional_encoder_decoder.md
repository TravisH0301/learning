# Convolutional Encoder Decoder

This model is a convolutional neural network model consisted of encoder and decoder.
During encoding, dimensions of input features are reduced. And this low resolution features are mapped back to 
its full initial dimensions via decoding. This enables the model to be space efficient, hence, result in faster 
computation with lower memory consumption. And this gives a good performance for a pixel-wise classification such as
image segmentation. The below image illustrates architecture of this model. 

