---
title: Short Summaries
tags:
date: "{{date}}"
---
current objective is simple, from every page of every research paper I read, take atleast one note.

Running Questions
- what is Emprical Risk Minimization?
- What are the current literature which is using diffusion classifier models for test-time-adaptation?
- 

In Search of Lost Domain Generalization
- model selection in domain generalization is non-trivial (significant) and domain generalization algorithms without model selection information are incomplete.
- models tend to easily depend on spurious features, rather than the causal features which are actually responsible.
- In domain generalization, we have access to multiple datasets, but all related to same task, but collected under different conditions/environments.
- Different domain generalization algorithms use different model selection criteria and dataset.
- A domain generalization algorithm should be responsible for specifying a model selection method.
- DOMAINBED framework
	- adding a new algorithm or dataset is few lines of code
	- a single command runs all experiments, performs all model selections, and autogenerates all tables used in this work.
- Domain Generalization problem
	- domain generalization is extension to supervised learning where we train our model to minimize loss(xi, f(xi)) where f is predictor which is composition of feature extractor model phi and classifier cl, where f = phi o cl.
	- In domain generalization, we have access to multiple datasets D_i, each of them are of different domain, and we need to do well on unseen domain.
- Model selection is also a learning problem
	- Training-domain validation set
	- Leave-one-domain-out cross validation
	- Test-domain validation set (oracle)

setup domain bed proper experimentation.
how do we calculate p(y|x) from class conditioned generative models?
how are class conditioned generative models, better compared generative+discriminative model?
how are generative+discriminative model different from self-supervised+discriminative model?
What are attention sinks? Read the two blogs on attention sinks?
hopfield network?

https://www.coursera.org/learn/computational-neuroscience - feels like a good course.

