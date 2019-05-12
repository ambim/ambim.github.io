---
layout: technical-post
title: Neural nets 101 - Self-organising maps

---

In this post, I take a look at self-organising maps. 

# NN03: Self-organising maps

In this notebook: 
- Self-organising maps

To introduce the Self-organising map architecture, first we need to introduce a few new concepts. 

#### Competitive learning 
In competitive learning, the neurons compete with each other to determine a winner - after competition, only one neuron will have a positive activition. 

Mathematically: 
- let *m* denote the dimension of the input space. 
- let **x** = [x<sub>1</sub>, x<sub>2</sub> ... x<sub>m</sub>] be an input vector. 
- Let **W**<sub>j</sub> = [W<sub>j1</sub>, W<sub>j2</sub> ... W<sub>jm</sub>] be the weight vector of the neuron j (j=1,2..n); where *n* is the total number of neurons in the network). 

**2D feature map:**
<img src="map.png",width=350,height=350>

**Winning criteria** 
The neuron whose weight vector **best matches** the current input vector wins: 
- compare inner product **W**<sub>j</sub>**x** for j = 1,2,...,n and select the largest. 
- Alternatively, minimize the Euclidean distance between **W**<sub>j</sub> and **x**
<img src="euc.png",width=150,height=150>
...where i(x) identifies the neuron i that best matches input x

#### Co-operation 
Co-operation: the winning neuron locates the centre of the topological neighbourhood of co-operating neurons. 

A firing neuron tends to excite the neurons in the immediate neighbourhood more than those far away. 

Let *h<sub>ji</sub>* denote a neighbourhood centred on winning neuron i and containing a set of co-operating neurons, one of which is j. 

Let *d<sub>ji</sub>* denote the distance between winning neuron i and its neighbour j. 
- we want *h<sub>ji</sub>* to be symmetric w.r.t *d<sub>ji</sub>*
- we want *h<sub>ji</sub>* to decrease monotonically with increasing *d<sub>ji</sub>*
- a typical choice is the Gaussian function 
<img src="coop.png",width=300,height=300>

A SOM is biologically plausible as Competition implements *lateral inhibition*, and Co-operation implements *lateral interaction*. 

#### Synaptic adaptation
For the network to be self-organising, the weight vectors are required to change according to the input vector. 
A variation of Hebb's rule is used: 
<img src="hebbb.png",width=150,height=150>
This has the effect of *moving* the weight vector W<sub>i</sub> of winning neuron i towards the input vector x, and the weight vector of neighbouring neurons W<sub>j</sub> towards x but less than W<sub>i</sub> (with discount *h<sub>ji</sub>*)

Upon repeated presentations of training data, the weight vectors follow the distribution of the input vectors. 

#### Properties of self-organising maps
- **Dimensionality reduction**: self organising maps transform input pattern of arbitrary dimension into one or two dimension discrete map. 
- **Feature extraction**: given data from an input space with a non-linear distribution, the SOM is able to select a set of best features for approximating the distribution. 
- **Topological ordering**: the feature map computed by the SOM is topologically ordered in the sense that the spatial location of a neuron in the lattice corresponds to a particular domain or feature of input patterns. 

#### Applications: 
- locating similar patterns in maps and satellite images (e.g. meteorology). 
- to predict potential areas for good oil prospecting based on geological data. 
- organisation and retrieval from large document collection (WEBSOM).
- dimensionality reduction before computer vision tasks (due to its topological ordering qualities: 
<img src="hybrid.png",width=350,height=350>



