---
title: "RL: Alpha"
tags:
  - sapling
enableToc: false
---
### Introduction
- The most important feature distinguishing reinforcement learning from other types of learning is that it uses training information that **evaluates** the actions taken rather than **instructs** by giving correct actions. This is what creates the need for active exploration, for an explicit search for good behavior.
* Why exploration is needed?
	- We won't be able to find optimal policy during exploitation since we are getting evaluation feedback alone.

The two main aspects of RL are
* **evaluative feedback***, ie, how good the action taken was, but not whether it was the best or the worst action possible.
- **associative property**, ie, the best action depends on the situation. The topic of K-armed Bandit problem settings helps understand non-associative, evaluative feedback aspect of RL.


### K-armed Bandit Problem
- "Learning Action-value estimates", ie, Action-value Methods
	- $\epsilon$-greedy
		- Incremental update to estimate the value associated with each action
		- The $\epsilon$ is the exploration probability
		- $NewEstimate \leftarrow  OldEstimate + StepSize*[Target-OldEstimate]$
		- Having **decreasing StepSize for stationary reward distribution** allows convergence of the Estimates, whereas **constant StepSize is used for non-stationary reward distribution**.
	- Optimistic Initial Values
		- For stationary reward distribution problem, setting optimistic initial values helps exploration and faster convergence.
		- For non-stationary reward distribution, it doesn't affect since the mean of targets are constantly changes
	- Upper-Confidence-Bound Action Selection
		- allows exploration, with preference to actions, ie, based on how close their estimates are to being maximal and the uncertainity in their estimates.
		   $$A_t = argmax_a[Q_t(a)+c\sqrt(ln(t)/N_t(a))]$$
		- If $N_t(a)=0$ then a is selected first.
- "Learning a Numerical Preference" is an alternative to methods that estimate action values.
	- Gradient Bandit Algorithm
		- The numerical preference has no interpretation in terms of rewards, in contrast with estimated action values. (ie, which is estimated mean reward we get)
		   $$Pr(A_t=a) = e^{H_t(a)}/{\Sigma_b e^{H_t(b)}} = \pi(a)$$
		- Here $\pi(a)$ is the probability of taking action a at time t. 
		- Learning algorithm for soft-max action preferences based on the idea of stochastic gradient ascent.
			- $H_{t+1}(A_t) = H_t(A_t) + \alpha * (R_t - \bar{R_t}) (1 - \pi_t(A_t))$
			- $H_{t+1}(a) = H_t(a) - \alpha * (R_t - \bar{R_t})* \pi_t(A_t)\text{   ........}     \forall a!=A_t$ 


The implementation related to the above algorithms can be found [here](https://github.com/ps4vs/Deep-RL/tree/main/Chapter-2). along with more detailed/rough notes.

### Bandits vs Contextual Bandits vs Full RL problems

- Associative search tasks (contextual bandits) are intermediate b/w the k-armed bandit problem and full-reinforcement learning problem. 
- They are like full-reinforcement learning problem in that they involve learning a policy, but they are also like our k-armed bandit problem in that each action only affects the immediate reward. 
- If actions are allowed to affect the next situation as well as the reward, then we have full-reinforcement learning problem

