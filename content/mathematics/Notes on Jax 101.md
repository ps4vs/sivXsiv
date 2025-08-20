---
title: Notes on Jax
tags: []
date: "{{date}}"
---
Notes for my revision, covers
- https://docs.jax.dev/en/latest/notebooks/thinking_in_jax.html
- https://docs.jax.dev/en/latest/key-concepts.html
Future work
- https://docs.jax.dev/en/latest/tutorials.html
# Some primer

- `import jax.numpy as jnp` Vs `import numpy as np`, computations are done using numpy like interface, same code can run on cpu, gpu, tpu and local and distributed too. 
- just-in-time compilation via Open XLA.
	- In general jax operations happen one at a time
	- jax.jit decorator allows bunch of code to be optimized and run at once
- gradients can be calculated for JAX functions using automatic differentiation transformation.
- JAX functions can be automatically vectorised, ie, mapping over batch of inputs.

>[!tip]
>Use duck typing to replace jax arrays with numpy arrays, but jax arrays are immutable.

JAX arrays
- provided by jax.Array, but mostly created using jnp API.
- if x: jax.Array; x.devices() and x.sharding allows us to inspect location and sharding.

```python
import jax.numpy as jnp

def norm(X):
	X = X - X.mean(0)
	return X / X.std(0)

from jax import jit
# can be considered as norm_compiled.
jit_norm = jit(norm)

np.random.seed(0)
X = jnp.array(np.random.rand(1000, 10))
np.allclose(norm(X), jit_norm(X), atol=1E-6)

%timeit norm(X).block_until_ready()
%timeit jit_norm(X).block_until_ready()
```
JAX uses `asynchronous dispatch` so we need to use block_until_ready.

>[!note] JAX.JIT LIMITATION
>1. all arrays should have static shapes
>```python
>def get_negatives(x):
>	return x[ x < 0 ]
>x = jnp.array(np.random.randn(10))
># ERROR
>get_negatives(x)
>jit(get_negatives(x)) # will give an error

# tracing and static variables

- JIT and JAX transforms work by tracing a function
	- helps determine how function effect on inputs of specific shape and type.
	- static variables won't be traced.
```python
@jit
def f(x, y):
	print("Running f():")
	print(f" {x= }")
	print(f" {y= }")
	result = jnp.dot(x+1, y+1)
	print(f" {result= }")
	return result
x = np.random.randn(3, 4)
y = np.random.randn(4)
```

```
Running f():
	x = JitTracer<float32[3,4]
	y = JitTracer<float32[4]
	result = JitTracer<float32[3]>
```

```
Array([0.25773212, 5.3623195 , 5.403243  ], dtype=float32)
```

- tracer objects are used to extract sequence of operations, encoded in jaxpr (JAX expression)
- tracer knows only dtype and shape of arrays.
- for matching inputs no re-compilation is required.
```python
from jax import make_jaxpr

def f(x, y):
	return jnp.dot( x+1, y+1 )

make_jaxpr(f)(x, y)
```
OUTPUT
```python
{ lambda ; a:f32[3,4] b:f32[4]. let
    c:f32[3,4] = add a 1.0:f32[]
    d:f32[4] = add b 1.0:f32[]
    e:f32[3] = dot_general[
      dimension_numbers=(([1], [0]), ([], []))
      preferred_element_type=float32
    ] c d
  in (e,) }
```

JIT cannot be used on control flow statements, static variables help us here.
```python
@jit
def f(x, neg):
	return -x if neg else x

f(1, True)
```

```python
TracerBoolConversionError: Attempted boolean conversion of traced array with shape bool[].
The error occurred while tracing the function f at /tmp/ipykernel_4292/2422663986.py:1 for jit. This concrete value was not available in Python because it depends on the value of the argument neg.
See https://docs.jax.dev/en/latest/errors.html#jax.errors.TracerBoolConversionError
```

```python
from functools import partial

@partial(jit, static_argnums=(1,))
def f(x, neg):
	return -x if neg else x

f(1, True)

