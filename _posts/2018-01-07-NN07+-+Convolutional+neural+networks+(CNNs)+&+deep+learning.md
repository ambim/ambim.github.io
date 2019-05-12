---
layout: technical-post
title: Neural nets 101 - Deep learning with Convolutional Neural Networks 	

---

A brief intro into deep learning & convolutional neural networks. 


# NN07 - Convolutional neural networks (CNNs) & deep learning

In this notebook: 
- Sigmoid belief nets (rev3) 
- deep belief nets 
- CNNs 
- deep learning


## Convolutional neural networks
- Inspired by the visual cortex: multiple layers with overlapping patches of neurons mapped onto feature maps. 
- Very successful at image and video recognition. 
- Seeks to achieve translation (shift), rotation, size and illumination invariance... 
- Convolution + subsampling steps followed by single-hidden layer classifier trained with backprop. 
- Overlapping input neuron areas with weight sharing. 
- Produces an activation map (similar to SOM) for each filter (set of weights). 

The activation map is then reduced (subsampling) by partitioning it into a set of new non-overlapping areas, and keeping only the neuron with the highest activation value in each area (max pooling). 

#### Advantages + disadvantages of a CNN: 
- (+) make assumption of locality (normally with images), successful in extracting relevant information from *local spatial coherence* of the input. 
- (-) can be non-trivial to train and optimise hyper-parameters. 
- (-) traditional implementations are not invariant to rotation or scale. 

#### Overview 
(ref: http://ufldl.stanford.edu/tutorial/supervised/ConvolutionalNeuralNetwork/) 
A Convolutional Neural Network (CNN) is comprised of one or more convolutional layers (often with a subsampling step) and then followed by one or more fully connected layers as in a standard multilayer neural network. The architecture of a CNN is designed to take advantage of the 2D structure of an input image (or other 2D input such as a speech signal). This is achieved with local connections and tied weights followed by some form of pooling which results in translation invariant features. Another benefit of CNNs is that they are easier to train and have many fewer parameters than fully connected networks with the same number of hidden units. 

#### Architecture
A CNN consists of a number of convolutional and subsampling layers optionally followed by fully connected layers. The input to a convolutional layer is a *m* x *m* x *r* image where *m* is the height and width of the image and *r* is the number of channels, e.g. an RGB image has *r=3*. The convolutional layer will have *k* filters (or kernels) of size *n* x *n* x *q* where *n* is smaller than the dimension of the image and *q* can either be the same as the number of channels *r* or smaller and may vary for each kernel. The size of the filters gives rise to the locally connected structure which are each convolved with the image to produce *k* feature maps of size *m*−*n*+*1*. Each map is then subsampled typically with mean or max pooling over *p* x *p* contiguous regions where p ranges between 2 for small images (e.g. MNIST) and is usually not more than 5 for larger inputs. Either before or after the subsampling layer an additive bias and sigmoidal nonlinearity is applied to each feature map. The figure below illustrates a full layer in a CNN consisting of convolutional and subsampling sublayers. Units of the same color have tied weights.
<img src="cnn.png",width=400,height=400>



```python

```
