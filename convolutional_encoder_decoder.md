# Convolutional Encoder Decoder

This model is a convolutional neural network model consisted of encoder and decoder.
During encoding, dimensions of input features are reduced. And this low resolution features are mapped back to 
its full initial dimensions via decoding. This enables the model to be space efficient, hence, result in faster 
computation with lower memory consumption. And this gives a good performance for a pixel-wise classification such as
image segmentation. As illustrated in the image below, pooling layers are used in encoder and upsampling layers are used in decoder. 
Mapping low resultion features lead to sparse features, yet, learning through convolutional layers make them dense. 
If gray-scale images are used, the model will user 1 channel for its input and output layers. Or if RGB-scale images are used, 
the model will take 3 channels. 

<p align="center">
<img src="https://github.com/TravisH0301/learning/blob/master/images/convolutional_encode_decode.png" width="600">
</p>

Below is the image segmentation of Lung CT images performed by the model. The left is the original CT image, the centre is the 
manually masked CT image and the right is the segmented CT image using the Convolutional Encoder Decoder model. 

<p align="center">
<img src="https://github.com/TravisH0301/learning/blob/master/images/lung_ct.png" width="400">
</p>
