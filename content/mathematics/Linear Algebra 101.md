---
title: System of Linear Equations x Matrices
tags:
date: "{{date}}"
---
### **Intuitive concepts can be formalised by defining set of objects and set of rules to manipulate them.**

For example:
	Linear algebra is an intuitive concept
		- objects here are vectors
		- set of rules
			- addition
			- scalar multiplication
	- from abstract mathematical viewpoint, any object that carriers these two properties is a vector.
Thoughts
	Platonic Representations?? abstract concept
		- imagine a neural network model, which can be represented as graph, how can we manipulate the graph such that the results are nearly same. (set of objects)
		- What would be the set of rules to manipulate them?
	Can we learn representations of neural network models such that they can be viewed as vectors? 
		Rather than finding a space such set of operations, and defining objects. 

**Vector Space** is set of all vectors you can reach starting from a set of vectors and performing vector operations on them.

### System of Linear Equations
$$\Sigma_{j=m}^na_{ij}x_j=b_i$$
Observations, all the columns will be eliminated into one column, so we RHS can be indexed by i.

**matrices represent linear mapping and system of linear equations.**
### Operations on Matrix and Vector
- Matrix Vector multiplication can be seen as each $x_j$ scaling column vector **$c$
- Matrix Matrix multiplication is dot product b/w corresponding row and column, for $c_{ij}= a_{i,:}.b_{:,j}$
- Element wise matrix multiplication is **hadamard product**
### Properties of Matrix Multiplication
- Not commutative $BA!=AB$
- associativity ABC = (AB)C = A(BC)
- left and right distribuitivity 
	- B(C+D) = BC+BD 
	- (C+D)B = CB + DB
# Inverse $A^{-1}$
- BA = I = AB, then B is inverse of A
- exists only for square matrix. 
- properties
	- $AA^{-1} = I = A^{-1}A$
	- $(AB)^{-1} = B^{-1}A^{-1}$
	- $(A+B)^{-1}$  !=  $A^{-1}B^{-1}$
- A is called regular/invertible/non singular if $A^{-1}$ exists.
- A is called non-invertible/singular  if $A^{-1}$ doesn't exists.
# Transpose $A^T$
- aij = bji then B is transpose of A
- writing columns as rows and rows as columns
- properties
	- $(A^T)^T = A$
	- $(A+B)^T = A^T + B^T$
	- $(AB)^T = B^TA^T$

**symmetric matrices are $A^T = A$**
- only square matrices can be symmetric.
- sum of symmetric matrices can be symmetric.
- product of symmetric matrices might not be symmetric.

question: What does symmetric matrices mean in a geometric sense.

### Compact Representation of System of Linear Equations
$$Ax = b$$
The above is compact representation of system of linear equations.
- They can have zero, one or infinitely many solutions.
**Particular solution and General solution**

A system of linear equation with 2 equations and 4 unknowns
- 4 dimensional solution space, but we have only 2 dimensional information about it.
- Adding an equation to system of linear equation can be treated as constraining the exact solution space, if it is providing new solution.
- There can be case where new equation could be not intersecting with existing exact solution space, example parallel equations, then solutions will become zero.
- Rank of column also represents the dimension of exact solution space
	- Lets say two columns are equal, or one column is linear combination of many other columns
	- then variable representing the column, will be collapsing, so even when solution space looks like some m dimension, it is actually m-1 dimension.

# Find solutions to Ax = b
- General approach
	- Find particular solution of Ax = b
	- Find all solutions of Ax = 0
	- Combine solutions from steps 1 & 2 to get general solution.
- Finding particular solution (move the [A|b] augmented matrix into a easy form)
	- limiting the uncertainty of the equations, by using gaussian elimination, eliminate the number of free variables in the last equation.
	- Then work your way upwards, once you fix values for the last equation.
	- 
#### Gaussian Elimination
- Elementary transformations
	- exchange of two equations.
	- multiplication of equation with constant.
	- addition of two equations.
