---
layout: technical-post
title: Introduction to pyTorch (part 2 of 3)

---

In the second part of this three-part introduction to pyTorch, I'll be looking at automatic differentiation and tesnor operations. 

# AUTOGRAD: automatic differentiation

Central to all neural networks in PyTorch is the autograd package. Let’s first briefly visit this, and we will then go to training our first neural network.

The autograd package provides automatic differentiation for all operations on Tensors. It is a define-by-run framework, which means that your backprop is defined by how your code is run, and that every single iteration can be different.

Let us see this in more simple terms with some examples.

## Tensor

**torch.Tensor** is the central class of the package. If you set its attribute **.requires_grad** as True, it starts to track all operations on it. When you finish your computation you can call **.backward()** and have all the gradients computed automatically. The gradient for this tensor will be accumulated into **.grad** attribute.

To stop a tensor from tracking history, you can call **.detach()** to detach it from the computation history, and to prevent future computation from being tracked.

To prevent tracking history (and using memory), you can also wrap the code block in with **torch.no_grad()**:. This can be particularly helpful when evaluating a model because the model may have trainable parameters with **requires_grad=True**, but for which we don’t need the gradients.

There’s one more class which is very important for autograd implementation - a Function.

Tensor and Function are interconnected and build up an acyclic graph, that encodes a complete history of computation. Each tensor has a **.grad_fn** attribute that references a Function that has created the Tensor (except for Tensors created by the user - their **grad_fn** is None).

If you want to compute the derivatives, you can call **.backward()** on a Tensor. If Tensor is a scalar (i.e. it holds a one element data), you don’t need to specify any arguments to backward(), however if it has more elements, you need to specify a gradient argument that is a tensor of matching shape.


```python
import torch
```


```python
#create a tensor and set requires_gra=True to track computation with it. 
x = torch.ones(2, 2, requires_grad=True) 

print(x)
```

    tensor([[1., 1.],
            [1., 1.]], requires_grad=True)



```python
#do a tensor operation
y = x + 2
print(y)
```

    tensor([[3., 3.],
            [3., 3.]], grad_fn=<AddBackward0>)



```python
#y was created as a result of an operation, so it has a grad_fn
print(y.grad_fn)
```

    <AddBackward0 object at 0x105386c50>



```python
#do more operations on y
z = y * y * 3
out = z.mean()

print(z, out) 
```

    tensor([[27., 27.],
            [27., 27.]], grad_fn=<MulBackward0>) tensor(27., grad_fn=<MeanBackward1>)



```python
# .requires_grad_(...) changes an existing tesnor's requires_grad flag 
# in place. The input flag defaults to False if not given. 
a = torch.randn(2,2)
a = ((a * 3) / (a - 1))
print(a.requires_grad)
a.requires_grad_(True)
print(a.requires_grad)
b = (a * a).sum()
print(b.grad_fn)
```

    False
    True
    <SumBackward0 object at 0x113475710>


### Gradients
Let's backprop now, because out contains a single scalar, out.backward() is equivalent to out.backward(torch.tensor(1.)). 



```python
out.backward()
```


```python
#print gradients d(out)/d
print(x.grad)
```

    tensor([[4.5000, 4.5000],
            [4.5000, 4.5000]])


<img src="autograd.png">


```python
x = torch.randn(3, requires_grad=True)

y = x * 2
while y.data.norm() < 1000: 
    y = y * 2
    
print(y) 
```

    tensor([-863.3226,  941.6025,   34.9647], grad_fn=<MulBackward0>)


Now in this case y is no longer a scalar. torch.autograd could not compute the full Jacobian directly, but if we just want the vector-Jacobian product, simply pass the vector to backward as argument:


```python
v = torch.tensor([0.1, 1.0, 0.0001], dtype=torch.float)
y.backward(v) 

print(x.grad)
```

    tensor([1.0240e+02, 1.0240e+03, 1.0240e-01])


You can also stop autograd from tracking history on Tensors with **.requires_grad=True** by wrapping the code block in with torch.no_grad():You can stop autograd from tracking history 




```python
print(x.requires_grad)
print((x**2).requires_grad)

with torch.no_grad():
    print((x ** 2).requires_grad)
    
```

    True
    True
    False


#### Read Later:

Documentation of autograd and Function is at https://pytorch.org/docs/autograd


```python

```
