---
layout: technical-post
title: Neural nets 101 - Recurrent Neural Networks 	

---

A brief intro into recurrent neural networks. 



# NN08 - recurrent networks, BPTT

In this notebook: 
- recurrent networks
- backpropagation through time (BPTT) 
- time-series models
- sequence models


## Re-current networks

<img src="rnn.png",width=300,height=300>
Examples include: Hopfield, Boltzmann, Echo-state, Long-short term memory(LSTMs), Nonlinear AutoRegressive network with eXogenous inputs (NARX), Elman and Jordan networks etc. 

Can be trained by standard backprop and variations e.g. input/ output vectors are mapped to input+context/output (possibly using a sliding window approach)...


#### Backprop. through time (BPTT) 
Given data  sequence x with target values t: (x<sub>0</sub>,t<sub>0</sub>),(x<sub>1</sub>,t<sub>1</sub>),(x<sub>2</sub>,t<sub>2</sub>)...
Unfold a recurrent network through time (e.g. k=3 below) 

<img src="bptt.png",width=400,height=400>

- Train unfolded net with backprop. but in order, i.e. obtaining o<sub>0</sub>, o<sub>1</sub>, o<sub>2</sub>...
- s<sub>0</sub> is normally a vector of zeros
- Each training example is of the form (s<sub>t-1</sub>, x<sub>t</sub>, s<sub>t</sub>, x<sub>t+1</sub>, s<sub>t+1</sub>, x<sub>t+2</sub>, t<sub>t+2</sub>)
- typically use online learning
- after each example, average the weights to get the same U, V, W. 




```python



```


```python

```