#### Row echelon form and Reduced Row echelon form
- **Pivot** - The leading coefficient of a row, the first non-zero element from left.
> [!note] 
> Strictly to the right of pivot above it != Strictly to the left of pivot below it 
- **Staircase structure** - The pivot is always **strictly to the right** of pivot above it.

**Row echelon form** - A matrix is in row echelon form if
- all rows that contain zero are at the bottom of matrix
- looking at non-zero rows only, pivot or leading coefficient is always strictly to the right of pivot of row above it.
How to find particular solution of Ax = b when [A|b] is in reduced row echelon form.
- express $b = \Sigma_{i=1}^{P}p_i\lambda_i$ where $p_i$, $i =1,..., P$ are pivot columns.
  
**Basic & Free variables** - the variables corresponding to the pivots in row echelon form are basic variables. And others are free variables.

**Reduced Row Echelon form (or) Row Canonical form**
- row echelon form
- every pivot is 1
- pivot is the only non-zero entry in its column.
How can you find solution, when Ax=b is in reduced row echelon form?
- you can read out the general solution of system of linear equations.
#### The Minus-1 Trick
How can you read out the solutions x of homogenous system of linear equations Ax = b, where $A \in R^{k \cross n}$ , $x \in R^n$ ?
1. assume A is in reduced row echelon form without any rows that just contain zeros, where * can be arbitrary real number
![[Pasted image 20250902000911.png]]
2. The columns $j_1$, ..., $j_k$ with pivots (marked in bold) are standard unit vectors $e_1$, ..., $e_k$ $\in R^k$. Then extend the matrix to an n x n matrix $A^{\tilda}$ by adding n - k rows of the form.
   ![[Pasted image 20250902001244.png]]
3. The diagonal of augmented matrix $A^\tilda$ contains either 1 or -1. Then the columns of $A^\tilda$ that contain -1 as pivots are solutions of the homogenous equation system **Ax = 0**, which we later call the **kernel space or the null space**.
#### Calculating the Inverse
Find a matrix **X** that satisfies $AX = I_n$, then $X=A^{-1}$, it is simply **solving set of simultaneous linear equations**.
![[Pasted image 20250902001927.png]]
If we bring the augmented matrix into reduced row echelon form, inverse will be on RHS.
- determining inverse of a matrix is solving system of simultaneous linear equations.
#### Algorithms for solving system of linear equations
- We make assumption that solution exists, if there is no solution then approximate solutions such as linear regression.
In special cases, we might have $A^{-1}$, such cases solution of system of linear equation $Ax = b$ is $x=A^{-1}b$, 
	This is only possible with square and invertible matrices.
Otherwise, A needs to have linearly independent columns, then using transformation.
	$Ax =b$  <=>  $A^TAx = A^Tb$  <=>  $x = (A^TA)^{-1}A^Tb$

**It is called Moore-Penrose pseudo-inverse $(A^TA)^{-1}A^T$ to solve Ax = b, which also corresponds to minimum least squares solution.**
- issues with calculating Moore-Penrose pseudo inverse and $A^{-1}$ is computationally expensive and numerical precision.
  
**Gaussian elimination** is used to calculate rank of a matrix, determine basis of vector space and checking whether set of vectors are linearly independent. **Yet it is impractical due to operations scaling cubically with number of simultaneous equations.** Why cubically?

### Iterative methods Summary
In practice, many linear equations are solved indirectly, by either stationary iterative methods, such as Richardson method, the Jacobi method, the Gaus-Seidel method, and successive over-relaxation method or Krylov subspace methods, such as conjugate gradients, generalised minimal residue, or biconjugate gradients.

**Key idea of iterative methods**
- Let $x_*$ be solution of Ax = b, the key idea is write $$x^{k+1}=Cx^k+d$$ for suitable C and d that reduces the residual error $||x^{k+1} - x_*||$ in every iteration, and converges.






