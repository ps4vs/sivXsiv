---
title: Vector Spaces 101
tags:
date: "{{date}}"
---
Lets start with few simple questions, and intuitive answers.
What are **Vectors**?
	objects that can be added together and multiplied by a scalar to produce only objects of same type.
What is a **Vector Space**?
	**structured space in which vectors live** is called **vector space**.
What is a **Group**?
	**set of elements and operation** defined on them that keeps some structure of set intact.


### Group
Consider a **set S** and **an operation $\otimes$: S x S -> S**, then G := (S, $\otimes$) is called a group if the following holds:
1. Closure of S under $\otimes: \forall x, y \in S: x \otimes y \in S$
2. Associativity: $\forall x, y, z \in S: (x \otimes y)\otimes z = x \otimes (y \otimes z)$
3. Neutral element: $\exists e \in S, \forall x \in S: x \otimes e = x$ and $e \otimes x = x$
4. Inverse element: $\forall x \in S, \exists y \in G: x \otimes y = e$ and $y \otimes x = e$, we write it as $x^{-1}$

If additionally $\forall x, y \in S: x \otimes y = y \otimes x$, **Commutative**, then $G = (S, \otimes)$ is an **Abelian Group**.

>[!tip]
>The inverse element is defined wrt operation $\otimes$, so not necessarily 1/x
>
>The worst Mnemonic to remember Group & Abelian Group "CIA-E" and "CIA-EC"
>
>C -> Closure
>I  -> Identity
>E -> Existence of Inverse
>A  -> Associativity
>C -> Commutative

 **General Linear Group**
	 The set of regular (invertible) matrices $A \in R^{n\times n}$ is a group with respect to matrix multiplication and is called general linear group. Not abelian since not commutative.

### Vector Spaces
![[Pasted image 20250908205614.png]]
	The neutral element of (V, +) is the zero vector **0** = [0, ..., 0]^T
	The **inner operation** + is called vector addition.
	The elements $\lambda \in R$ are called scalars.
	The **outer operation** . is called multiplication by scalars.

$ab^T \in R^{n \times n}$ is called **outer product**
$a^Tb \in R$ is called **inner/scalar/dot product**.

>[!note]
>Inner product, Outer product are different from Inner operation and Outer operation.

If Inner product and Outer product are standard vector addition and scalar multiplication, then we denote (V, +, .) by simply V.

### Vector Subspaces
Intuitively, when we perform vector space operations we will stay in the same set.

Let V = (V, +, .) be a vector space and $U \subseteq V, U \neq \Phi$, then U = (U, +, .) is called **vector subspace** of V (or **linear subspace**) if U is a vector space with vector space operations + and . on U are restricted to U x U and R x U.

hehehe, U naturally inherits many properties of V, ie, abelian group properties, distributivity, associativity and neutral element.

We just need to check the following to understand whether U is vector subspace or not.
1. U $\neq \Phi$, in particular: **0** $\in U$
2. Closure of U:
	1. wrt outer operation
	2. wrt inner operation

>[!Remark]
>Every subspace $U \subseteq (R^n, +, .)$ is solution space of homogenous system of linear equation **Ax = 0** for **x** $\in R^{n\times n}$

### Linear Independence
- **closure property** of vector guarantees that vector operations will end with another vector in the same vector space.
- Is it possible to find a set of vectors with which we can represent every vector in vector space?
![[Pasted image 20250908235916.png]]
Trivial solution is when all $\lambda_i$ are zero, then only we get **0** vector.

The vectors k >= 2 are linearly dependent if and only if one of them is linear combination of others.

What is Practical Way of checking linear (in)dependence?
- Perform Gaussian elimination on matrix A, where $x_i$ are column vectors, until matrix is in row echelon form.
- The pivot columns indicate vectors that are linearly independent of vectors on the left.
- The non-pivot columns can be expressed as linear combination of pivot columns on their left.
![[Pasted image 20250909000554.png]]

**If we know basis vectors are linearly independent, we can just check for linear independence of coefficient vectors.**

### Basis and Rank

**Generating Set and Span**
![[Pasted image 20250909000944.png]]

**Basis**
![[Pasted image 20250909001000.png]]

**Equivalent Statements about basis of V.**
![[Pasted image 20250909001110.png]]


