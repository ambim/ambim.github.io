---
layout: technical-post
title: Neural nets 101 - support vector machines 

---

Although not strictly a neural net, here we introduce the Support Vector Machine. 


# NN04 - Support vector machines

## Core concepts
In SVMs we're looking for the hyperplane with the largest distance to the nearest training data point of any class (**functional margin**). 
<img src="svm.png",width=300,height=300>

To achieve this, we need to: 
1. input data in vector space, 
2. if the data is non-linearly separable, use the kernel trick to simulate transform data into higher-dimensional space (until classes can be separated by a hyperplane). Details of how this is achieved are outlined below.  
3. construct parallel hyperplanes to separate classes. 
4. convex optimisation of these hyperplanes to maximise functional margin between support vectors. 

**Advangates of SVM**:   
- regularisation parameter, which makes the user think about avoiding over-fitting. 
- kernel trick, so you can build in expert knowledge about the problem via engineering the kernel. 
- SVM is defined by a convex optimisation problem (no local minima) for which there are efficient methods (e.g. SMO). - it is an approximation to a bound on the test error rate, and there is a substantial body of theory behind it which suggests it should be a good idea.

**Disadvantages of SVM**: 
-  In a way the SVM moves the problem of over-fitting from optimising the parameters to model selection. Sadly kernel models can be quite sensitive to over-fitting the model selection criterion, see G. C. Cawley and N. L. C. Talbot, Over-fitting in model selection and subsequent selection bias in performance evaluation, Journal of Machine Learning Research, 2010. Research, vol. 11, pp. 2079-2107, July 2010.) 


#### Implementation: 
**SKLearn input parameters**
sklearn.svm.SVC(C=1.0, kernel='rbf', degree=3, gamma=0.0, coef0=0.0, shrinking=True, probability=False,tol=0.001, cache_size=200, class_weight=None, verbose=False, max_iter=-1, random_state=None)


- **Kernel** - as discussed. 
- **Gamma** - Kernel coefficient for ‘rbf’, ‘poly’ and ‘sigmoid’. Higher the value of gamma, will try to exact fit the as per training data set i.e. generalization error and cause over-fitting problem.
- **C** - Penalty parameter C of the error term. It also controls the trade off between smooth decision boundary and classifying the training points correctly.

## ===============================================
## Underlying mathematical concepts explained in more detail

In SVMs we're looking for the hyperplane with the largest distance to the nearest training data point of any class (**functional margin**). 

<img src="svm.png",width=300,height=300>

#### Functional margin
Linear classifier *h* for a binary classification problem with labels *y* in {-1,1} and input vectors (features) *x*: 
<img src="margin.png",width=200,height=200>
where *g(z)=1 if z>= 0; g(z)= -1* otherwise. Given training example (x<sup>(i)</sup>, y<sup>(i)</sup>), the functional margin of (w,b) w.r.t (x<sup>(i)</sup>, y<sup>(i)</sup>) is defined as: 
<img src="margin2.png",width=200,height=200>
If *y<sup>(i)</sup>=1*, for the margin to be large then *w<sup>T</sup>x+b* needs to be a large positive number.  

#### Normalization
<img src="norm.png",width=200,height=200>
Scaling (w,b) can make the margin arbitrarily large without really changing anything. 

Solution: replace (w,b) with (w/||w||,*b*/||w||). Given a set of examples S, we're interested in maximising the distance from (w,b) to the smallest of the functional margins in S. 

#### Convex optimisation problem
Constraint: make functional margin w.r.t S equal to 1. Note that maximising 1/||*w*|| is the same as minimising ||*w*||<sup>2</sup>. 
<img src="opt.png",width=300,height=300>
i.e. quadratic objective (a.k.a cost) function with only linear constraints = computationally efficient for large m (but not for very large m). 

#### Support vectors
<img src="supportvectors.png",width=300,height=300>
<img src="svm2.png",width=300,height=300>
α<sub>i</sub> is non-zero only for the three points in the parallel lines. 

**Using lagrange duality**:
<img src="lagrange.png",width=350,height=350>
If we manage to find the above α<sub>i</sub>'s, test for new point *x* only needs to calculate inner products between *x* and the support vectors. 
<img src="ld.png",width=250,height=250>

#### Solving the XOR problem, with feature mapping 
Solving the XOR problem with a single perceptron can be achieved with an increase in dimensionality. 

Solving XOR with an SVM requires feature mapping, e.g. if x<sub>i</sub> in {-1,1}, let φ(X) = -x<sub>1</sub>x<sub>2</sub>
<img src="MAPPING.png",width=250,height=250>

#### SVM kernels
Given a feature mapping φ we define a Kernel between two data points *x* and *z* as:
*K(x,z) = φ(x)<sup>T</sup>φ(z)* 

e.g. new point *x* and support vector *z*. For high-dimensional vectors, calculating φ(x) may be very expensive, but not calculating, say, *K(x,z) = (x<sup>T</sup>z)<sup>2</sup>*

#### Mercer's theorem 
Given a function K how can we tell that it is a valid Kernel, i.e. that there is a feature mapping φ such that *K(x,z) = φ(x)<sup>T</sup>φ(z)*  for all x, z? 

Given m data points, let Kernel matrix **K** be m x m matrix with i,j entry equal to K(x<sup>i</sup>, x<sup>j</sup>). Then K is valid if **K** is symmetric, positive and semi-definite. 

#### Kernel trick 
If a learning algorithm can be written in therms of inner products "<x,z>" between input vectors x and z then the use of polynomial kernel *K(x,z) = (x<sup>T</sup>z)<sup>2</sup>* or Gaussian kernel K(x,z)=exp(|x-z|<sup>2</sup>/2σ<sup>2</sup>) offers an improvement in computational efficiency by avoiding the computation of the feature mapping φ(x). 
We simply replace all inner products x,y in an SVM computation by K(x,z)

#### Regularisation
<img src="svm_reg.png",width=450,height=450>
(ref. Andrew Ng) 






