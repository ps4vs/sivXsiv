---
title: "RL: Alpha"
tags:
  - sapling
enableToc: false
---
### Introduction

* Reinforcement Learning (RL) is a computational approach uniquely focused on goal-directed agents interacting with an uncertain environment, ie, more towards AGI. It is **"learning from interaction"** featuring "trail and error" and "delayed reward".

* The four main subelements of RL are the Policy, Reward Signal, Value Function, and Model:
	- **Policy**: The agent's strategy of action at a given time.
	- **Reward Signal**: An indicator of immediate success.
	- **Value Function**: A measure of long-term success.
	- **Model**: The agent's internal representation of the environment, aiding in planning and predicting future outcomes.
### **Flip!!!** 

- According to [Reinforcement Learning, Fast and Slow](https://www.cell.com/action/showPdf?pii=S1364-6613%2819%2930061-0), Machine Learning methods are ***sample inefficient*** for given reasons and factors

	1. **Incremental parameter adjustment**
		- The adjustments made during learning must be small, in order to maximize generalization and avoid overwriting the effects of earlier learning
	2. **Weak inductive bias**
		- According to learning theory every learning procedure necessarily faces bias-variance tradeoff. 
		- Generic neural networks are extremely low-bias learning systems, ie, they will be able to master a wider range of patterns (higher variance) but will in general be less sample-efficient.

- Furthermore, RL's ***sample inefficiency*** is exacerbated as the agent must self-determine labels (i.e., actions leading to higher rewards) through environmental interaction, unlike in supervised learning where labeled data is readily available.

### Tic Tac Toe Case Study

- In a practical application, I used RL to train an agent to play Tic-Tac-Toe using the Temporal Difference Method. [The code for RL agent learning to play Tic-Tac-Toe using Temporal Difference Method.](https://github.com/ps4vs/Deep-RL/blob/main/Chapter-1/TicTacToe.ipynb)

- Key insights include:
	1. **Adaptive Strategy**: The agent learns to counter repetitive strategies, such as blocking repeated moves.
	2. **Learning and Unlearning**: The agent's learning is dynamic, influenced by the nature of temporal updates (positive or negative).
	3. **Challenge with Random Play**: Learning efficiency drops significantly against a randomly moving opponent due to limited informative feedback.
	4. **Learning Through Self-Play**: When the RL agent plays Tic-Tac-Toe against itself, it experiences a unique learning environment. Since both players (first and second) are the same agent, they update value functions of different sets of states based on their position in the game.

More detailed/rough notes can be found [here](https://github.com/ps4vs/Deep-RL/tree/main/Chapter-1).