**Dimension of Vector Space** is number of basis vectors of V, and number of independent directions in the vector space V.

The Dimension of vector space is not necessarily the number of elements in a vector. V = span\[\[0, 1\]^T\] is one-dimensional.

### RANK

![[Pasted image 20250909001607.png]]

### Linear Mappings
also called **Vector space homomorphism** or **Linear transformation**
	Mappings on vector space that preserve their structure, these mappings allows to us to define concept of coordinate.

Consider two real vector spaces V, W. A mapping $\Phi$: V -> W preserves the structure of vector space if for all **x, y** $\in$ V and $\lambda \in$ R.
$$\Phi(x+y) = \Phi(x) + \Phi(y)$$
$$\Phi(\lambda x) = \lambda \Phi(x)$$
**together can be represented as**
![[Pasted image 20250909094815.png]]
>[!note]
>**Two representations of Matrices**, a linear mapping or collection of vectors.

#### Injective, Surjective and Bijective Mappings
- Injective is one-to-one, each element maps to only one element.
- Surjective is onto, all elements from V are mapped to all elements from W, ie, codomain is range.
- Bijective is one-to-one and onto.

Consider a mapping $\Phi$: V -> W, where V, W are arbitrary sets. Then $\Phi$ is called.
- Injective if $\forall x, y \in V$: $\Phi(x)=\Phi(y)$ => **x** = **y**.
- Surjective if $\Phi(V) = W$.
- Bijective if it is injective and surjective.

A bijective mapping can be "undone", ie, inverse exists from W to V, such that $\psi \circ \Phi(x) = x$, then $\psi$ is called $\phi^{-1}$


#### Special cases of Linear Mapping
- Isomorphism
- Endomorphism
- Automorphism
- Identity mapping or identity automorphism in V, $id_V$: V -> V 

![[Pasted image 20250909100229.png]]
### Important Theorem
Finite-dimensional vector spaces V and W are isomorphic **if and only if** dim(V) = dim(W)

	The theorem states that vector spaces of same dimension are same thing, and they can be transformed into each other without incurring any loss. 

	$R^{m \times n}$ is $R^{mn}$, since they are of same dimension, ie, there exists a bijective mapping that transforms one into the other.

>[!Remarks]
>1. For linear mappings $\Phi$: V -> W and $\Psi$: W -> X, the mapping $\Psi \circ \Phi$: V -> X is also linear
>2. If $\Phi$: V -> W is an isomorphism, then $\Phi^{-1}$: W -> V is an isomorphism too.
> 3. If $\Phi$: V -> W, $\Psi$: V -> W are linear, then $\Phi+\Psi$ and $\lambda\Phi, \lambda \in$ R are linear too.

### Matrix Representation of Linear Mappings
Any n-dimensional vector space is isomorphic to $R^n$.

Consider a **basis {$b_1, ..., b_n$} of an n-dimensional vector space V**, order of basis is important, so B = ($b_1, ..., b_n$), and **call this n-tuple an ordered basis** of V.

![[Pasted image 20250909112137.png]]Ordered basis
Unordered basis
Matrix whose columns are vectors b_1, ..., b_n.

#### Coordinates of x w.r.t ordered basis B

![[Pasted image 20250909112303.png]]

Instead of storing all coordinates of vector of vector space V, we just store the dim(V) number of basis vectors, and define coordinates w.r.t these ordered basis.



![[Pasted image 20250909112545.png]]
# Connection b/w matrices and linear mappings b/w finite dimensional vector spaces.

### Transformation Matrix

Consider a vector spaces V, W with corresponding (ordered) bases B = ($b_1, .., b_n$) and C = ($c_1, ..., c_m$). Moreover consider a linear mapping $\Phi$: V -> W. For j $\in$ {1, ..., n},

$$\Phi(b_j) = \alpha_{1j}c_1 + ... + \alpha_{mj}c_m = \Sigma_{i=1}^m \alpha_{ij}c_i$$
is unique representation of $\Phi(b_j)$ w.r.t C. 

Then, we call the m x n matrix **A**$_{\Phi}$, whose elements are given by **A**$_{\Phi}(i, j)$ = $\alpha_{ij}$, the **transformation matrix** of $\Phi$ (w.r.t ordered bases B of V and C of W).

