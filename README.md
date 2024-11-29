
### pygrad: Lightweight automatic differentiation engine in NumPy/Numba.

Documentation: https://baubels.github.io/pygrad/.

This is a lightweight (<300kB) automatic differentiation engine based on NumPy, Numba, and opt_einsum.
Included is a differentiable Tensor class, layers such as Dropout/Linear/Attention, loss functions such as BCE/CCE, optimizers such as SGD/RMSProp/Adam, and an example DNN/CNN/Transformer architecture. This library is a good alternative if you want to do backpropagation on simple and small functions or networks, without much overhead.

The main component is the `Tensor` class supporting many math operations. `Tensor`s have `.value` and `.grad` attributes, gradients being populated by calling `.backward()` on either self or any of its children. They can be used standalone, or for constructing more complex architectures such as a vanilla Transformer.

#### Usage

Tensors accept the same input value as a NumPy array. Create them with Tensor(value) or tensor.array(value).

A simple usage example:

```python
from pygrad.tensor import Tensor
x = Tensor(1)
(((x**3 + x**2 + x + 1) - 1)**2).backward()
x.grad  # 36.
```

Since `Tensor` store their value in `.value` and their gradient in `.grad`, it's easy to perform gradient descent.

```python
for _ in range(100):
    (((x**3 + x**2 + x + 1) - 1)**2).backward()     # gradients are automatically reset when called
    x.value = x.value - 0.01*x.grad
```

Tensors can also be operated on with broadcast-friendly NumPy arrays or other Tensors whose value is broadcast friendly.
Internally, a Tensor will always cast it's set value to a NumPy array.

```python
import numpy as np
x  = Tensor(np.ones((10,20)))
y  = Tensor(np.ones((20,10)))
z1 = x@y
z2 = x@np.ones((20,10))       
np.all(z1.value == z2.value)  # True
```

There are enough expressions defined to be able to create many neural networks.

**Supported Tensor Methods**

| Tensor Ops       | `pygrad.tensor`                      | Dependencies |
| ---------------  | -------------                              | ------------ |
| Magic methods    |  + - * / ** @                              | NumPy        |
| Other math ops   | sum, reshape, transpose, mean, std, conv2D | NumPy        |
| Common activations | relu, tanh, sigmoid                      | NumPy        |
| Loss              | softmax                                   | NumPy, Numba  |

**Supported Objects**

| Extraneous Ops   | Created Classes                                            | Dependencies |
| ---------------  | -------------                                              | ------------ |
| Common layers    |  ReLU, Dropout, AddNorm, Linear, Softmax, Flatten, Conv2D  | NumPy, Numba |
| Losses           | BCELoss, CCELoss                                           | NumPy, Numba |
| Optimizers       | SGD, SGD_Momentum, RMSProp, Adam                           | NumPy        |
| Architectures    | DNN, CNN, Vanila Transformer                               | NumPy/Numba  |


#### Citation/Contribution

If you'd like to contribute please do! If you find this project helpful in your research or work, I kindly ask that you cite it: [View Citation](./CITATION.cff). Thank you! 
