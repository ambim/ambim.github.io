---
layout: technical-post
title: Neural nets 101 - Formulas

---

Here is an overview of some common mathematical formulas from the field of neural networks. 

## ====================================
## Perceptron 

#### Calculating output
<img src="f_perc.png",width=200,height=200>


#### Calculating error

#### Changing weights

<img src="f_plearn.png",width=180,height=180>


## Activation functions

<img src="activ.png",width=900,height=800>

## ====================================
## Backpropagation 

#### Gradient of the error function: 
<img src="f_bp.png",width=200,height=200>

#### Gradient descent

<img src="f_gd.png",width=200,height=200>

<img src="f_bp2.png",width=250,height=250>

#### ... tries to minimise the network's global error function: 
<img src="f_error.png",width=200,height=200>

#### Local error 
<img src="f_lerror.png",width=250,height=250>

#### Momentum 
<img src="f_mom.png",width=300,height=250>

#### Weight decay 
<img src="f_wd.png",width=300,height=250>


## ====================================
## Hebb's rule 
**Learning rule for bipolar activation**:  
<img src="f_hebb.png",width=180,height=180>

#### Sign activiation function

<img src="f_sign.png",width=180,height=180>
Where: 
- *sign(x) = 1 if x > 0*
- *sign(x) = -1 if x < 0 *
- *sign(x) = A<sub>i</sub>(t) if x = 0*

## ====================================
## Hopfield networks

#### Storage phase
<img src="f_hop.png",width=180,height=180>

#### Hopfield energy function 
<img src="f_energy.png",width=300,height=250>


## ====================================
## Self-organizing maps (SOM) 
#### Minimising Euclidean distance in competition phase
<img src="f_min.png",width=180,height=180>

#### Cooperation
<img src="f_coop.png",width=250,height=180>

#### Synaptic adaptation
<img src="f_syn.png",width=200,height=180>


## ====================================
## Boltzmann machines

#### Energy of a BM 
<img src="f_ebm.png",width=300,height=200>

#### Difference in energy when a single unit i is flipped from off to on
<img src="f_edelta.png",width=400,height=200>

#### Gibbs sampling 
<img src="f_gibbs.png",width=200,height=200>

#### Boltzmann learning rule 
<img src="f_boltz.png",width=180,height=180>

## ====================================
## Support vector machines (SVM)

#### Calculating functional margin
<img src="f_fm.png",width=400,height=200>

#### Normalization
<img src="f_norm.png",width=200,height=200>

#### Convex optimization 
<img src="f_co.png",width=300,height=200>

#### Support vectors
<img src="f_sv.png",width=350,height=250>

#### Lagrange duality
<img src="f_lag.png",width=350,height=250>
<img src="f_lag2.png",width=400,height=300>

#### Calculating a feature mapping (too expensive) 
<img src="f_map.png",width=150,height=150>

#### Polynomial kernel trick 
<img src="f_poly.png",width=100,height=130>

#### Gaussian kernel trick
<img src="f_gaus.png",width=160,height=160>

