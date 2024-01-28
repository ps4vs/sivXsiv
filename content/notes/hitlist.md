---
title: Hitlist
tags:
  - seed
---
![robotics](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExdXVtbmV4ajVyYzBsNjNybmZ3M21lcTh0bTB0MHdnZmVibWx3eW15ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/TJyHZPUF4jNZRpbWkK/source.gif)

### **Somethings I love to-do!!!**
###### Reading
* [https://distill.pub/](https://distill.pub/) Indepth explainations of multiple areas though outdated 2021.
###### Robotics
* Create an automatic drone from scratch using MIT's [Liquid Neural Networks code ](https://github.com/makramchahine/drone_causality)repository, and https://hackaday.io/ 's resources.
###### Research 
- Benchmarking recent sequential models, including Mamba, Transformer, ResNet, RetNet, RNN, Liquid Neural Networks, and Neural ODEs in ["Scalable-L20"](https://github.com/VITA-Group/Scalable-L2O)
- Hyper-networks to include sleep like replay buffer to aid continual learning.
- Using [Gumbel-Softmax Trick](https://blog.evjang.com/2016/11/tutorial-categorical-variational.html) to increase fairness in models. 
	- probabilistic method to increase fairness
	- auto-encoder is allowed to learn only useful features
		- learns initial encoder and decoder normally
		- now we train the encoder to produce different encodings and respective probabilities
		- task of decoder is to train the model to increase probabilties associated with representations which are re-contructed with fairness.
			- The decoder weights are kept constant, we will ask the decoder to project back the representations to image
			- and these images are calculated for fariness.
		- training can be done, to take images which contain more male, and less female image
		- allowing the model to sample male and female equally or something like that.
- Replacing grouped convolution with hyper-networks based grouped convolution
	- hyper-network will produce weights for grouped convolution give image encoding and group id. 
	- This can be implemented on AlexNet where grouped convolution was first used
	- Wav2Vec 2.0 also uses grouped convolution.
- Inspiration from multiple cooperative agents to grouped convolution tasks or convolutions filters in general.
- add LNN, and neuralODE to hugging-face
- add learned optimizer to huggingface



