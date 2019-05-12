---
layout: technical-post
title: Neural nets 101 - Hopfield network & Gibbs Sampling	

---

In this post we look at Hebbian Learning, Gibbs Sampling and Hopfield Networks.

# NN05 - Hopfield networks

In this notebook: 
- hebbian learning and Hebb's rule
- associative and auto-associative networks
- hopfield networks


## Hebbian Learning 
Hebb's postulate states that... 
"*When an axon of cell A is near enough to excite a cell B and repeatedly or persistently takes part in firing it, some growth process or metabolic change takes place in one or both cells such that A's efficiency, as one of the cells firing B, is increased." 

**Hebb's rule**: If two neurons on either side of a synapse are activated simultaneously, increase the weight of the synapse. Otherwise, decrease the weight. 
<img src="hebb.png",width=200,height=200>

Thus, a good candidate learning rule for neurons with bipolar activation is: 
<img src="hebb2.png",width=200,height=200>

## Hopfield networks
A hopfield network is a single-layer recurrent network (with feedback). Each neuron is a perceptron with sign(x) activation function. It's a symmetrical network, therefore W<sub>ij</sub> = W<sub>ji</sub>. 

**A hopfield network uses Hebb's rule for learning.** At each time *t*, each neuron *i* has activation A<sub>i</sub>(t) = 1 or -1. Selecting neuron *i* to calculate its new activation value at t+1: 
<img src="hopfield.png",width=200,height=200>
- *sign(x) = 1 if x > 0*
- *sign(x) = -1 if x < 0 *
- *sign(x) = A<sub>i</sub>(t) if x = 0*

#### Two phases of Hopfield Networks (as associative memory): 
1. Storage phase: 
    - to store M N-dimensional vectors ε<sub>µ</sub>
    - let ε<sub>µi</sub> denote the i-th element of ε<sub>µ</sub>
    - this is also called one-shot learning. 
<img src="osl.png",width=200,height=200>

2. Retrieval phase: 
    - An n-dimensional vector of ε<sub>p</sub> is imposed on the network; 
    - At each time *t*, a neuron *i* is selected and its activation state is updated to A<sub>i</sub>(t+1) immediately before another neuron is selected. 
    -Neurons are selected in order until a time invariant vector of activations (stable state) ε<sub>S</sub> is found, i.e. until A<sub>i</sub>(t+1) = A<sub>i</sub>(t) for all neurons in the network. 
<img src="energy.png",width=400,height=400>


#### State transition tables
https://www.inf.ed.ac.uk/teaching/courses/nlu/assets/reading/Gurney_et_al.pdf

