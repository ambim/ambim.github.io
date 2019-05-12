
# Intro to pyTorch
pyTorch is an open source deep learning platform that provides a seamless path from research prototyping to production deployment.


```python
from __future__ import print_function
import torch
```

Constructing a 5x3 matrix


```python
x = torch.empty(5,3)
print(x)
```

    tensor([[ 0.0000e+00,  1.0842e-19, -3.1118e-18],
            [ 8.5920e+09,  4.9371e-37,  1.4013e-45],
            [ 9.2354e-37,  1.4013e-45,  6.4097e-10],
            [ 6.4097e-10,  6.4097e-10,  6.4097e-10],
            [ 6.4097e-10,  6.4097e-10,  6.4097e-10]])


Constructing a randomly initialized matrix


```python
x = torch.rand(5,3)
print(x)
```

    tensor([[0.4258, 0.3095, 0.4439],
            [0.6245, 0.1630, 0.9765],
            [0.9851, 0.8920, 0.7059],
            [0.4985, 0.9556, 0.0502],
            [0.4168, 0.9831, 0.0360]])


Construct a matrix filled with zeros and of dtype long


```python
x = torch.zeros(5,4,dtype=torch.long)
print(x)
```

    tensor([[0, 0, 0, 0],
            [0, 0, 0, 0],
            [0, 0, 0, 0],
            [0, 0, 0, 0],
            [0, 0, 0, 0]])


Construct a tensor directly from data


```python
x = torch.tensor([5.5, 3])
print (x)
```

    tensor([5.5000, 3.0000])


Create a tensor based on an existing tensor. These methods will reuse properties of the input tensor, e.g. dtype, unless new values are provided by user


```python
x = x.new_ones(5, 3, dtype=torch.double) # new_* method takes in sizes
print(x)

x = torch.randn_like(x, dtype=torch.float)
print(x)

```

    tensor([[1., 1., 1.],
            [1., 1., 1.],
            [1., 1., 1.],
            [1., 1., 1.],
            [1., 1., 1.]], dtype=torch.float64)
    tensor([[ 0.0462, -1.2396, -0.0415],
            [ 0.8868, -1.5131,  1.5147],
            [ 1.0145,  0.2150,  1.2778],
            [ 0.7786, -0.1636, -0.3087],
            [-0.0846,  0.8452,  0.2403]])


Get it's size


```python
print(x.size())
```

    torch.Size([5, 3])


## Operations

Addition


```python
y = torch.rand(5,3)
print(x+y)
```

    tensor([[ 5.6409e-02, -8.4203e-01,  9.3244e-02],
            [ 1.6478e+00, -6.7512e-01,  2.4597e+00],
            [ 1.7626e+00,  8.3986e-01,  2.0402e+00],
            [ 1.4260e+00,  7.9624e-01, -2.6055e-02],
            [-1.9657e-03,  9.0799e-01,  5.5779e-01]])



```python
#or method 2
print(torch.add(x, y))
```

    tensor([[ 5.6409e-02, -8.4203e-01,  9.3244e-02],
            [ 1.6478e+00, -6.7512e-01,  2.4597e+00],
            [ 1.7626e+00,  8.3986e-01,  2.0402e+00],
            [ 1.4260e+00,  7.9624e-01, -2.6055e-02],
            [-1.9657e-03,  9.0799e-01,  5.5779e-01]])



```python
# Addition: providing an output tensor as argument

result = torch.empty(5,3)
torch.add(x, y, out=result)
print(result)
```

    tensor([[ 5.6409e-02, -8.4203e-01,  9.3244e-02],
            [ 1.6478e+00, -6.7512e-01,  2.4597e+00],
            [ 1.7626e+00,  8.3986e-01,  2.0402e+00],
            [ 1.4260e+00,  7.9624e-01, -2.6055e-02],
            [-1.9657e-03,  9.0799e-01,  5.5779e-01]])



```python
# addition in place
y.add_(x)
print(y)
```

    tensor([[ 5.6409e-02, -8.4203e-01,  9.3244e-02],
            [ 1.6478e+00, -6.7512e-01,  2.4597e+00],
            [ 1.7626e+00,  8.3986e-01,  2.0402e+00],
            [ 1.4260e+00,  7.9624e-01, -2.6055e-02],
            [-1.9657e-03,  9.0799e-01,  5.5779e-01]])


### Note: Any operation that mutates a tensor in-place is post-fixed with an _. For example: x.copy_(y), x.t_(), will change x.

standard numpy indexing available: 


```python
print(x[:,1])

```

    tensor([-1.2396, -1.5131,  0.2150, -0.1636,  0.8452])


Resizing/ shaping the tensor 


```python
x = torch.randn(4,4)
y = x.view(16) 
z = x.view(-1,8) #size -1 is inferred from other dimensions
print(x.size(), y.size(), z.size() )
```

    torch.Size([4, 4]) torch.Size([16]) torch.Size([2, 8])



```python
print(x)
print(y)
print(z)
```

    tensor([[ 1.6827,  0.8553,  0.7791, -1.3447],
            [ 0.2892,  0.8515, -0.7454, -0.0041],
            [-1.7828,  0.8462, -0.1214,  0.0126],
            [-0.1525, -1.4152,  0.3365, -0.5977]])
    tensor([ 1.6827,  0.8553,  0.7791, -1.3447,  0.2892,  0.8515, -0.7454, -0.0041,
            -1.7828,  0.8462, -0.1214,  0.0126, -0.1525, -1.4152,  0.3365, -0.5977])
    tensor([[ 1.6827,  0.8553,  0.7791, -1.3447,  0.2892,  0.8515, -0.7454, -0.0041],
            [-1.7828,  0.8462, -0.1214,  0.0126, -0.1525, -1.4152,  0.3365, -0.5977]])


If you have a one element tensor, use .item() to get the value as a Python number


```python
x = torch.randn(1)
print(x) 
print(x.item())
```

    tensor([0.7438])
    0.7437915205955505


## The numpy bridge

Converting a Torch Tensor to a NumPy array and vice versa is a breeze.

The Torch Tensor and NumPy array will share their underlying memory locations, and changing one will change the other.

Converting a Torch Tensor to a NumPy Array


```python
a = torch.ones(5)
print(a)
```

    tensor([1., 1., 1., 1., 1.])



```python
b = a.numpy()
print(b)
```

    [1. 1. 1. 1. 1.]



```python
a.add_(1)
print(a)
print(b)
```

    tensor([2., 2., 2., 2., 2.])
    [2. 2. 2. 2. 2.]


Converting numpy array to torch tensor


```python
import numpy as np
a = np.ones(5) 
b = torch.from_numpy(a)
np.add(a, 1, out=a) 
print(a) 
print(b)
```

    [2. 2. 2. 2. 2.]
    tensor([2., 2., 2., 2., 2.], dtype=torch.float64)


### CUDA tensors

Tensors can be moved onto any device using the .to method. 


```python
# let us run this cell only if CUDA is available 
# we will use ''torch.device'' objects to move tensors in and out of GPU

if torch.cuda.is_available():
    device = torch.device("cuda") # CUDA device object
    y = torch.ones_like(x, device=device)
    x = x.to(device) 
    z = x + y
    print(z) 
    print(z.to("cpu", torch.double))
```


```python

```


```python

```


```python



```
