---
title: t-SNE
tags:
  - sapling
enableToc: true
date: "{{date}}"
---
### t-distributed Stochastic Neighbour Embedding

t-SNE is a non-linear dimensionality reduction, ie, it allows us to separate data which cannot be separated by any straight line.
![[Pasted image 20240129112542.png]]
![[Pasted image 20240129130138.png]]
>[!question] t-SNE vs PCA
>- t-SNE is iterative, so unlike PCA it cannot be applied to another dataset.
>- t-SNE is used to understand high-dimensional data, and project it into low-dimensional space (like 2D or 3D).

### Detailed Explanation

- **Problem**: To understand high-dimensional datasets, less useful to perform dimensionality reduction for ML training.
- Details:
	- t-SNE cannot be reapplied similar to PCA, since t-SNE is iterative and non deterministic.
	- STEP 1: Creating similarities, ie, probability distribution.
		- Create probability distribution that represents similarities between neighbours. 
		- Here similarity of datapoint $x_j$ to datapoint $x_i$ is the conditional probability $p_{j|i}$, that $x_i$ would pick $x_j$ as its neighbour.
		- This similarity is proportional to probability density under Gaussian centered at $x_i$.
		- ![[Pasted image 20240129130119.png]]
		- ![[Pasted image 20240129130221.png]]
		- ![[Pasted image 20240129130235.png]]
		- We can distinguish b/w similar and non-similar points, but absolute values of probability are much smaller than in first example (compare Y-axis values).
		- We can fix that by normalisation $$p_{j|i} = \frac{g(|x_i - x_j|)}{\Sigma_{k!=i}g(|x_i - x_k|)}$$
		- This scales all values to have a sum equal to 1. We set $p_{i|i}=0$, not 1.
	- STEP 2: Dealing with different distances
		- ![[Pasted image 20240129130947.png]]
		- ![[Pasted image 20240129130955.png]]
		- ![[Pasted image 20240129131008.png]]
		- Where N is number of dimensions.
	- STEP 3: Final formula
		- We haven't used any variance, for gaussian distribution.
		- ![[Pasted image 20240129131243.png]]
		- This is the final formula
	- STEP 4: Create low-dimensional space
		- Gaussian distribution has "short tail", so it creates crowding problem.
		- To solve it we use Student t-distribution with a single degree of freedom.
		- ![[Pasted image 20240129131426.png]]
	- Visual comparison between Gaussian and Student t-distribution
	- ![[Pasted image 20240129131514.png]]
### The Gradient Descent

To optimise this distribution t-SNE is using Kullback-Leibler divergence between the conditional probabilities $p_{j|i}$ and $q_{j|i}$.

![[Pasted image 20240129131751.png]]
![[Pasted image 20240129131808.png]]

The gradient descent step can be treated as repulsion and attraction between points. 

>[!question] What are we updating in t-SNE gradient descent optimisation step???
>We are updating the y_i position, the datapoint around which we are estimating the distribution.
> ![[t-SNE-Gradient-Descent.gif]]

### t-SNE and CNN feature maps
t-SNE can be used when dealing with CNN feature maps, it helps us understand which input data seems similar from the n-layer features extracted for an image.