```
`Array(-1, dtype=int32, weak_type=True)`

If we change the static arguments then recompilation will happen.


# Derivatives with jax.grad

- jax.grad transformation is used for automatic differentiation
- jax.jacobian transformation is used to compute full jacobian matrix for vector-valued functions.

>[!note] Jacobian vs Grad
>1. jacobian is for vector valued functions, whereas, grad is for scalar valued functions.
>2. jacobian is matrix, where as grad is vector


![[Pasted image 20250820212612.png]]
**REVISIT THIS PART LATER**
- No proper knowledge of hessian, jacobian vector product and vector jacobian product.

>[!tip]
>multiple transformations can be composed grad(jit(grad(f)))(x)


# Auto-vectorization with jax.vmap()
- jax.vmap provides automatic vectorization transformation.
- **matrix-vector to matrix-matrix multiplication**
```python
from jax import random

key = random.key(1)
key1, key2 = random.split(key)
mat = random.normal(key1, (150, 100))
batched_x = random.normal(key2, (10, 100))

def apply_matrix(x):
	return jnp.dot(mat, x)

def natively_batched_apply_matrix(v_batched):
	return jnp.stack([apply_matrix(v) for v in v_batched])

def batched_apply_matrix(batched_x):
	return jnp.dot(batched_x, mat.T)
```

```python
from jax import numpy

@jit
def vmap_batched_apply_matrix(batched_x):
	return vmap(apply_matrix)(batched_x)
```


# Pseudorandom numbers

- JAX random functions consume a random key that must be split to generate new independent keys.
- JAX random key model is thread-safe and avoids issues with global state. [dig deeper]
	- numpy uses global state, set using numpy.random.seed
	- issue: global state won't work with JAX computation model, reproducibility over different threads, processes and devices.
	- solution: track the state explicitly using random key.
>[!note] The rule of thumb
>never reuse keys (unless we want identical outputs).

```python
for i in range(3):
	new_key, subkey = random.split(key)
	del key # consumed by split.
	
	val = random.normal(subkey)
	del subkey # consumed by normal.
	
	key = new_key # new_key is safe to use in next_iteration.
```

>[!tip] [PATTERN] np vs jnp & static vs traced
>use numpy for operations that should be static (i.e. done at compile time) and use jax.numpy for operations that should be traced.

Jaxpr
- jax uses tracer to extract operations performed in a function, which are stored in jaxpr syntax.
- jax.make_jaxpr
# Pytrees

- JAX functions and transformation fundamentally operate on single array.
- But in practice we encounter collection of arrays, like neural network.
- JAX relies on Pytree abstraction to treat such collections in uniform manner.

**(nested) list of params**
```python
params = [1, 2 (jnp.arange(3), jnp.ones(2))]

print(jax.tree.structure(params))
print(jax.tree.leaves(params))

# PyTreeDef([*, *, (*, *)])
# [1, 2, Array([0, 1, 2], dtype=int32), Array([1., 1.], dtype=float32)]
```
**Dictionary of params**
```python
params = {'n': 5, 'W': jnp.ones((2, 2)), 'b': jnp.zeros(2)}

print(jax.tree.structure(params))
print(jax.tree.leaves(params))
# PyTreeDef({'W': *, 'b': *, 'n': *})
# [Array([[1., 1.],
#        [1., 1.]], dtype=float32), Array([0., 0.], dtype=float32), 5]
```
**Named tuple of parameters**
```python
from typing import NamedTuple

class Params(NamedTuple):
	a: int
	b: float

params = Params(1, 5.0)
print(jax.tree.structure(params))
print(jax.tree.leaves(params))
# PyTreeDef(CustomNode(namedtuple[Params], [*, *]))
# [1, 5.0]
```

General purpose utilities for working with PyTrees
- `jax.tree.map` function is used to map a function to every leaf in a tree.
- `jax.tree.reduce` function is used to apply reduction across leaves in a tree.
  
JAX API layering: jax.numpy -> jax.lax -> XLA
# Core concepts

- Just-in-time compilation
- Automatic vectorisation
- Automatic differentiation
- Debugging
- Pseudorandom numbers
- Pytrees
- Parallel programming
- Stateful computations
- Control flow and logical operators with JIT
- Advanced concepts
	- Advanced automatic differentiation
	- External callbacks
	- Gradient checkpointing
	- Jax Internals
		- Primitives
		- The jaxpr language

# Just-in-time Compilation
