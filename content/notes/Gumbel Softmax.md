---
title: Gumbel Softmax
tags:
  - sapling
enableToc: false
---
It is used to estimate uniform distribution inside the model during back-propagation.

During the training of VAE, along with the decoder in two different settings
- Gaussian Distribution
	- The encoder predicts the gaussian distribution parameters $\mu \text{ and } \Sigma$
	- Sample from this gaussian distribution is drawn Z.
	- This Z is passed to the decoder to reconstruct the image
	- During Back-propagation, Z can be re-parameterised as Z = $\mu + \Sigma*\epsilon$
	- This re-parameterisation trick allows the gradients to flow back from decoder to the encoder.
- Categorical Distribution
	- If we are using categorical distribution, and we have some codebook for which we the encoder create the probabilities.
	- Sample Z from the categorical distribution.
	- Since, we can't write Z using values of probabilities of categorical distribution like re-parameterisation trick used in gaussian distribution case. 
	- No gradients can back-propagate from decoder to the encoder.  

To allow sampling with differentiation for categorical distribution case, Gumbel-max trick is used.
But Gumbel-max still has arg-max, to allow gradients to flow-back through arg-max, arg-max is estimated using Softmax along with temperature parameter `t`, ie, softmax on [p1/t, p2/t, p3/t, ....] 

The temperature is slowly decreased to zero, and when temperature tends to zero, softmax behaves like arg-max.


