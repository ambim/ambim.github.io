---
layout: technical-post
title: Neural nets 101: History of neural networks

---

In this notebook, I look back through the history of neural networks. 

# NN01 - history of neural networks

This notebook includes: 
- Model of the neuron
- Perceptron
- Pearceptron's learning algorithm 
- Multilayer perceptron
- Learning


#### Haykin's definition: 
A neural network is a massively parallel distributed processor made up of simple processing units, which has a natural propensity for storing experiential knowledge anad making it available for use. It resembles the brain in two respects: 
1. Knowledge is acquired by the network from its environment through a learning process. 
2. Interneuron connection strengths, known as synaptic weights, are used to store the acquired knowledge. 

**Useful properties of a neural network**: 
- *Non-linearity* - a neural network, made up of an interconnection of nonlinear neurons, is itself nonlinear. Interestingly the nonlinearity is distributed through the network. Nonlinearity is a highly important property, particularly if the underlying physical mechanism for generation of the input signal (e.g. speech signal) is inherently nonlinear. 
- *Input-Output mapping*: supervised learning involves the modification of the synaptic weights of a neural network by applying a set of labeled *training samples*. The network is presented with an example picked at random from the set, and the synaptic weights (free parameters) of the network are modified to minimise the difference between the desired response and the actual response. Thus the network learns from the example by constructing an *input-output mapping* for the problem at hand. 
- *Adaptivity* - neural networks have a built-in capability to *adapt* their synaptic weights to changes in the surrounding environment and can be *easily retrained* to deal with minor changes in the operating environment (i.e. one where statistics change with time), a neural network can be designed to change its synaptic weights in real time. 
- *Evidential response* - a NN can be designed to provide information not only about which particular pattern to *select*, but also about the confidence in the decision made. This latter information may be used to reject ambiguous patterns, should they arise, and thereby improve the classification performance of the network. 
- *Contextual information*
- *Fault tolerance* 
- *Uniformity of analysis and design* 


## Model of a neuron 

<img src="neuron2.png",width=500,height=500>
(ref: Artur Garcez)

Four basic elements of the neuronal model: 
1. Set of connections, characterised by a *weight* of its own. Specifically, an input signal I<sub>i</sub>(t) at the input of synapse *j* connected to the neuron *k* is multipled by the synaptic weight *w<sub>kj</sub>*. 
2. An *adder* - summing the input signals, weighted by the respective synapses of the neuron; the operations described here constitute a linear combiner. 
3. An *activation function* for limiting the amplitude of the output of a neuron (also known as a squashing function). Typical outputs either in the range [0,1] or [-1,1]. 
4. Bias - denoted θ<sub>i</sub>, has the effect pf increasing or lowering the net input of the activation function; depending on whether it is positive or negative, respectively. 

#### Activation functions
<img src="activation.png",width=500,height=600>
(ref: Artur Garcez)

**Step function (threshold)**: 
Used in McCulloch and Pitts; an output of 1 if the induced local field (i.e. weighted sum of inputs) is non-negative, and 0 otherwise. 


**Linear function**: 
Returns any value of between 0 and 1, proportional to the size of the input. If training with gradient descent & backprop... the derivative of a linear function is constant. Therefore the gradient is constant and therefore  

**Sigmoid**: 


**ReLU (rectified linear unit)**: 



#### McCulloch-Pitts neuron
<img src="mcp.png",width=500,height=600>




## Perceptron

<img src="perceptron.png",width=500,height=600>



## Perceptron's learning algorithm

<img src="learning.png",width=500,height=600>




**Perceptron's linear separability/ XOR problem**: 


## Learning 

Haykins definition: 
- *Learning is a process by which the free parameters of a neural network are adapted through a process of stimulation by the environment in which the network is embedded. The typ of learning is determined by the manner in which the parameter changes take place.*

**Basic learning rules**: 
- **Error-correction learning**: minimising a cost function with the delta rule, or Widrow-Hoff rule. 
- **Memory-based learning**: all or most of the past experiences are explicitly stored in a large memory of correctly classified input/output examples. 
- **Hebbian learning**: *"if two neurons on either side of a synapse (connection) are activated simultaneously, then the strength of that synapse is selectively increased.*". Activity product rule 
- **Competitive learning**: output neurons of a neural network compete among themselves to become active (fired). In CL, only one neuron fires at a time, making it highly suited to discover statistically salient features that may be used to classify a set of input patterns. E.g. Self-organising map
- **Boltzmann learning**: stochastic learning algorithm derived from ideas rooted in statistical mechanics. BM characterised by an energy function, E, the value of which is determined by the particular states occupied by the individual neurons of the machine. 




```python

```
