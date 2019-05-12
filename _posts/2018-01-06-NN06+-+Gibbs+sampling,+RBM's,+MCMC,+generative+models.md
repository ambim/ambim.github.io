---
layout: technical-post
title: Neural nets 101: Restricted boltzmann machines	

---

All about RBMs. 



# NN06 - Gibbs sampling, RBM's, MCMC, generative models

In this notebook: 
- gibbs sampling
- boltzmann machines
- RBMs
- generative models
- contrastive divergence



## Boltzmann machines
A Boltzmann machine: 
- is a stochastic variant of the Hopfield network. 
- it uses the Boltzmann distribution as a sampling function. 
- good for learning joint data distributions. 
- slow in practice, but efficient with restricted connectivity. 

Now neurons are *on* (resp. *off*) __with probability__ *p<sub>i</sub>*. The energy E<sub>i</sub> of a Boltzmann machine is similar to that of a Hopfield net. The use of bias (denoted W<sub>0i</sub> or θ<sub>i</sub>): 
<img src="energy2.png",width=400,height=400>

The network learns a joint distribution on the visible units. 
<img src="bm.png",width=200,height=200>
The difference in energy when single unit *i* is flipped from *off* (zero) to *on* (one) is given by: 
<img src="energy3.png",width=400,height=400>

#### Boltzmann learning rule
- **Positive phase:** visibile units' states are clamped to a particular binary state vector sampled from the training set 
- **Negative phase:** the network is allowed to run freely, i.e. no units have their state determined by external data. 
<img src="learnings.png",width=200,height=200>
- *p+* is probability of units *i* and *j* both being on when the machine is at equilibrium on the positive phase. 
- *p-* is probability of units *i* and *j* both being on when the machine is at equilibrium on the negative phase.

#### Gibbs sampling
1. Start with an arbitrary state
2. Neurons are repeatedly visited in order
3. Calculate new value for neuron i in {0,1} according to: 

p<sub>i</sub> = 1/ (1 +exp(-∆E<sub>i</sub>/T)) ...until the network reaches thermal equilibirum. 

#### MCMC (Markov chan Monte Carlo) 
Markov chain monte carlo methods (like Gibbs sampling provide a way of approximating the value of an integral...

**Monte Carlo** = random sampling i.e. from a uniform distribution

**Markov chain** = walking the right way (sampling from a distribution P(x) that is not uniform); i.e. make the likelihood of visiting a point *x* proportional to P(x) so that x<sup>t+1</sup> depends only on x<sup>t</sup> 

#### Combining probability models
- **Mixture**: Take a weighted average of the distributions (never to be sharper than the individual distributions). 
- **Product**: multiply the distributions at each point and then re-normalize (this is how an RB combines the distributions defined by the hidden units). More powerful than a mixture, but the normalization makes maximum likelihood learning difficult; but approximations allow learning. 
- **Composition** - use the values of the latent variables of one model as the data for the next model. Works well for learning multiple layers of representation, but only if the individual models are undirected (i.e. symmetrical networks). 

#### Generative neural networks
If we connect binary stochastic neurons in a directed acyclic graph we get a **Sigmoid Belief Net** (Neal, 1992). 

If we connect binary stochastic neurons using symmetric connections we get a **Boltzmann machine** (Hinton and Sejnowski, 1983). If we restrict the connectivity in a special way we get a **Restricted Boltzmann Machine**, which is easy to learn. 
    
## Restricted Boltzmann Machine (RBM) 
(*...combining all the above elements*) 
<img src="rbm.png",width=200,height=200>
Restrict the connectivity of the Boltzmann machine to make learning easier: 
- only one layer of hidden units (apart from a deep BM, see below). 
- no connections between hidden units or between visible units. 

Thus, in an RBM, the hidden units are conditionally independent given the visible states. 

**Energy of an RBM**: 
<img src="energy4.png",width=400,height=400>

**RBM learning**: 
<img src="rbmlearn.png",width=400,height=400>

**Contrastive divergence (CD1) method of learning an RBM**: 
<img src="cd1.png",width=400,height=400>