**The coordinates of $\Phi(b_j)$ w.r.t the ordered basis C of W are the j-th column of A$_\Phi$.**

Consider (finite-dimensional) vector spaces V, W with ordered bases B, C and a linear mapping $\Phi$: V -> W with transformation matrix **A**$_{\phi}$.

If $\hat{x}$ is the coordinate vector of x $\in$ V w.r.t B, and $\hat{y}$ the coordinate vector of y = $\Phi(x)$ $\in$ W w.r.t C, then
![[Pasted image 20250909115008.png]]
![[Pasted image 20250909115106.png]]
![[Pasted image 20250909115117.png]]

### Basis Change
![[Pasted image 20250925114130.png]]

Proof: 
- vectors in bases b, can be written as vectors in bases $b^\tilda$, by expressing old bases as linear combination of new bases, and then taking transpose of matrix representing this system of linear equations.
- note both transformations matrices S and T are regular.
- ![[Pasted image 20250925114435.png]]
- ![[Pasted image 20250925114442.png]]
- ![[Pasted image 20250925114612.png]]
>[!note]: Pictorial Pictorial Way of arriving at the proof using composition of linear mappings
>![[Pasted image 20250925114849.png]]
>
![[Pasted image 20250925114649.png]]


$Id_V$ and $Id_W$ are Identity mappings that maps the vectors onto themselves in a different bases.

### Equivalence and Similar Matrices
![[Pasted image 20250925115131.png]]

### Image and Kernel 
Image and Kernel of linear mapping are vector spaces with certain important properties.
![[Pasted image 20250925115548.png]]
Remarks
- Null/Kernel space is never empty, because 0 is always part of it.
- Ker($\Phi$) is subspace of V and Im($\Phi$) is subspace of W
- $\Phi$ is injective (one-to-one) if and only if ker($\Phi$) = {0}
![[Pasted image 20250925124008.png]]

- Span of columns of A or column space are Img($\Phi$), column space is subspace of R^m, where m is height of matrix.
- Kernel/Null space are set of general solutions to homogenous equations Ax = 0, captures all possible linear combinations of the elements in R^n that produce 0 ∈ R^m, where n is width of matrix.

#### Rank-Nullity Theorem
Also known as **fundamental theorem of linear mappings**.

For vector spaces V and W and a linear mapping $\Phi$: V -> W it holds that
$$dim(Img(\Phi)) + dim(Ker(\Phi)) = dim(V)$$
![[Pasted image 20250925125144.png]]

### Affine Spaces
Affine spaces are spaces that are offset from origin, ie, spaces that are no longer vector subspaces. There are some properties of affine spaces which resemble linear mappings.

![[Pasted image 20250925125407.png]]
Examples of affine subspaces are points, lines and planes of R^3 that doesn't necessarily go through origin.

The definition of affine subspace excludes **0** for $x_0 \notin U$, therefore affine subspace is not linear mapping or vector subspace for $x_0 \notin U$

![[Pasted image 20250925133752.png]]

Affine subspaces are often described by parameters: Consider a k-dimensional affine subspace L = x_0 + U of V. If (b_1, ..., b_k) are ordered basis of subspace U, then every element x $\in$ L can be uniquely described using $$x = x_0 + \lambda_1 b_1 + ... + \lambda_k b_k$$
where $\lambda_i \in R$. This representation is called parametric equation of affine subspace L with directional vectors $b_i$ and parameters $\lambda_i$.

### hyperplanes, planes and lines

![[Pasted image 20250925134403.png]]
![[Pasted image 20250925134413.png]]
![[Pasted image 20250925134423.png]]

### Affine subspace and non-homogenous System of Linear Equns
![[Pasted image 20250925134544.png]]
Every k-dimensional affine subspace is solution of non-homogenous system of linear equations Ax = b, where rk(A) = n - k.

### Affine Mappings
Similar to linear mapping b/w vector subspaces, we have affine mappings b/w affine subspaces.

For two vector subspaces V and W, a linear mapping $\Phi$: V -> W and a $\in$ W, the mapping
$\Phi^{'''}$: V -> W, defined as x -> a + $\Phi$(x) is affine mapping from V to W. The vector a is called translation vector of $\Phi^{'''}$.
![[Pasted image 20250925135343.png]]


